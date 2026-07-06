---
title : "Prerequisites"
date : 2026-04-12
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### 1. AWS account & Region

In this workshop we will use the **Singapore (ap-southeast-1)** region. Please ensure that you:
* Are logged into the AWS Console with a user that has Administrator access (or the IAM policy below).
* Have **Singapore** selected in the top-right corner of the Console.

#### 2. Enable Model Access in Amazon Bedrock

Before you can invoke Claude or Titan, you need to **request model access**:

1. Open the Amazon Bedrock Console in the Singapore region.
2. Choose **Model access** in the left menu under **Bedrock configurations**.
3. Click **Manage model access** and select these models:
   * **Anthropic:** Claude 3.5 Sonnet, Claude 3 Haiku
   * **Amazon:** Titan Embeddings v2, Titan Text G1 - Express
   * (optional) **Meta:** Llama 3 8B Instruct
4. Click **Request model access** and wait a few minutes for AWS to approve.

![model access](/images/5-Workshop/5.2-Prerequisite/model-access.png)

#### 3. Create an IAM policy for the workshop user

Attach the following policy to your IAM user (or simply use AdministratorAccess for the workshop):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "BedrockWorkshop",
      "Effect": "Allow",
      "Action": [
        "bedrock:*",
        "s3:CreateBucket",
        "s3:DeleteBucket",
        "s3:GetObject",
        "s3:GetObjectVersion",
        "s3:ListBucket",
        "s3:PutObject",
        "s3:DeleteObject",
        "s3:GetBucketPolicy",
        "s3:PutBucketPolicy",
        "s3:GetBucketCORS",
        "s3:PutBucketCORS",
        "aoss:*",
        "iam:CreateRole",
        "iam:DeleteRole",
        "iam:AttachRolePolicy",
        "iam:DetachRolePolicy",
        "iam:PassRole",
        "iam:CreatePolicy",
        "iam:DeletePolicy",
        "iam:GetRole",
        "iam:GetPolicy",
        "lambda:CreateFunction",
        "lambda:DeleteFunction",
        "lambda:GetFunction",
        "lambda:InvokeFunction",
        "lambda:UpdateFunctionCode",
        "lambda:UpdateFunctionConfiguration",
        "apigateway:*",
        "cloudfront:*",
        "logs:CreateLogGroup",
        "logs:DeleteLogGroup",
        "logs:DescribeLogGroups",
        "logs:PutRetentionPolicy",
        "cloudwatch:GetMetricData",
        "cloudwatch:GetMetricStatistics",
        "cloudwatch:ListMetrics",
        "cloudwatch:DescribeAlarms"
      ],
      "Resource": "*"
    }
  ]
}
```

> 💡 **Note:** Most of the cost is in Bedrock (per-token). A single Claude 3.5 Sonnet query is around $0.003 input + $0.015 output per 1K tokens. A few dozen queries during the workshop cost less than $1. OpenSearch Serverless has a minimum charge of $0.24/OCU/hour — remember to clean up after the workshop.

#### 4. Install local tools

* **AWS CLI v2** (installed in Week 1): `aws --version`
* **Node.js 18+** and **npm** to build the React frontend (optional if you use the prebuilt one).
* **Python 3.10+** and **boto3** (already available on Amazon Linux 2023 if you use Cloud9/CloudShell).

```bash
# Update AWS CLI & boto3
pip install --upgrade boto3 awscli
aws configure  # enter Access Key, Secret Key, region ap-southeast-1, output json
```

#### 5. Create the source documents S3 bucket

```bash
# Create the documents bucket for the Knowledge Base
aws s3 mb s3://fcaj-bedrock-docs-<your-id> --region ap-southeast-1
aws s3api put-bucket-versioning \
  --bucket fcaj-bedrock-docs-<your-id> \
  --versioning-configuration Status=Enabled

# Upload some sample documents
curl -o aws-overview.pdf https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html
aws s3 cp aws-overview.pdf s3://fcaj-bedrock-docs-<your-id>/
```

#### 6. Verify the environment

```bash
# Verify Bedrock access
aws bedrock list-foundation-models --region ap-southeast-1 \
  --query 'modelSummaries[?contains(modelId, `claude`) || contains(modelId, `titan-embed`)].modelId'

# Verify S3 bucket
aws s3 ls s3://fcaj-bedrock-docs-<your-id>/

# Verify IAM user
aws sts get-caller-identity
```

If all three commands return successfully, you are ready for the next part.

![prereq complete](/images/5-Workshop/5.2-Prerequisite/complete.png)

#### References
* [Amazon Bedrock Model Access](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html)
* [Setting up Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/setting-up.html)
* [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)