---
title : "Các bước chuẩn bị"
date : 2026-04-12
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### 1. Tài khoản AWS & Region

Trong workshop này, chúng ta sẽ dùng region **Singapore (ap-southeast-1)**. Hãy đảm bảo bạn đã:
* Đăng nhập vào AWS Console với user có quyền Administrator (hoặc user có IAM policy dưới đây).
* Region được chọn là **Singapore** ở góc trên bên phải Console.

#### 2. Bật Model Access trong Amazon Bedrock

Trước khi dùng được Claude hay Titan, bạn cần **request access** cho model:

1. Mở Amazon Bedrock Console ở region Singapore.
2. Vào **Model access** ở menu trái dưới mục **Bedrock configurations**.
3. Click **Manage model access** → chọn các model sau:
   * **Anthropic:** Claude 3.5 Sonnet, Claude 3 Haiku
   * **Amazon:** Titan Embeddings v2, Titan Text G1 - Express
   * (tuỳ chọn) **Meta:** Llama 3 8B Instruct
4. Click **Request model access** và chờ vài phút để AWS duyệt.

![model access](/images/5-Workshop/5.2-Prerequisite/model-access.png)

#### 3. Tạo IAM policy cho user thực hiện workshop

Gắn policy sau vào IAM user của bạn (hoặc dùng AdministratorAccess cho đơn giản):

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

> 💡 **Lưu ý:** Workshop này dùng các dịch vụ fully-managed nên hầu hết chi phí nằm ở Bedrock (theo token). Mỗi query Claude 3.5 Sonnet ~$0.003 input + $0.015 output token. Vài chục câu hỏi trong workshop chỉ tốn chưa đến $1. OpenSearch Serverless có minimum charge $0.24/OCU/giờ — bạn nhớ dọn dẹp sau workshop.

#### 4. Cài đặt công cụ local

* **AWS CLI v2** (đã cài ở tuần 1): `aws --version`
* **Node.js 18+** và **npm** để build React frontend (tuỳ chọn nếu dùng frontend có sẵn).
* **Python 3.10+** và **boto3** (đã có sẵn trên Amazon Linux 2023 nếu bạn dùng Cloud9/CloudShell).

```bash
# Cập nhật AWS CLI & boto3
pip install --upgrade boto3 awscli
aws configure  # nhập Access Key, Secret Key, region ap-southeast-1, output json
```

#### 5. Tạo S3 bucket chứa tài liệu nguồn

```bash
# Tạo bucket documents cho Knowledge Base
aws s3 mb s3://fcaj-bedrock-docs-<your-id> --region ap-southeast-1
aws s3api put-bucket-versioning \
  --bucket fcaj-bedrock-docs-<your-id> \
  --versioning-configuration Status=Enabled

# Upload một số tài liệu mẫu (download từ AWS Whitepapers, hoặc dùng file của bạn)
curl -o aws-overview.pdf https://docs.aws.amazon.com/whitepapers/latest/aws-overview/introduction.html
aws s3 cp aws-overview.pdf s3://fcaj-bedrock-docs-<your-id>/
```

#### 6. Kiểm tra môi trường

```bash
# Verify Bedrock access
aws bedrock list-foundation-models --region ap-southeast-1 \
  --query 'modelSummaries[?contains(modelId, `claude`) || contains(modelId, `titan-embed`)].modelId'

# Verify S3 bucket
aws s3 ls s3://fcaj-bedrock-docs-<your-id>/

# Verify IAM user
aws sts get-caller-identity
```

Nếu cả 3 lệnh đều trả về kết quả thành công, bạn đã sẵn sàng cho phần tiếp theo.

![prereq complete](/images/5-Workshop/5.2-Prerequisite/complete.png)

#### Tài liệu tham khảo
* [Amazon Bedrock Model Access](https://docs.aws.amazon.com/bedrock/latest/userguide/model-access.html)
* [Setting up Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/setting-up.html)
* [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)