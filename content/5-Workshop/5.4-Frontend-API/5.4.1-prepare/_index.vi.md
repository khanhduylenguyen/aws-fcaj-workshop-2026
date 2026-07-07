---
title : "Chuẩn bị IAM Role & môi trường"
date : 2026-04-12
weight : 1
chapter : false
pre : " <b> 5.4.1 </b> "
---

Trong phần này bạn sẽ chuẩn bị:

* **IAM Role** cho Lambda được phép gọi Bedrock Knowledge Base.
* **Cấu trúc thư mục** mã nguồn để viết Lambda + Frontend.
* **Lấy các ID cần thiết** (Knowledge Base ID, Model ARN) từ phần 5.3.

#### 1. Tạo IAM Role cho Lambda

Lambda cần một **execution role** để có quyền gọi Bedrock Agent Runtime API.

Tạo trust policy cho phép Lambda assume role:

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

Tạo permission policy cho phép gọi Bedrock + ghi CloudWatch Logs:

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

> 💡 Thay `<account-id>` bằng AWS Account ID của bạn (lấy bằng `aws sts get-caller-identity`).

#### 2. Lấy Knowledge Base ID và Model ARN

```bash
# Lấy KB_ID từ phần 5.3
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)

echo "KB_ID = $KB_ID"

# Model ARN cố định cho Claude 3.5 Sonnet ở Singapore
export MODEL_ARN="arn:aws:bedrock:ap-southeast-1::foundation-model/anthropic.claude-3-5-sonnet-20240620-v1:0"
export REGION="ap-southeast-1"
```

Lưu 3 giá trị này để dùng cho bước tiếp theo (5.4.2 — tạo Lambda).

#### 3. Chuẩn bị thư mục source code

```bash
mkdir -p ~/fcaj-chat-app
cd ~/fcaj-chat-app

# Thư mục con
mkdir -p lambda       # Lambda function code
mkdir -p frontend     # React SPA
```

#### 4. Kiểm tra môi trường

Chạy 3 lệnh kiểm tra, cả 3 phải trả kết quả thành công:

```bash
# (a) IAM role đã tạo
aws iam get-role --role-name fcaj-chat-handler-role \
  --query 'Role.RoleName' --output text
# Kỳ vọng: fcaj-chat-handler-role

# (b) Bedrock model access
aws bedrock list-foundation-models --region ap-southeast-1 \
  --query 'modelSummaries[?contains(modelId, `claude-3-5`)].modelId'
# Kỳ vọng: có ít nhất 1 model Claude 3.5 Sonnet

# (c) Knowledge Base đã sẵn sàng
aws bedrock-agent get-knowledge-base \
  --knowledge-base-id $KB_ID \
  --region ap-southeast-1 \
  --query 'knowledgeBase.status'
# Kỳ vọng: "ACTIVE"
```

![prepare complete](/images/5-Workshop/5.4-Frontend-API/prepare.png)

Nếu cả 3 lệnh đều OK, bạn sẵn sàng cho [phần 5.4.2 — Tạo Lambda function & API Gateway endpoint](../5.4.2-create-interface-enpoint/).

#### Tài liệu tham khảo
* [Lambda execution role](https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro-execution-role.html)
* [Bedrock Agent Runtime permissions](https://docs.aws.amazon.com/bedrock/latest/userguide/agents-lambda.html)
* [ListKnowledgeBases API](https://docs.aws.amazon.com/bedrock-agent/latest/APIReference/API_ListKnowledgeBases.html)