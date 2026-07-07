---
title : "Tạo Knowledge Base & test retrieve"
date : 2026-04-12
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

Trong phần này bạn sẽ:
1. **Tạo OpenSearch Serverless collection + vector index** (lưu embeddings).
2. **Tạo IAM Role** cho Bedrock Knowledge Base.
3. **Tạo Knowledge Base** trỏ tới S3 bucket ở 5.3.1.
4. **Sync dữ liệu** & **test truy vấn** ngay trong Bedrock console.

#### 1. Tạo OpenSearch Serverless collection

```bash
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

# (a) Encryption policy (bắt buộc)
aws opensearchserverless create-security-policy \
  --name kb-encryption \
  --type encryption \
  --policy "{\"Rules\":[{\"ResourceType\":\"collection\",\"Resource\":[\"collection/fcaj-vector-collection\"]}],\"AWSOwnedKey\":true}" \
  --region ap-southeast-1

# (b) Network policy: cho phép public access (lab/dev)
aws opensearchserverless create-security-policy \
  --name kb-network \
  --type network \
  --policy '[{"Rules":[{"ResourceType":"collection","Resource":["collection/fcaj-vector-collection"]},{"ResourceType":"dashboard","Resource":["collection/fcaj-vector-collection"]}],"AllowFromPublic":true}]' \
  --region ap-southeast-1

# (c) Data access policy: cho phép Bedrock IAM role đọc/ghi
cat > kb-data-policy.json << EOF
[
  {
    "Rules": [
      {
        "ResourceType": "collection",
        "Resource": ["collection/fcaj-vector-collection"]
      },
      {
        "ResourceType": "index",
        "Resource": ["index/fcaj-vector-collection/kb-documents-index"]
      }
    ],
    "Principal": [
      "arn:aws:iam::${ACCOUNT_ID}:role/BedrockKnowledgeBaseRole",
      "arn:aws:iam::${ACCOUNT_ID}:role/fcaj-kb-role"
    ],
    "Action": ["aoss:*"]
  }
]
EOF

aws opensearchserverless create-access-policy \
  --name kb-data-access \
  --type data \
  --policy file://kb-data-policy.json \
  --region ap-southeast-1

# (d) Tạo collection
aws opensearchserverless create-collection \
  --name fcaj-vector-collection \
  --type VECTORSEARCH \
  --region ap-southeast-1

# Chờ trạng thái ACTIVE (~2-3 phút)
aws opensearchserverless batch-get-collection \
  --names fcaj-vector-collection \
  --region ap-southeast-1 \
  --query 'collectionDetails[0].status'
# Kỳ vọng: "ACTIVE"
```

#### 2. Tạo vector index

```bash
COLLECTION_ID=$(aws opensearchserverless list-collections \
  --region ap-southeast-1 \
  --query 'collectionSummaries[?name==`fcaj-vector-collection`].id' \
  --output text)

COLLECTION_ARN=$(aws opensearchserverless batch-get-collection \
  --ids $COLLECTION_ID --region ap-southeast-1 \
  --query 'collectionDetails[0].arn' --output text)

cat > index-config.json << 'EOF'
{
  "settings": { "index": { "knn": true } },
  "mappings": {
    "properties": {
      "bedrock-knowledge-base-default-vector": {
        "type": "knn_vector",
        "dimension": 1024,
        "method": { "name": "hnsw", "engine": "faas" }
      },
      "AMAZON_BEDROCK_METADATA":   { "type": "text", "index": false },
      "AMAZON_BEDROCK_TEXT_CHUNK": { "type": "text", "index": false }
    }
  }
}
EOF

# Tạo index thông qua OpenSearch API (cần ký request body)
awscurl="curl"
# Bedrock tự tạo index này khi bạn chọn OpenSearch Serverless trong KB wizard,
# nên phần này có thể bỏ qua nếu dùng console.
echo "Vector index sẽ được tạo tự động khi bạn associate với Knowledge Base."
```

#### 3. Tạo IAM Role cho Bedrock Knowledge Base

```bash
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

# Trust policy
cat > kb-trust-policy.json << 'EOF'
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": { "Service": "bedrock.amazonaws.com" },
    "Action": "sts:AssumeRole"
  }]
}
EOF

BUCKET=$(cat ~/fcaj-docs/.bucket 2>/dev/null || echo "fcaj-bedrock-docs-<your-id>")

cat > kb-permission-policy.json << EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": ["s3:GetObject", "s3:ListBucket"],
      "Resource": [
        "arn:aws:s3:::${BUCKET}",
        "arn:aws:s3:::${BUCKET}/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": ["aoss:APIAccessAll"],
      "Resource": ["arn:aws:aoss:ap-southeast-1:${ACCOUNT_ID}:collection/*"]
    },
    {
      "Effect": "Allow",
      "Action": ["bedrock:InvokeModel"],
      "Resource": ["arn:aws:bedrock:ap-southeast-1::foundation-model/amazon.titan-embed-text-v2:0"]
    }
  ]
}
EOF

aws iam create-role \
  --role-name fcaj-kb-role \
  --assume-role-policy-document file://kb-trust-policy.json

aws iam create-policy \
  --policy-name fcaj-kb-policy \
  --policy-document file://kb-permission-policy.json

aws iam attach-role-policy \
  --role-name fcaj-kb-role \
  --policy-arn arn:aws:iam::${ACCOUNT_ID}:policy/fcaj-kb-policy
```

