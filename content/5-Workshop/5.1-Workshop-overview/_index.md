---
title : "Workshop Introduction"
date : 2026-07-20
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction to Amazon S3 in Medi Path Ease

**Amazon S3 (Simple Storage Service)** is AWS's object storage service, used in Medi Path Ease to store important patient medical documents.

#### S3 Features Used

* **Lab Results Storage:** Upload and retrieve PDF lab test results (blood tests, urine tests, specialized tests).
* **Prescription Storage:** Store photographed or scanned prescription images.
* **Electronic Health Records (EHR):** Store electronic medical record documents.
* **Telemedicine Attachments:** Store images and documents shared during remote consultations.
* **Presigned URLs:** Generate temporary URLs for patients and doctors to view files without public access.

#### Why Use S3 Instead of Database Storage?

* **Security:** Files are stored separately from the database, accessed only via presigned URLs with expiration.
* **Performance:** Object storage is optimized for large files (PDFs, images) compared to MongoDB.
* **Cost:** S3 Standard is cheaper than MongoDB Atlas for large file storage.
* **Durability:** 99.999999999% (11 nines) data durability.
* **Versioning:** Can recover accidentally deleted files.

#### S3 Bucket Folder Structure

```
medi-path-ease-uploads/
├── lab-results/           # Lab test results
│   └── <patientId>/
│       └── <yyyy>/
│           └── <mm>/
├── prescriptions/         # Prescriptions
│   └── <patientId>/
│       └── <yyyy>/
│           └── <mm>/
├── ehr/                  # Electronic Health Records
│   └── <patientId>/
│       └── <yyyy>/
│           └── <mm>/
└── chat-attachments/     # Telemedicine chat attachments
    └── <patientId>/
        └── <yyyy>/
            └── <mm>/
```

#### Workshop Overview

In this workshop, you will configure and integrate Amazon S3 into the Medi Path Ease system:

* **Cloud side (AWS):** Create S3 bucket with security configuration, IAM policies, presigned URLs.
* **Backend side:** Integrate AWS SDK into Express server, build upload/download API.
* **Frontend side:** Connect React UI with backend for file upload and presigned URL display.

```mermaid
flowchart TB
  subgraph Frontend
    UI[React UI<br>Upload/File Viewer]
  end

  subgraph Backend["Express Backend :3001"]
    API[API Routes<br>/api/uploads/*]
    S3Service[S3 Service<br>s3Service.js]
  end

  subgraph AWS["AWS Region ap-southeast-1"]
    S3[(S3 Bucket<br>medi-path-ease-uploads)]
    IAM[IAM User<br>Restricted Permissions]
  end

  UI -->|1. Upload File| API
  API -->|2. PutObject| S3Service
  S3Service -->|3. AWS SDK| S3
  S3 -->|4. ETag| S3Service
  S3Service -->|5. Key/Metadata| API
  API -->|6. Success Response| UI
  
  UI -->|7. Request View| API
  API -->|8. GetSignedUrl| S3Service
  S3Service -->|9. Presigned URL (1h)| API
  API -->|10. Redirect/URL| UI
  UI -->|11. Fetch File| S3

  IAM -.->|Credentials| S3Service
```

#### Workshop Outcomes

You will have:
* S3 bucket configured with versioning and encryption.
* IAM user with restricted permissions (least privilege).
* Complete backend API for file upload/download.
* Frontend integration for uploading lab results and prescriptions.
* Presigned URL system for secure file access.

#### References
* [Amazon S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
* [AWS SDK for JavaScript v3](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/)
* [S3 Presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html)
* [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
