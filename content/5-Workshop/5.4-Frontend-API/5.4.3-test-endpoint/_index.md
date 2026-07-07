---
title : "Test endpoint with curl / Postman"
date : 2026-04-12
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

In this section you will validate the full end-to-end pipeline by sending a question to API Gateway and reading the response returned from the Bedrock Knowledge Base.

#### 1. Quick test with curl

Replace `<API_ID>` with the API Gateway ID you obtained in section 5.4.2:

```bash
API_URL="https://<API_ID>.execute-api.ap-southeast-1.amazonaws.com/prod/chat"

curl -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "What is AWS Lambda?"}'
```

Sample response (Bedrock answer + citations extracted from KB documents):

```json
{
  "answer": "AWS Lambda is a serverless compute service from AWS that lets you run code without managing servers. Lambda automatically scales with traffic and you only pay for the compute time actually used...",
  "citations": [
    {
      "uri": "s3://fcaj-bedrock-docs-xxx/aws-overview.pdf",
      "title": "s3://fcaj-bedrock-docs-xxx/aws-overview.pdf"
    },
    {
      "uri": "s3://fcaj-bedrock-docs-xxx/lambda-faq.md",
      "title": "s3://fcaj-bedrock-docs-xxx/lambda-faq.md"
    }
  ]
}
```

If you see `answer` and `citations` ⇒ the pipeline is working.

#### 2. Test additional queries

Try 4 types of queries to verify the RAG pipeline handles them correctly:

```bash
# (a) Question in Knowledge Base → answer + citation
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "What S3 storage classes are available?"}' | jq .

# (b) Question outside KB → answer + "not found" disclaimer
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "What is the weather in Tokyo today?"}' | jq .

# (c) Vietnamese query (verify multilingual)
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "Amazon Bedrock là gì?"}' | jq .

# (d) Empty question → 400 error
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": ""}' | jq .
```

**Expected results:**

| # | Question | Result |
|---|---|---|
| (a) | "What S3 storage classes are available?" | 200 + long answer + 1-3 citations from KB |
| (b) | "What is the weather in Tokyo?" | 200 + short answer like "I couldn't find..." + 0 citations |
| (c) | "Amazon Bedrock là gì?" | 200 + Vietnamese answer + citations |
| (d) | "" | 400 + `{"error": "Missing 'question' field"}` |

#### 3. Test with Postman (optional)

If you prefer a GUI:

1. Open Postman → **New → HTTP Request**.
2. Method: `POST`, URL: `$API_URL`.
3. Tab **Headers** → add `Content-Type: application/json`.
4. Tab **Body** → choose **raw / JSON** → enter `{"question": "..."}`.
5. Click **Send** → the response shows in the lower pane.

![postman test](/images/5-Workshop/5.4-Frontend-API/postman.png)

#### 4. View logs to debug

If the response errors out, check the Lambda CloudWatch Logs immediately:

```bash
aws logs tail /aws/lambda/fcaj-chat-handler --follow --region ap-southeast-1
```

Or open **CloudWatch console → Log groups → /aws/lambda/fcaj-chat-handler → pick the newest log stream**.

Common errors:

| Error | Root cause | Fix |
|---|---|---|
| `AccessDeniedException` | IAM role missing `bedrock:RetrieveAndGenerate` | Review the policy from 5.4.1 |
| `ResourceNotFoundException` | Wrong `KB_ID` or KB not ACTIVE | Verify `aws bedrock-agent get-knowledge-base` |
| `ThrottlingException` | Too many requests | Wait a few seconds and retry |
| Lambda timeout (30s) | KB too large, slow retrieval | Increase timeout or lower top-K |

![cloudwatch logs](/images/5-Workshop/5.4-Frontend-API/cloudwatch.png)

#### 5. Test directly via Lambda console

Open **Lambda console → fcaj-chat-handler → Test tab**:

```json
{
  "question": "What is AWS Lambda?"
}
```

Click **Test** → response appears in **Execution result**. This bypasses API Gateway and is useful to debug Lambda before debugging API Gateway.

#### Summary

After this section you have verified:
* Lambda + Bedrock KB + API Gateway work correctly.
* The RAG pipeline retrieves + generates answers with proper citations.
* Four cases are handled: KB hit / KB miss / multilingual / empty input.

In the next section (5.4.4) you will **build & deploy the React frontend** on S3 + CloudFront so users have a polished chat UI.

#### References
* [Test API Gateway with curl](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-test-method.html)
* [Bedrock KB error handling](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-error.html)
* [CloudWatch Logs Insights query](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)