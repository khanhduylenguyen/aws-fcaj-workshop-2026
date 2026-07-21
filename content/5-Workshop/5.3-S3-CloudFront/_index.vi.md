---
title: "Dịch vụ Lưu trữ S3 & CloudFront"
date: 2026-07-20
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Giới thiệu Amazon S3 & CloudFront

**Amazon S3 (Simple Storage Service)** là dịch vụ lưu trữ object với độ bền 99.999999999% (11 nines). **Amazon CloudFront** là CDN (Content Delivery Network) giúp phân phối nội dung toàn cầu với độ trễ thấp.

##### Tại sao cần S3 + CloudFront?

| Tính năng | S3 | CloudFront |
|-----------|-----|------------|
| Lưu trữ file | ✅ | ❌ |
| CDN toàn cầu | ❌ | ✅ |
| Presigned URL | ✅ | ❌ |
| Edge locations | ❌ | ✅ (300+) |
| WAF integration | ❌ | ✅ |

#### Cấu hình S3 Bucket

##### Bước 1: Tạo S3 Bucket

```bash
# Tạo bucket
aws s3 mb s3://medi-path-ease-uploads --region ap-southeast-1

# Kết quả:
# make_bucket: medi-path-ease-uploads
```

> **📸 [HÌNH 1]: S3 Bucket Creation trên AWS Console**

##### Bước 2: Bật Versioning

```bash
# Bật versioning để khôi phục file bị xóa nhầm
aws s3api put-bucket-versioning \
    --bucket medi-path-ease-uploads \
    --versioning-configuration Status=Enabled
```

##### Bước 3: Bật Server-Side Encryption

```bash
# Mã hóa AES-256
aws s3api put-bucket-encryption \
    --bucket medi-path-ease-uploads \
    --server-side-encryption-configuration '{
        "Rules": [{
            "ApplyServerSideEncryptionByDefault": {
                "SSEAlgorithm": "AES256"
            },
            "BucketKeyEnabled": true
        }]
    }'
```

##### Bước 4: Cấu hình Bucket Policy

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "EnforceHTTPS",
            "Effect": "Deny",
            "Principal": {"AWS": "*"},
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::medi-path-ease-uploads",
                "arn:aws:s3:::medi-path-ease-uploads/*"
            ],
            "Condition": {
                "Bool": {"aws:SecureTransport": "false"}
            }
        },
        {
            "Sid": "AllowBackendAccess",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::123456789:user/medi-path-ease-backend"
            },
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": "arn:aws:s3:::medi-path-ease-uploads/*"
        }
    ]
}
```

```bash
# Áp dụng policy
aws s3api put-bucket-policy \
    --bucket medi-path-ease-uploads \
    --policy file://bucket-policy.json
```

##### Bước 5: Cấu hình CORS

```json
[
    {
        "AllowedOrigins": [
            "http://localhost:5173",
            "https://mediathease.com"
        ],
        "AllowedMethods": ["GET", "PUT", "POST", "DELETE", "HEAD"],
        "AllowedHeaders": ["*"],
        "MaxAgeSeconds": 3600
    }
]
```

```bash
aws s3api put-bucket-cors \
    --bucket medi-path-ease-uploads \
    --cors-configuration file://cors.json
```

#### Cấu hình CloudFront

##### Bước 1: Tạo CloudFront Distribution

```bash
# Tạo distribution
aws cloudfront create-distribution \
    --origin-domain-name medi-path-ease-uploads.s3.ap-southeast-1.amazonaws.com \
    --default-root-object index.html \
    --enabled
```

> **📸 [HÌNH 2]: CloudFront Distribution Configuration**

##### Bước 2: Cấu hình Behavior

```json
{
    "PathPattern": "uploads/*",
    "TargetOriginId": "medi-path-ease-s3",
    "ViewerProtocolPolicy": "redirect-to-https",
    "AllowedMethods": ["GET", "HEAD"],
    "CachedMethods": ["GET", "HEAD"],
    "ForwardValues": "none",
    "MinTTL": 3600,
    "DefaultTTL": 86400,
    "MaxTTL": 31536000
}
```

#### Cấu trúc Folder trong S3

```
medi-path-ease-uploads/
├── uploads/
│   ├── lab-results/           # Kết quả xét nghiệm
│   │   └── <patientId>/
│   │       └── <yyyy>/
│   │           └── <mm>/
│   ├── prescriptions/         # Đơn thuốc
│   │   └── <patientId>/
│   ├── ehr/                  # Hồ sơ bệnh án
│   │   └── <patientId>/
│   └── chat-attachments/     # File đính kèm chat
│       └── <patientId>/
└── static/
    └── images/               # Hình ảnh tĩnh
```

```bash
# Tạo folder structure
aws s3api put-object --bucket medi-path-ease-uploads --key uploads/lab-results/
aws s3api put-object --bucket medi-path-ease-uploads --key uploads/prescriptions/
aws s3api put-object --bucket medi-path-ease-uploads --key uploads/ehr/
aws s3api put-object --bucket medi-path-ease-uploads --key uploads/chat-attachments/
```

#### Tích hợp vào Backend

##### Cài đặt AWS SDK

```bash
npm install @aws-sdk/client-s3 @aws-sdk/s3-request-presigner
```

##### S3 Service

```javascript
// services/s3Service.js
import {
    S3Client,
    PutObjectCommand,
    GetObjectCommand,
    DeleteObjectCommand
} from "@aws-sdk/client-s3";
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";
import crypto from "crypto";
import path from "path";

const s3Client = new S3Client({
    region: process.env.AWS_REGION || "ap-southeast-1",
    credentials: {
        accessKeyId: process.env.AWS_ACCESS_KEY_ID,
        secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY
    }
});

/**
 * Tạo key theo cấu trúc: uploads/<folder>/<patientId>/<yyyy>/<mm>/<uuid>-<safeName>
 */
