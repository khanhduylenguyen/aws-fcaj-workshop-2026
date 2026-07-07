---
title : "Prepare IAM Role & environment"
date : 2026-04-12
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

In this section you will prepare:

* The **IAM Role** that lets Lambda call the Bedrock Knowledge Base.
* The **source code layout** for Lambda + Frontend.
* The **required IDs** (Knowledge Base ID, Model ARN) from section 5.3.

#### 1. Create the IAM Role for Lambda

Lambda needs an **execution role** to invoke the Bedrock Agent Runtime API.

Create the trust policy allowing Lambda to assume the role:

```bash
cat > trust-policy.json << 'EOF'
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "lambda.amazonaws.com" },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF

aws iam create-role \
  --role-name fcaj-chat-handler-role \
  --assume-role-policy-document file://trust-policy.json \
  --region ap-southeast-1
```

Create the permission policy that grants Bedrock + CloudWatch Logs access:

```bash
cat > bedrock-policy.json << 'EOF'
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "BedrockRetrieveAndGenerate",
      "Effect": "Allow",
      "Action": [
        "bedrock:RetrieveAndGenerate",
        "bedrock:Retrieve",
        "bedrock:InvokeModel"
      ],
      "Resource": "*"
    },
    {
      "Sid": "CloudWatchLogs",
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents"
      ],
      "Resource": "arn:aws:logs:ap-southeast-1:*:log-group:/aws/lambda/fcaj-chat-handler:*"
    }
  ]
}
EOF

aws iam create-policy \
  --policy-name fcaj-chat-handler-policy \
  --policy-document file://bedrock-policy.json \
  --region ap-southeast-1

aws iam attach-role-policy \
  --role-name fcaj-chat-handler-role \
  --policy-arn arn:aws:iam::<account-id>:policy/fcaj-chat-handler-policy
```

> 💡 Replace `<account-id>` with your AWS Account ID (run `aws sts get-caller-identity` to get it).

#### 2. Retrieve Knowledge Base ID and Model ARN

```bash
# Get the KB_ID from section 5.3
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)

echo "KB_ID = $KB_ID"

# Model ARN is fixed for Claude 3.5 Sonnet in Singapore
export MODEL_ARN="arn:aws:bedrock:ap-southeast-1::foundation-model/anthropic.claude-3-5-sonnet-20240620-v1:0"
export REGION="ap-southeast-1"
```

Save these 3 values for the next step (5.4.2 — create Lambda).

#### 3. Prepare source-code directories

```bash
mkdir -p ~/fcaj-chat-app
cd ~/fcaj-chat-app

# Sub-directories
mkdir -p lambda       # Lambda function code
mkdir -p frontend     # React SPA
```

#### 4. Verify environment

Run these 3 checks — all 3 must succeed:

```bash
# (a) IAM role created
aws iam get-role --role-name fcaj-chat-handler-role \
  --query 'Role.RoleName' --output text
# Expected: fcaj-chat-handler-role

# (b) Bedrock model access enabled
aws bedrock list-foundation-models --region ap-southeast-1 \
  --query 'modelSummaries[?contains(modelId, `claude-3-5`)].modelId'
# Expected: at least one Claude 3.5 Sonnet model

# (c) Knowledge Base ACTIVE
aws bedrock-agent get-knowledge-base \
  --knowledge-base-id $KB_ID \
  --region ap-southeast-1 \
  --query 'knowledgeBase.status'
# Expected: "ACTIVE"
```

![prepare complete](/images/5-Workshop/5.4-Frontend-API/prepare.png)

If all 3 checks pass, you are ready for [section 5.4.2 — Create Lambda function & API Gateway endpoint](../5.4.2-create-interface-enpoint/).

#### References
* [Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)
* [Bedrock Agent Runtime permissions](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-lambda.html)
* [ListKnowledgeBases API](https://docs.aws.amazon.com/bedrock-agent/latest/APIReference/API_ListKnowledgeBases.html)