#### 4. Tạo Knowledge Base trong Bedrock console

1. Mở **Bedrock console** → **Knowledge bases** → **Create knowledge base**.
2. **Knowledge base details:**
   * Name: `fcaj-workshop-kb`
   * Description: "Knowledge base for FCAJ workshop chatbot"
   * IAM role: `fcaj-kb-role` (vừa tạo)
3. **Data source:**
   * Source: **Amazon S3**
   * S3 URI: `s3://<your-bucket>/`
4. **Embedding model:**
   * Embedding model: **Titan Text Embeddings v2**
   * Dimensions: **1024**
5. **Vector store:**
   * **Amazon OpenSearch Serverless** (chọn collection `fcaj-vector-collection`)
6. Click **Create knowledge base**.

```bash
# Lấy Knowledge Base ID để dùng cho bước sau
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)
echo "KB_ID = $KB_ID"
```

#### 5. Sync dữ liệu từ S3

```bash
KB_ID="..."  # paste từ output bước 4

# Lấy Data source ID
DS_ID=$(aws bedrock-agent list-data-sources \
  --knowledge-base-id $KB_ID \
  --query 'dataSourceSummaries[0].dataSourceId' \
  --output text --region ap-southeast-1)
echo "DS_ID = $DS_ID"

# Bắt đầu ingestion job
JOB_ID=$(aws bedrock-agent start-ingestion-job \
  --knowledge-base-id $KB_ID \
  --data-source-id $DS_ID \
  --query 'ingestionJob.ingestionJobId' \
  --output text --region ap-southeast-1)
echo "JOB_ID = $JOB_ID"

# Theo dõi job
aws bedrock-agent get-ingestion-job \
  --knowledge-base-id $KB_ID \
  --data-source-id $DS_ID \
  --ingestion-job-id $JOB_ID \
  --region ap-southeast-1 \
  --query 'ingestionJob.status'
# Kỳ vọng: "COMPLETE" trong 1-3 phút
```

Trong console, Knowledge Base → **Data source** → bạn sẽ thấy:
* Status: **Sync complete**
* Records loaded: **3 files** → nhiều chunks

#### 6. Test retrieve ngay trong Bedrock console

1. Knowledge Base → tab **Test**.
2. Chọn model **Claude 3.5 Sonnet**.
3. Thử các câu hỏi:

| Câu hỏi | Kỳ vọng |
|---|---|
| `AWS Lambda là gì?` | Answer trả về từ nội dung `aws-lambda.md`, có citation |
| `S3 có những storage class nào?` | Trả lời đầy đủ 7 loại, citation từ `aws-s3.md` |
| `Bedrock Knowledge Base flow?` | Mô tả flow, citation từ `aws-bedrock.md` |
| `Thời tiết hôm nay ở Tokyo?` | "Tôi không tìm thấy trong tài liệu", 0 citation |

Quan sát panel **Retrieved chunks** để xem 3-5 chunks nào được retrieve cho từng câu hỏi — rất hữu ích để debug chunking strategy.

![test kb](/images/5-Workshop/5.3-Knowledge-Base/test-kb.png)

#### 7. Lưu IDs quan trọng

```bash
echo "
KB_ID         = $KB_ID
DS_ID         = $DS_ID
S3_BUCKET     = $BUCKET
COLLECTION_ID = $COLLECTION_ID
" > ~/fcaj-chat-app/ids.txt
cat ~/fcaj-chat-app/ids.txt
```

> 💡 Bạn sẽ cần `KB_ID` cho **5.4 (Lambda handler)** và **5.5 (Guardrails)**.

#### Tổng kết

Sau phần này bạn có:
* OpenSearch Serverless collection **ACTIVE** với vector index 1024 dims.
* IAM role `fcaj-kb-role` với quyền S3 + aoss + embed model.
* Knowledge Base `fcaj-workshop-kb` đã **sync xong 3 file**.
* Console trả lời chính xác + citation từ các markdown mẫu.

Bạn đã sẵn sàng cho phần 5.4 — xây dựng **API + Frontend** chat với Knowledge Base này.

#### Tài liệu tham khảo
* [Bedrock Knowledge Base User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
* [OpenSearch Serverless Vector Search](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless-vector-search.html)
* [StartIngestionJob API](https://docs.aws.amazon.com/bedrock-agent/latest/APIReference/API_StartIngestionJob.html)
* [Titan Embeddings v2 dimensions](https://docs.aws.amazon.com/bedrock/latest/userguide/titan-embedding-models.html)