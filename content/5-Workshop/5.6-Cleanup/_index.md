---
title : "Clean up resources"
date : 2026-04-12
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Workshop summary

Congratulations on completing the **"Build a Q&A Chatbot with Amazon Bedrock, Knowledge Base & RAG"** workshop!

In this workshop you have learned and practised:
* Creating an **OpenSearch Serverless** collection + vector index for RAG
* Building a **Bedrock Knowledge Base** wired to S3 + Titan Embeddings v2
* Configuring the **chunking strategy** (default / hierarchical / semantic)
* **Auto-syncing** documents into the Knowledge Base
* Building a **Lambda function** calling the `RetrieveAndGenerate` API
* Deploying an **API Gateway** REST endpoint
* Building a **React frontend** chat UI on S3 + CloudFront
* Applying **Bedrock Guardrails** for Responsible AI
* **Monitoring** with CloudWatch + alarms

#### Estimated cost

In this workshop, the main costs come from:
* **OpenSearch Serverless:** ~$0.24/OCU/hour for Search OCU + $0.24/OCU/hour for Indexing OCU. With 1 OCU each, about **$12/day** if kept running.
* **Bedrock:** Claude 3.5 Sonnet ~$3/1M input tokens, $15/1M output tokens. A few dozen questions usually cost < $1.
* **Lambda, API Gateway, S3, CloudWatch:** within Free Tier or just a few cents.
* **Total for a 1-day workshop:** ~$13 (mostly from OpenSearch).

> ⚠️ **Important:** If you only demoed and no longer need it, **clean up the same day** to avoid unnecessary charges.

---

#### Cleaning up

##### Step 1: Delete the CloudFront distribution (if any)

```bash
# Get the distribution ID
DIST_ID=$(aws cloudfront list-distributions \
  --query "DistributionList.Items[?Comment=='fcaj-chat-ui'].Id" \
  --output text)

# Disable the distribution first, wait for status Deployed (~5-10 minutes)
aws cloudfront get-distribution-config --id $DIST_ID > /tmp/cf-config.json
# Edit /tmp/cf-config.json: Enabled = false
aws cloudfront update-distribution --id $DIST_ID --if-match <ETag> \
  --distribution-config file:///tmp/cf-config.json

# After deployment, delete
aws cloudfront delete-distribution --id $DIST_ID --if-match <ETag>
```

##### Step 2: Delete the frontend & documents S3 buckets

```bash
# Empty and delete the frontend bucket
aws s3 rm s3://fcaj-chat-ui-<your-id>/ --recursive
aws s3 rb s3://fcaj-chat-ui-<your-id>/

# Empty and delete the documents bucket
aws s3 rm s3://fcaj-bedrock-docs-<your-id>/ --recursive
aws s3 rb s3://fcaj-bedrock-docs-<your-id>/
```

##### Step 3: Delete API Gateway

```bash
API_ID=$(aws apigateway get-rest-apis \
  --query "items[?name=='fcaj-chat-api'].id" \
  --output text)
aws apigateway delete-rest-api --rest-api-id $API_ID --region ap-southeast-1
```

##### Step 4: Delete the Lambda function

```bash
aws lambda delete-function \
  --function-name fcaj-chat-handler \
  --region ap-southeast-1
```

##### Step 5: Delete the Knowledge Base & Data Source

```bash
# Get KB_ID
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)

# Delete each data source first
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

# Delete the Knowledge Base
aws bedrock-agent delete-knowledge-base \
  --knowledge-base-id $KB_ID \
  --region ap-southeast-1
```

##### Step 6: Delete the OpenSearch Serverless collection

```bash
COLLECTION_ID=$(aws opensearchserverless list-collections \
  --query "collectionSummaries[?name=='kb-vector-collection'].id" \
  --output text --region ap-southeast-1)

aws opensearchserverless delete-collection \
  --id $COLLECTION_ID \
  --region ap-southeast-1
```

Note: OpenSearch collection takes ~5-10 minutes to fully delete. You may be charged OCU during this period.

##### Step 7: Delete the Guardrail

```bash
GUARDRAIL_ID=$(aws bedrock list-guardrails \
  --query "guardrails[?name=='fcaj-workshop-guardrail'].id" \
  --output text --region ap-southeast-1)

aws bedrock delete-guardrail \
  --guardrail-identifier $GUARDRAIL_ID \
  --region ap-southeast-1
```

##### Step 8: Delete IAM roles & CloudWatch log groups

```bash
# Lambda role
aws iam detach-role-policy --role-name lambda-bedrock-exec \
  --policy-arn arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
aws iam delete-role-policy --role-name lambda-bedrock-exec \
  --policy-name bedrock-access
aws iam delete-role --role-name lambda-bedrock-exec

# KB service role
aws iam detach-role-policy --role-name AmazonBedrockExecutionRoleForKnowledgeBase_fcaj-workshop-kb \
  --policy-arn <policy-arn>
aws iam delete-role --role-name AmazonBedrockExecutionRoleForKnowledgeBase_fcaj-workshop-kb

# CloudWatch log groups
aws logs delete-log-group --log-group-name /aws/lambda/fcaj-chat-handler
aws logs delete-log-group --log-group-name /aws/bedrock/knowledgebase/$KB_ID
```

##### Step 9: Delete SNS topic (if any)

```bash
aws sns delete-topic --topic-arn arn:aws:sns:ap-southeast-1:<account-id>:bedrock-alerts
```

##### Step 10: Verify cleanup

```bash
# Check for any leftover resources
aws resourcegroupstaggingapi get-resources \
  --tag-filters Key=Project,Values=fcaj-workshop \
  --region ap-southeast-1

# Check estimated cost
aws ce get-cost-and-usage \
  --time-period Start=2026-04-12,End=2026-04-19 \
  --granularity DAILY \
  --metrics "UnblendedCost" \
  --group-by Type=DIMENSION,Key=SERVICE \
  --region us-east-1
```

#### Best practice: use an automatic cleanup script

Save this as `cleanup.sh` to run whenever needed:

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

Run the script:

```bash
chmod +x cleanup.sh
./cleanup.sh
```

#### Thank you for completing the workshop!

If you want to explore more:
* [Bedrock Workshop Collection](https://github.com/aws-samples/amazon-bedrock-workshops)
* [Generative AI on AWS](https://aws.amazon.com/ai/generative-ai/)
* [RAG Best Practices](https://docs.aws.amazon.com/prescriptive-guidance/latest/retrieval-augmented-generation-options/rag-architecture.html)

#### References
* [AWS OpenSearch Serverless Pricing](https://aws.amazon.com/opensearch-service/pricing/)
* [Amazon Bedrock Pricing](https://aws.amazon.com/bedrock/pricing/)
* [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)
* [Tagging AWS Resources](https://docs.aws.amazon.com/tag-editor/latest/userguide/tagging.html)