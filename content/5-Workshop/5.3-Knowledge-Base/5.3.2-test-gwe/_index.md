---
title : "Create Knowledge Base & test retrieve"
date : 2026-04-12
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

In this section you will:
1. **Create an OpenSearch Serverless collection + vector index** (for embeddings).
2. **Create the IAM Role** for the Bedrock Knowledge Base.
3. **Create the Knowledge Base** pointed at the S3 bucket from 5.3.1.
4. **Sync data** & **test retrieval** right in the Bedrock console.

#### 1. Create the OpenSearch Serverless collection

```bash
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)

# (a) Encryption policy (required)
aws opensearchserverless create-security-policy \
  --name kb-encryption \
  --type encryption \
  --policy "{\"Rules\":[{\"ResourceType\":\"collection\",\"Resource\":[\"collection/fcaj-vector-collection\"]}],\"AWSOwnedKey\":true}" \
  --region ap-southeast-1

# (b) Network policy: allow public access (lab/dev)
aws opensearchserverless create-security-policy \
  --name kb-network \
  --type network \
  --policy '[{"Rules":[{"ResourceType":"collection","Resource":["collection/fcaj-vector-collection"]},{"ResourceType":"dashboard","Resource":["collection/fcaj-vector-collection"]}],"AllowFromPublic":true}]' \
  --region ap-southeast-1

# (c) Data access policy: let the Bedrock IAM role read/write
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

# (d) Create the collection
aws opensearchserverless create-collection \
  --name fcaj-vector-collection \
  --type VECTORSEARCH \
  --region ap-southeast-1

# Wait until ACTIVE (~2-3 minutes)
aws opensearchserverless batch-get-collection \
  --names fcaj-vector-collection \
  --region ap-southeast-1 \
  --query 'collectionDetails[0].status'
# Expected: "ACTIVE"
```

#### 2. Create the vector index

```bash
COLLECTION_ID=$(aws opensearchserverless list-collections \
  --region ap-southeast-1 \
  --query 'collectionSummaries[?name==`fcaj-vector-collection`].id' \
  --output text)

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

# Note: Bedrock automatically creates this index when you associate the
# collection with a Knowledge Base, so this step can be skipped when using
# the console wizard.
echo "Vector index will be created automatically when you associate the KB."
```

#### 3. Create the IAM role for the Bedrock Knowledge Base

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

#### 4. Create the Knowledge Base in the Bedrock console

1. Open **Bedrock console** → **Knowledge bases** → **Create knowledge base**.
2. **Knowledge base details:**
   * Name: `fcaj-workshop-kb`
   * Description: "Knowledge base for FCAJ workshop chatbot"
   * IAM role: `fcaj-kb-role` (just created)
3. **Data source:**
   * Source: **Amazon S3**
   * S3 URI: `s3://<your-bucket>/`
4. **Embedding model:**
   * Embedding model: **Titan Text Embeddings v2**
   * Dimensions: **1024**
5. **Vector store:**
   * **Amazon OpenSearch Serverless** (pick the `fcaj-vector-collection` collection)
6. Click **Create knowledge base**.

```bash
# Retrieve the Knowledge Base ID for later use
KB_ID=$(aws bedrock-agent list-knowledge-bases \
  --query "knowledgeBaseSummaries[?name=='fcaj-workshop-kb'].knowledgeBaseId" \
  --output text --region ap-southeast-1)
echo "KB_ID = $KB_ID"
```

#### 5. Sync data from S3

```bash
KB_ID="..."  # paste from step 4

# Get the Data source ID
DS_ID=$(aws bedrock-agent list-data-sources \
  --knowledge-base-id $KB_ID \
  --query 'dataSourceSummaries[0].dataSourceId' \
  --output text --region ap-southeast-1)
echo "DS_ID = $DS_ID"

# Start the ingestion job
JOB_ID=$(aws bedrock-agent start-ingestion-job \
  --knowledge-base-id $KB_ID \
  --data-source-id $DS_ID \
  --query 'ingestionJob.ingestionJobId' \
  --output text --region ap-southeast-1)
echo "JOB_ID = $JOB_ID"

# Watch the job
aws bedrock-agent get-ingestion-job \
  --knowledge-base-id $KB_ID \
  --data-source-id $DS_ID \
  --ingestion-job-id $JOB_ID \
  --region ap-southeast-1 \
  --query 'ingestionJob.status'
# Expected: "COMPLETE" in 1-3 minutes
```

In the console, Knowledge Base → **Data source** you'll see:
* Status: **Sync complete**
* Records loaded: **3 files** → many chunks

#### 6. Test retrieval right in the Bedrock console

1. Knowledge Base → tab **Test**.
2. Select the model **Claude 3.5 Sonnet**.
3. Try these questions:

| Question | Expected result |
|---|---|
| `What is AWS Lambda?` | Answer from `aws-lambda.md` with citations |
| `What S3 storage classes are available?` | Lists all 7 classes, citations from `aws-s3.md` |
| `Describe the Bedrock Knowledge Base flow?` | Describes the flow, citation from `aws-bedrock.md` |
| `What is the weather in Tokyo today?` | "I couldn't find it in the documents", 0 citations |

Watch the **Retrieved chunks** panel to see which 3-5 chunks were retrieved for each question — useful to debug the chunking strategy.

![test kb](/images/5-Workshop/5.3-Knowledge-Base/test-kb.png)

#### 7. Save the important IDs

```bash
echo "
KB_ID         = $KB_ID
DS_ID         = $DS_ID
S3_BUCKET     = $BUCKET
COLLECTION_ID = $COLLECTION_ID
" > ~/fcaj-chat-app/ids.txt
cat ~/fcaj-chat-app/ids.txt
```

> 💡 You'll need `KB_ID` for **5.4 (Lambda handler)** and **5.5 (Guardrails)**.

#### Summary

After this section you have:
* An OpenSearch Serverless collection **ACTIVE** with a 1024-dim vector index.
* IAM role `fcaj-kb-role` with permissions for S3 + aoss + embedding model.
* Knowledge Base `fcaj-workshop-kb` with **3 files synced**.
* The console answering correctly with citations from the sample markdowns.

You're ready for section 5.4 — building the **API + Frontend** that chats with this Knowledge Base.

#### References
* [Bedrock Knowledge Base User Guide](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
* [OpenSearch Serverless Vector Search](https://docs.aws.amazon.com/opensearch-service/latest/developerguide/serverless-vector-search.html)
* [StartIngestionJob API](https://docs.aws.amazon.com/bedrock-agent/latest/APIReference/API_StartIngestionJob.html)
* [Titan Embeddings v2 dimensions](https://docs.aws.amazon.com/bedrock/latest/userguide/titan-embedding-models.html)