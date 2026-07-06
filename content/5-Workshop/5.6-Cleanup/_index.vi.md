---
title : "Dọn dẹp tài nguyên"
date : 2026-04-12
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tổng kết workshop

Xin chúc mừng bạn đã hoàn thành workshop **"Xây dựng Chatbot Hỏi-Đáp với Amazon Bedrock, Knowledge Base & RAG"**!

Trong workshop này, bạn đã học & thực hành:
* Tạo **OpenSearch Serverless** collection + vector index cho RAG
* Tạo **Bedrock Knowledge Base** kết nối S3 + Titan Embeddings v2
* Cấu hình **chunking strategy** (default/hierarchical/semantic)
* **Sync tự động** tài liệu lên Knowledge Base
* Xây dựng **Lambda function** gọi `RetrieveAndGenerate` API
* Triển khai **API Gateway** REST endpoint
* Xây dựng **React frontend** chat UI trên S3 + CloudFront
* Áp dụng **Bedrock Guardrails** cho Responsible AI
* **Monitoring** với CloudWatch + alarm

#### Chi phí ước tính

Trong workshop này, chi phí chính đến từ:
* **OpenSearch Serverless:** ~$0.24/OCU/giờ cho Search OCU + $0.24/OCU/giờ cho Indexing OCU. Với 1 OCU mỗi loại, khoảng **$12/ngày** nếu để chạy liên tục.
* **Bedrock:** Claude 3.5 Sonnet ~$3/1M input tokens, $15/1M output tokens. Vài chục câu hỏi thường < $1.
* **Lambda, API Gateway, S3, CloudWatch:** nằm trong Free Tier hoặc rất nhỏ (vài cents).
* **Tổng workshop 1 ngày:** ~$13 (chủ yếu từ OpenSearch).

> ⚠️ **Quan trọng:** Nếu bạn chỉ demo thử và không dùng nữa, hãy **dọn dẹp ngay** trong ngày để tránh phát sinh chi phí không cần thiết.

---

#### Dọn dẹp tài nguyên

##### Bước 1: Xoá CloudFront distribution (nếu có)

```bash
# Lấy Distribution ID
DIST_ID=$(aws cloudfront list-distributions \
  --query "DistributionList.Items[?Comment=='fcaj-chat-ui'].Id" \
  --output text)

# Disable distribution trước, đợi status Deployed (~5-10 phút)
aws cloudfront get-distribution-config --id $DIST_ID > /tmp/cf-config.json
# Sửa file /tmp/cf-config.json: Enabled = false
aws cloudfront update-distribution --id $DIST_ID --if-match <ETag> \
  --distribution-config file:///tmp/cf-config.json

# Sau khi deployed, xoá
aws cloudfront delete-distribution --id $DIST_ID --if-match <ETag>
```

##### Bước 2: Xoá S3 bucket frontend & documents

```bash
# Empty và xoá bucket frontend
aws s3 rm s3://fcaj-chat-ui-<your-id>/ --recursive
aws s3 rb s3://fcaj-chat-ui-<your-id>/

# Empty và xoá bucket documents
aws s3 rm s3://fcaj-bedrock-docs-<your-id>/ --recursive
aws s3 rb s3://fcaj-bedrock-docs-<your-id>/
```

##### Bước 3: Xoá API Gateway

```bash
API_ID=$(aws apigateway get-rest-apis \
  --query "items[?name=='fcaj-chat-api'].id" \
  --output text)
aws apigateway delete-rest-api --rest-api-id $API_ID --region ap-southeast-1
```

##### Bước 4: Xoá Lambda function

```bash
aws lambda delete-function \
  --function-name fcaj-chat-handler \
  --region ap-southeast-1
```

##### Bước 5: Xoá Knowledge Base & Data Source

```bash
# Lấy KB_ID
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)

# Xoá từng data source trước
DS_IDS=$(aws bedrock-agent list-data-sources \
  --knowledge-base-id $KB_ID \
  --query "dataSourceSummaries[].dataSourceId" \
  --output text --region ap-southeast-1)

for DS_ID in $DS_IDS; do
  aws bedrock-agent delete-data-source \
    --knowledge-base-id $KB_ID \
    --data-source-id $DS_ID \
    --region ap-southeast-1
done

# Xoá Knowledge Base
aws bedrock-agent delete-knowledge-base \
  --knowledge-base-id $KB_ID \
  --region ap-southeast-1
```

##### Bước 6: Xoá OpenSearch Serverless collection

```bash
COLLECTION_ID=$(aws opensearchserverless list-collections \
  --query "collectionSummaries[?name=='kb-vector-collection'].id" \
  --output text --region ap-southeast-1)

aws opensearchserverless delete-collection \
  --id $COLLECTION_ID \
  --region ap-southeast-1
```

Lưu ý: OpenSearch collection mất ~5-10 phút để xoá hoàn toàn. Có thể bị charge thêm OCU trong thời gian này.

##### Bước 7: Xoá Guardrail

```bash
GUARDRAIL_ID=$(aws bedrock list-guardrails \
  --query "guardrails[?name=='fcaj-workshop-guardrail'].id" \
  --output text --region ap-southeast-1)

aws bedrock delete-guardrail \
  --guardrail-identifier $GUARDRAIL_ID \
  --region ap-southeast-1
```

##### Bước 8: Xoá IAM roles & CloudWatch log groups

