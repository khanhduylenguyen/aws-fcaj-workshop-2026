---
title : "Tạo Lambda function & API Gateway endpoint"
date : 2026-04-12
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

Trong phần này bạn sẽ:

1. Viết code **Lambda handler** (Python 3.12) gọi `RetrieveAndGenerate`.
2. **Deploy Lambda** với env vars trỏ tới Knowledge Base.
3. Tạo **API Gateway REST API** expose endpoint `POST /chat`.

#### 1. Viết Lambda handler

Tạo file `lambda/lambda_function.py`:

```python
import json
import os
import logging
import boto3
from botocore.exceptions import ClientError

logger = logging.getLogger()
logger.setLevel(logging.INFO)

bedrock_agent_runtime = boto3.client(
    "bedrock-agent-runtime",
    region_name=os.environ.get("REGION", "ap-southeast-1"),
)

KB_ID = os.environ["KB_ID"]
MODEL_ARN = os.environ["MODEL_ARN"]


def lambda_handler(event, context):
    try:
        # Parse body (API Gateway wraps the request in `body`)
        if isinstance(event.get("body"), str):
            body = json.loads(event["body"])
        else:
            body = event

        question = (body.get("question") or "").strip()
        if not question:
            return _resp(400, {"error": "Missing 'question' field"})

        # Gọi Bedrock KB RetrieveAndGenerate
        response = bedrock_agent_runtime.retrieve_and_generate(
            input={"text": question},
            retrieveAndGenerateConfiguration={
                "type": "KNOWLEDGE_BASE",
                "knowledgeBaseConfiguration": {
                    "knowledgeBaseId": KB_ID,
                    "modelArn": MODEL_ARN,
                    "generationConfiguration": {
                        "inferenceConfig": {
                            "textInferenceConfig": {
                                "maxTokens": 1024,
                                "temperature": 0.3,
                                "topP": 0.9,
                            }
                        }
                    },
                },
            },
        )

        answer = response["output"]["text"]

        # Trích xuất citations (tối đa 5 nguồn unique)
        citations = []
        for cite in response.get("citations", []):
            for ref in cite.get("retrievedReferences", []):
                loc = ref.get("location", {}).get("s3Location", {})
                citations.append({
                    "uri":   loc.get("uri", ""),
                    "title": ref.get("metadata", {}).get("x-amz-bedrock-kb-source-uri", ""),
                })

        # dedup
        seen, uniq = set(), []
        for c in citations:
            if c["uri"] and c["uri"] not in seen:
                seen.add(c["uri"])
                uniq.append(c)

        logger.info(f"Question: {question} | Citations: {len(uniq)}")

        return _resp(200, {
            "answer":    answer,
            "citations": uniq[:5],
        })

    except ClientError as e:
        logger.error(f"Bedrock error: {e}")
        return _resp(500, {"error": str(e)})
    except Exception as e:
        logger.exception("Unhandled error")
        return _resp(500, {"error": str(e)})


def _resp(code, body):
    return {
        "statusCode": code,
        "headers": {
            "Content-Type":                 "application/json",
            "Access-Control-Allow-Origin":  "*",
            "Access-Control-Allow-Headers": "Content-Type",
            "Access-Control-Allow-Methods": "OPTIONS,POST",
        },
        "body": json.dumps(body, ensure_ascii=False),
    }
```

#### 2. Đóng gói & deploy Lambda

```bash
cd ~/fcaj-chat-app/lambda

# Đóng gói code thành .zip
zip -r ../chat-handler.zip lambda_function.py

# Lấy ARN của role đã tạo ở 5.4.1
ROLE_ARN=$(aws iam get-role \
  --role-name fcaj-chat-handler-role \
  --query 'Role.Arn' --output text)

# Tạo Lambda function
aws lambda create-function \
  --function-name fcaj-chat-handler \
  --runtime python3.12 \
  --role "$ROLE_ARN" \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://../chat-handler.zip \
  --timeout 30 \
  --memory-size 256 \
  --region ap-southeast-1
```

Cấu hình **environment variables** trỏ tới Knowledge Base:

```bash
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)

aws lambda update-function-configuration \
  --function-name fcaj-chat-handler \
  --environment "Variables={KB_ID=${KB_ID},MODEL_ARN=arn:aws:bedrock:ap-southeast-1::foundation-model/anthropic.claude-3-5-sonnet-20240620-v1:0,REGION=ap-southeast-1}" \
  --region ap-southeast-1
```

Verify Lambda đã tạo:

```bash
aws lambda get-function \
  --function-name fcaj-chat-handler \
  --region ap-southeast-1 \
  --query 'Configuration.[FunctionName, Runtime, State]'
# Kỳ vọng: ["fcaj-chat-handler", "python3.12", "Active"]
```

