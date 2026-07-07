---
title : "Tạo S3 bucket & upload tài liệu"
date : 2026-04-12
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

Trong phần này bạn sẽ:
1. **Tạo S3 bucket** làm data source cho Knowledge Base.
2. **Upload các file tài liệu mẫu** (PDF, Markdown, HTML) lên bucket.
3. **Bật versioning** (khuyến nghị cho production).

#### 1. Tạo S3 bucket

```bash
# Tên bucket unique (thay <your-id> bằng ID của bạn)
BUCKET="fcaj-bedrock-docs-$(date +%s)"
echo "Bucket: $BUCKET"

# Tạo bucket
aws s3 mb s3://$BUCKET --region ap-southeast-1

# Bật versioning (rollback khi file bị lỗi)
aws s3api put-bucket-versioning \
  --bucket $BUCKET \
  --versioning-configuration Status=Enabled

# Khuyến nghị: bật encryption at rest
aws s3api put-bucket-encryption \
  --bucket $BUCKET \
  --server-side-encryption-configuration '{
    "Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "AES256"}}]
  }'

echo "BUCKET = $BUCKET"
```

> 💡 **Lưu bucket name** để dùng cho phần 5.3 (tạo Knowledge Base).

#### 2. Chuẩn bị tài liệu mẫu

Tạo 3 file markdown giả lập tài liệu AWS overview (dùng cho việc test RAG ở các bước sau):

```bash
mkdir -p ~/fcaj-docs && cd ~/fcaj-docs

# Tài liệu 1: Lambda overview
cat > aws-lambda.md << 'EOF'
# AWS Lambda — Compute Serverless

AWS Lambda là dịch vụ compute **serverless** cho phép bạn chạy code mà không cần
quản lý server. Lambda tự động scale theo lưu lượng và bạn chỉ trả tiền cho thời
gian compute thực tế (tính theo millisecond).

## Các tính năng chính

* Hỗ trợ nhiều runtime: Python, Node.js, Java, Go, .NET, Ruby, custom runtime
* Tích hợp sẵn với 200+ AWS services & SaaS apps thông qua EventBridge
* Provisioned Concurrency giữ lambda ấm, giảm cold start
* Tối đa 15 phút mỗi lần invoke
* Memory cấu hình từ 128 MB đến 10240 MB

## Use cases phổ biến

1. Xử lý event real-time (S3 upload, DynamoDB stream, Kinesis)
2. REST API backend (kết hợp API Gateway)
3. Scheduled jobs (cron thay cho EC2)
4. Chatbot backend với AI/ML inference
EOF

# Tài liệu 2: S3 overview
cat > aws-s3.md << 'EOF'
# Amazon S3 — Object Storage

Amazon S3 (Simple Storage Service) là dịch vụ lưu trữ object với **99.999999999%
(11 nines) độ bền** dữ liệu và 99.99% availability. S3 phù hợp cho mọi use case
lưu trữ từ website hosting, backup, data lake cho đến static asset.

## Storage Classes

| Class | Use case | Availability | Min storage |
|---|---|---|---|
| **Standard** | Truy cập thường xuyên | 99.99% | — |
| **Intelligent-Tiering** | Pattern truy cập không đoán được | 99.9% | — |
| **Standard-IA** | Truy cập không thường xuyên | 99.9% | 30 ngày |
| **One Zone-IA** | Dữ liệu không quan trọng, tái tạo được | 99.5% | 30 ngày |
| **Glacier Instant** | Archive, truy cập trong vài ms | 99.9% | 90 ngày |
| **Glacier Flexible** | Archive, truy cập trong vài phút–giờ | 99.9% | 90 ngày |
| **Glacier Deep Archive** | Long-term archive | 99.9% | 180 ngày |
EOF

# Tài liệu 3: Bedrock overview
cat > aws-bedrock.md << 'EOF'
# Amazon Bedrock — Foundation Models

Amazon Bedrock là dịch vụ **fully-managed** cung cấp Foundation Models (FMs)
qua API. Bạn không cần quản lý infrastructure, có thể chọn giữa nhiều model
của Anthropic, AI21, Cohere, Meta, Mistral, Stability AI, và Amazon.

## Tính năng chính

* **Knowledge Base:** RAG tự động — kết nối tài liệu với FM qua vector search
* **Agents:** tạo AI agent có thể gọi API và tự lập kế hoạch
* **Guardrails:** lọc nội dung có hại, mask PII, chống prompt injection
* **Fine-tuning & Continued Pre-training:** tuỳ chỉnh model
* **Provisioned Throughput:** đảm bảo latency và throughput

## Knowledge Base flow

```
User query → Embedding model → Vector search → Top-K chunks
                                          ↓
                       Claude 3.5 Sonnet (generation) → answer + citation
```
EOF

ls -la
```

#### 3. Upload tài liệu lên S3

```bash
aws s3 sync . s3://$BUCKET/ --exclude "*.DS_Store"
```

Verify:

```bash
aws s3 ls s3://$BUCKET/ --recursive --human-readable
```

Kỳ vọng:

```
2026-04-12 10:30:00    1.4 KiB aws-lambda.md
2026-04-12 10:30:00    1.9 KiB aws-s3.md
2026-04-12 10:30:00    1.6 KiB aws-bedrock.md
```

![S3 uploaded](/images/5-Workshop/5.3-Knowledge-Base/s3-uploaded.png)

#### 4. (Tuỳ chọn) Upload file PDF thật

Nếu bạn có PDF tài liệu AWS, có thể upload thêm để test multi-format:

```bash
# Upload file PDF
aws s3 cp ~/Downloads/aws-overview.pdf s3://$BUCKET/pdfs/

# Upload file HTML
aws s3 cp ~/Downloads/whitepaper.html s3://$BUCKET/html/
```

Knowledge Base hỗ trợ các format:
* **Tài liệu có cấu trúc:** PDF, DOCX, XLSX, HTML
* **Văn bản:** Markdown (.md), Plain text (.txt), RTF
* **Semi-structured:** CSV, JSON

#### 5. Resource summary

Sau phần này bạn có:

| Resource | Value | Use case |
|---|---|---|
| S3 bucket | `fcaj-bedrock-docs-<id>` | Data source cho Knowledge Base |
| 3+ files | `.md` / `.pdf` / `.html` | Tài liệu sẽ được ingest và chunk |
| Versioning | Enabled | Rollback khi cần |
| Encryption | AES-256 (SSE-S3) | Bảo mật dữ liệu tĩnh |

Phần tiếp theo (5.3.2) sẽ **tạo Knowledge Base** trỏ tới S3 bucket này và test retrieve trong console.

#### Tài liệu tham khảo
* [Create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-buckets-s3.html)
* [S3 versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
* [Supported document formats in Bedrock KB](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-supported-source-attr.html)