```bash
# Xoá Lambda role
aws iam detach-role-policy --role-name lambda-bedrock-exec \
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
aws iam delete-role-policy --role-name lambda-bedrock-exec \
  --policy-name bedrock-access
aws iam delete-role --role-name lambda-bedrock-exec

# Xoá KB service role
aws iam detach-role-policy --role-name AmazonBedrockExecutionRoleForKnowledgeBase_fcaj-workshop-kb \
  --policy-arn <policy-arn>
aws iam delete-role --role-name AmazonBedrockExecutionRoleForKnowledgeBase_fcaj-workshop-kb

# Xoá CloudWatch log groups
aws logs delete-log-group --log-group-name /aws/lambda/fcaj-chat-handler
aws logs delete-log-group --log-group-name /aws/bedrock/knowledgebase/$KB_ID
```

##### Bước 9: Xoá SNS topic (nếu có)

```bash
aws sns delete-topic --topic-arn arn:aws:sns:ap-southeast-1:<account-id>:bedrock-alerts
```

##### Bước 10: Verify cleanup

```bash
# Kiểm tra còn resource nào sót không
aws resourcegroupstaggingapi get-resources \
  --tag-filters Key=Project,Values=fcaj-workshop \
  --region ap-southeast-1

# Kiểm tra chi phí ước tính
aws ce get-cost-and-usage \
  --time-period Start=2026-04-12,End=2026-04-19 \
  --granularity DAILY \
  --metrics "UnblendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE \
  --region us-east-1
```

#### Best practice: dùng script tự động dọn dẹp

Lưu script `cleanup.sh` để chạy khi cần:

```bash
#!/bin/bash
set -e
REGION="ap-southeast-1"

echo "🧹 Cleaning up FCAJ workshop resources..."

# 1. CloudFront
DIST_ID=$(aws cloudfront list-distributions --query "DistributionList.Items[?Comment=='fcaj-chat-ui'].Id" --output text)
if [ -n "$DIST_ID" ]; then
  echo "  - Disabling CloudFront $DIST_ID"
  aws cloudfront get-distribution-config --id $DIST_ID > /tmp/cf.json
  sed -i 's/"Enabled": true/"Enabled": false/' /tmp/cf.json
  ETAG=$(jq -r '.ETag' /tmp/cf.json)
  aws cloudfront update-distribution --id $DIST_ID --if-match $ETAG --distribution-config file:///tmp/cf.json
  echo "  - Wait for CloudFront deploy (~5 min), then run delete"
fi

# 2. S3
for BUCKET in fcaj-chat-ui-<your-id> fcaj-bedrock-docs-<your-id>; do
  if aws s3 ls s3://$BUCKET 2>/dev/null; then
    echo "  - Removing S3 bucket $BUCKET"
    aws s3 rm s3://$BUCKET/ --recursive
    aws s3 rb s3://$BUCKET/
  fi
done

# 3. API Gateway
API_ID=$(aws apigateway get-rest-apis --query "items[?name=='fcaj-chat-api'].id" --output text --region $REGION)
[ -n "$API_ID" ] && aws apigateway delete-rest-api --rest-api-id $API_ID --region $REGION

# 4. Lambda
aws lambda delete-function --function-name fcaj-chat-handler --region $REGION 2>/dev/null || true

# 5. Bedrock KB
KB_ID=$(aws bedrock-agent list-knowledge-bases --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" --output text --region $REGION)
if [ -n "$KB_ID" ]; then
  for DS_ID in $(aws bedrock-agent list-data-sources --knowledge-base-id $KB_ID --query "dataSourceSummaries[].dataSourceId" --output text --region $REGION); do
    aws bedrock-agent delete-data-source --knowledge-base-id $KB_ID --data-source-id $DS_ID --region $REGION
  done
  aws bedrock-agent delete-knowledge-base --knowledge-base-id $KB_ID --region $REGION
fi

# 6. OpenSearch
COLLECTION_ID=$(aws opensearchserverless list-collections --query "collectionSummaries[?name=='kb-vector-collection'].id" --output text --region $REGION)
[ -n "$COLLECTION_ID" ] && aws opensearchserverless delete-collection --id $COLLECTION_ID --region $REGION

# 7. Guardrail
GUARDRAIL_ID=$(aws bedrock list-guardrails --query "guardrails[?name=='fcaj-workshop-guardrail'].id" --output text --region $REGION)
[ -n "$GUARDRAIL_ID" ] && aws bedrock delete-guardrail --guardrail-identifier $GUARDRAIL_ID --region $REGION

# 8. CloudWatch log groups
aws logs delete-log-group --log-group-name /aws/lambda/fcaj-chat-handler 2>/dev/null || true

echo "✅ Cleanup complete! Don't forget to delete the CloudFront distribution after it disables."
```

Chạy script:

```bash
chmod +x cleanup.sh
./cleanup.sh
```

#### Cảm ơn bạn đã hoàn thành workshop!

Nếu bạn muốn tìm hiểu thêm:
* [Bedrock Workshop Collection](https://github.com/aws-samples/amazon-bedrock-workshops)
* [Generative AI on AWS](https://aws.amazon.com/ai/generative-ai/)
* [RAG Best Practices](https://docs.aws.amazon.com/prescriptive-guidance/latest/retrieval-augmented-generation-options/rag-architecture.html)

#### Tài liệu tham khảo
* [AWS OpenSearch Serverless Pricing](https://aws.amazon.com/opensearch-service/pricing/)
* [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/)
* [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
* [Tagging AWS Resources](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html)