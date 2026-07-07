---
title : "Create S3 bucket & upload documents"
date : 2026-04-12
weight : 1
chapter : false
pre : " <b> 5.3.1 </b> "
---

In this section you will:
1. **Create an S3 bucket** to act as the Knowledge Base data source.
2. **Upload sample documents** (PDF, Markdown, HTML) to the bucket.
3. **Enable versioning** (recommended for production).

#### 1. Create the S3 bucket

```bash
# Unique bucket name (replace <your-id> with your own id)
BUCKET="fcaj-bedrock-docs-$(date +%s)"
echo "Bucket: $BUCKET"

# Create the bucket
aws s3 mb s3://$BUCKET --region ap-southeast-1

# Enable versioning (rollback when a file is broken)
aws s3api put-bucket-versioning \
  --bucket $BUCKET \
  --versioning-configuration Status=Enabled

# Recommended: enable encryption at rest
aws s3api put-bucket-encryption \
  --bucket $BUCKET \
  --server-side-encryption-configuration '{
    "Rules": [{"ApplyServerSideEncryptionByDefault": {"SSEAlgorithm": "AES256"}}]
  }'

echo "BUCKET = $BUCKET"
```

> 💡 **Save the bucket name** for use in section 5.3 (create the Knowledge Base).

#### 2. Prepare sample documents

Create 3 markdown files that simulate AWS documentation (used to test RAG in later steps):

```bash
mkdir -p ~/fcaj-docs && cd ~/fcaj-docs

# Document 1: Lambda overview
cat > aws-lambda.md << 'EOF'
# AWS Lambda — Serverless Compute

AWS Lambda is a **serverless** compute service that lets you run code without
managing servers. Lambda automatically scales with traffic and you only pay for
the actual compute time (billed per millisecond).

## Key features

* Supports multiple runtimes: Python, Node.js, Java, Go, .NET, Ruby, custom
* Built-in integration with 200+ AWS services & SaaS apps via EventBridge
* Provisioned Concurrency keeps Lambdas warm, reducing cold start
* Maximum 15 minutes per invocation
* Memory configurable from 128 MB up to 10240 MB

## Common use cases

1. Real-time event processing (S3 upload, DynamoDB stream, Kinesis)
2. REST API backend (paired with API Gateway)
3. Scheduled jobs (cron, replacing EC2)
4. Chatbot backend with AI/ML inference
EOF

# Document 2: S3 overview
cat > aws-s3.md << 'EOF'
# Amazon S3 — Object Storage

Amazon S3 (Simple Storage Service) is an object-storage service with
**99.999999999% (11 nines) durability** and 99.99% availability. S3 fits every
storage use case from website hosting, backup, and data lakes to static assets.

## Storage Classes

| Class | Use case | Availability | Min storage |
|---|---|---|---|
| **Standard** | Frequent access | 99.99% | — |
| **Intelligent-Tiering** | Unpredictable access pattern | 99.9% | — |
| **Standard-IA** | Infrequent access | 99.9% | 30 days |
| **One Zone-IA** | Non-critical, reproducible data | 99.5% | 30 days |
| **Glacier Instant** | Archive, ms access | 99.9% | 90 days |
| **Glacier Flexible** | Archive, minutes–hours access | 99.9% | 90 days |
| **Glacier Deep Archive** | Long-term archive | 99.9% | 180 days |
EOF

# Document 3: Bedrock overview
cat > aws-bedrock.md << 'EOF'
# Amazon Bedrock — Foundation Models

Amazon Bedrock is a **fully-managed** service that provides Foundation Models
(FMs) via API. You don't manage any infrastructure, and you can choose from
models from Anthropic, AI21, Cohere, Meta, Mistral, Stability AI, and Amazon.

## Key features

* **Knowledge Base:** automatic RAG — connects your documents to an FM via vector search
* **Agents:** build AI agents that can call APIs and self-plan
* **Guardrails:** filter harmful content, mask PII, resist prompt injection
* **Fine-tuning & Continued Pre-training:** customize the model
* **Provisioned Throughput:** guaranteed latency and throughput

## Knowledge Base flow

```
User query → Embedding model → Vector search → Top-K chunks
                                          ↓
                       Claude 3.5 Sonnet (generation) → answer + citation
```
EOF

ls -la
```

#### 3. Upload documents to S3

```bash
aws s3 sync . s3://$BUCKET/ --exclude "*.DS_Store"
```

Verify:

```bash
aws s3 ls s3://$BUCKET/ --recursive --human-readable
```

Expected:

```
2026-04-12 10:30:00    1.4 KiB aws-lambda.md
2026-04-12 10:30:00    1.9 KiB aws-s3.md
2026-04-12 10:30:00    1.6 KiB aws-bedrock.md
```

![S3 uploaded](/images/5-Workshop/5.3-Knowledge-Base/s3-uploaded.png)

#### 4. (Optional) Upload real PDF files

If you have real AWS whitepaper PDFs, you can upload them too and test the multi-format path:

```bash
# Upload a PDF file
aws s3 cp ~/Downloads/aws-overview.pdf s3://$BUCKET/pdfs/

# Upload an HTML file
aws s3 cp ~/Downloads/whitepaper.html s3://$BUCKET/html/
```

Knowledge Base supports these formats:
* **Structured documents:** PDF, DOCX, XLSX, HTML
* **Plain text:** Markdown (.md), plain text (.txt), RTF
* **Semi-structured:** CSV, JSON

#### 5. Resource summary

After this section you have:

| Resource | Value | Use case |
|---|---|---|
| S3 bucket | `fcaj-bedrock-docs-<id>` | Data source for the Knowledge Base |
| 3+ files | `.md` / `.pdf` / `.html` | Documents that will be ingested and chunked |
| Versioning | Enabled | Rollback when needed |
| Encryption | AES-256 (SSE-S3) | Data at rest protection |

The next section (5.3.2) will **create the Knowledge Base** pointed at this S3 bucket and test retrieval in the console.

#### References
* [Create a bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/creating-buckets-s3.html)
* [S3 versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html)
* [Supported document formats in Bedrock KB](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-supported-source-attr.html)