![lambda created](/images/5-Workshop/5.4-Frontend-API/lambda-created.png)

#### 3. Tạo API Gateway REST API

```bash
# Tạo REST API
API_ID=$(aws apigateway create-rest-api \
  --name "fcaj-chat-api" \
  --endpoint-configuration types=REGIONAL \
  --query 'id' --output text --region ap-southeast-1)

echo "API_ID = $API_ID"

# Lấy root resource id
ROOT_ID=$(aws apigateway get-resources \
  --rest-api-id $API_ID \
  --query 'items[0].id' --output text --region ap-southeast-1)

# Tạo resource /chat
RES_ID=$(aws apigateway create-resource \
  --rest-api-id $API_ID \
  --parent-id $ROOT_ID \
  --path-part chat \
  --query 'id' --output text --region ap-southeast-1)

echo "RES_ID = $RES_ID"

# Lambda ARN
LAMBDA_ARN=$(aws lambda get-function \
  --function-name fcaj-chat-handler \
  --region ap-southeast-1 \
  --query 'Configuration.FunctionArn')

# Tạo POST method, integration với Lambda
aws apigateway put-method \
  --rest-api-id $API_ID \
  --resource-id $RES_ID \
  --http-method POST \
  --authorization-type NONE \
  --region ap-southeast-1

aws apigateway put-integration \
  --rest-api-id $API_ID \
  --resource-id $RES_ID \
  --http-method POST \
  --type AWS_PROXY \
  --integration-http-method POST \
  --uri "arn:aws:apigateway:ap-southeast-1:lambda:path/2015-03-31/functions/${LAMBDA_ARN}/invocations" \
  --region ap-southeast-1

# Thêm OPTIONS method để xử lý CORS preflight
aws apigateway put-method \
  --rest-api-id $API_ID \
  --resource-id $RES_ID \
  --http-method OPTIONS \
  --authorization-type NONE \
  --region ap-southeast-1

aws apigateway put-integration \
  --rest-api-id $API_ID \
  --resource-id $RES_ID \
  --http-method OPTIONS \
  --type MOCK \
  --request-templates '{"application/json":"{\"statusCode\":200}"}' \
  --region ap-southeast-1

aws apigateway put-method-response \
  --rest-api-id $API_ID \
  --resource-id $RES_ID \
  --http-method OPTIONS \
  --status-code 200 \
  --response-parameters "method.response.header.Access-Control-Allow-Headers=false,method.response.header.Access-Control-Allow-Methods=false,method.response.header.Access-Control-Allow-Origin=false" \
  --region ap-southeast-1

aws apigateway put-integration-response \
  --rest-api-id $API_ID \
  --resource-id $RES_ID \
  --http-method OPTIONS \
  --status-code 200 \
  --response-parameters "method.response.header.Access-Control-Allow-Headers=\"'Content-Type'\",method.response.header.Access-Control-Allow-Methods=\"'OPTIONS,POST'\",method.response.header.Access-Control-Allow-Origin=\"'*'\"" \
  --region ap-southeast-1

# Cấp quyền cho API Gateway invoke Lambda
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
aws lambda add-permission \
  --function-name fcaj-chat-handler \
  --statement-id apigateway-chat-invoke \
  --action lambda:InvokeFunction \
  --principal apigateway.amazonaws.com \
  --source-arn "arn:aws:execute-api:ap-southeast-1:${ACCOUNT_ID}:${API_ID}/*/POST/chat" \
  --region ap-southeast-1

# Deploy API
aws apigateway create-deployment \
  --rest-api-id $API_ID \
  --stage-name prod \
  --region ap-southeast-1
```

Sau khi deploy, copy **Invoke URL**:

```bash
echo "https://${API_ID}.execute-api.ap-southeast-1.amazonaws.com/prod/chat"
```

Lưu URL này — sẽ dùng cho phần 5.4.4 (Frontend) và test ở 5.4.3.

![api deployed](/images/5-Workshop/5.4-Frontend-API/api-deployed.png)

#### Tổng kết

Sau phần này bạn đã có:
* Lambda function `fcaj-chat-handler` gọi được Bedrock Knowledge Base.
* REST API endpoint `POST https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/chat`.

Phần tiếp theo (5.4.3) sẽ **test endpoint** này bằng `curl` hoặc Postman.

#### Tài liệu tham khảo
* [Bedrock RetrieveAndGenerate API](https://docs.aws.amazon.com/bedrock/latest/APIReference/API_agent-runtime_RetrieveAndGenerate.html)
* [Lambda deployment package (Python)](https://docs.aws.amazon.com/lambda/latest/dg/python-package.html)
* [API Gateway + Lambda proxy integration](https://docs.aws.amazon.com/apigateway/latest/developerguide/getting-started-with-lambda-integration.html)
* [Enable CORS on API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-cors.html)