function buildKey({ folder, patientId, originalName }) {
    const now = new Date();
    const yyyy = now.getUTCFullYear();
    const mm = String(now.getUTCMonth() + 1).padStart(2, "0");
    const ext = path.extname(originalName || "").toLowerCase();
    const uuid = crypto.randomUUID();
    const safeName = (path.basename(originalName || "file", ext) || "file")
        .replace(/[^a-zA-Z0-9-_]+/g, "_")
        .slice(0, 60);
    const safePatientId = String(patientId || "unknown").slice(0, 60);
    return `uploads/${folder}/${safePatientId}/${yyyy}/${mm}/${uuid}-${safeName}${ext}`;
}

/**
 * Upload file lên S3
 */
export async function uploadBuffer({ buffer, originalName, mimeType, folder, patientId }) {
    const key = buildKey({ folder, patientId, originalName });
    
    const command = new PutObjectCommand({
        Bucket: process.env.AWS_S3_BUCKET,
        Key: key,
        Body: buffer,
        ContentType: mimeType,
        Metadata: {
            patientId: String(patientId),
            uploadedAt: new Date().toISOString()
        }
    });

    const result = await s3Client.send(command);
    
    return {
        key,
        bucket: process.env.AWS_S3_BUCKET,
        region: process.env.AWS_REGION,
        size: buffer.length,
        etag: result.ETag?.replace(/"/g, "")
    };
}

/**
 * Tạo presigned URL để xem file (hết hạn sau 1 giờ)
 */
export async function getSignedDownloadUrl(key, expiresIn = 3600) {
    const command = new GetObjectCommand({
        Bucket: process.env.AWS_S3_BUCKET,
        Key: key
    });
    
    return await getSignedUrl(s3Client, command, { expiresIn });
}

/**
 * Xóa file khỏi S3
 */
export async function deleteObject(key) {
    const command = new DeleteObjectCommand({
        Bucket: process.env.AWS_S3_BUCKET,
        Key: key
    });
    
    await s3Client.send(command);
    return { deleted: true, key };
}
```

##### Upload API Route

```javascript
// routes/uploads.js
import express from "express";
import multer from "multer";
import { uploadBuffer, getSignedDownloadUrl, deleteObject } from "../services/s3Service.js";

const router = express.Router();
const upload = multer({
    storage: multer.memoryStorage(),
    limits: { fileSize: 10 * 1024 * 1024 } // 10MB
});

router.post("/:folder", authMiddleware, upload.single("file"), async (req, res) => {
    try {
        const { folder } = req.params;
        const patientId = req.body.patientId;

        const result = await uploadBuffer({
            buffer: req.file.buffer,
            originalName: req.file.originalname,
            mimeType: req.file.mimetype,
            folder,
            patientId
        });

        res.json({ success: true, data: result });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

router.get("/url", authMiddleware, async (req, res) => {
    try {
        const { key } = req.query;
        const url = await getSignedDownloadUrl(key);
        res.json({ success: true, url });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

router.delete("/:key(*)", authMiddleware, async (req, res) => {
    try {
        await deleteObject(req.params.key);
        res.json({ success: true });
    } catch (error) {
        res.status(500).json({ error: error.message });
    }
});

export default router;
```

#### Environment Variables

```bash
# .env
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
AWS_S3_BUCKET=medi-path-ease-uploads
CLOUDFRONT_URL=https://d123456.cloudfront.net
```

#### Resource Summary

| Resource | Giá trị | Mô tả |
|---------|---------|--------|
| S3 Bucket | `medi-path-ease-uploads` | Lưu trữ tài liệu y tế |
| CloudFront | `d123456.cloudfront.net` | CDN toàn cầu |
| Encryption | AES-256 | Bảo mật dữ liệu |
| Versioning | Enabled | Khôi phục file |
| CORS | Configured | Cho phép frontend |

> **📸 [HÌNH 3]: S3 Bucket Management Dashboard**

#### Tài liệu tham khảo

* [Amazon S3 Documentation](https://docs.aws.amazon.com/s3/)
* [Amazon CloudFront Documentation](https://docs.aws.amazon.com/cloudfront/)
* [AWS SDK for JavaScript v3](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/)
