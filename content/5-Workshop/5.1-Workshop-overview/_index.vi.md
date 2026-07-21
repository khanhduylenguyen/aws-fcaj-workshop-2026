---
title : "Giới thiệu workshop"
date : 2026-07-20
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về Amazon S3 trong Medi Path Ease

**Amazon S3 (Simple Storage Service)** là dịch vụ lưu trữ object của AWS, được sử dụng trong Medi Path Ease để lưu trữ các tài liệu y tế quan trọng của bệnh nhân.

#### Các tính năng S3 được sử dụng

* **Lưu trữ kết quả xét nghiệm (Lab Results):** Upload và truy xuất file PDF kết quả xét nghiệm máu, nước tiểu, và các xét nghiệm chuyên sâu.
* **Lưu trữ đơn thuốc (Prescriptions):** Lưu trữ hình ảnh đơn thuốc được chụp hoặc scan.
* **Lưu trữ hồ sơ điện tử (EHR):** Lưu trữ các tài liệu hồ sơ bệnh án điện tử.
* **File đính kèm telemedicine:** Lưu trữ hình ảnh, tài liệu được chia sẻ trong buổi khám từ xa.
* **Presigned URLs:** Tạo URL tạm thời để bệnh nhân và bác sĩ xem file mà không cần public access.

#### Tại sao dùng S3 thay vì lưu trữ trong database?

* **Bảo mật:** File được lưu trữ riêng biệt với database, chỉ truy cập qua presigned URL có thời hạn.
* **Hiệu suất:** Object storage tối ưu cho file lớn (PDF, hình ảnh) hơn so với lưu trữ trong MongoDB.
* **Chi phí:** S3 Standard rẻ hơn so với MongoDB Atlas cho việc lưu trữ file lớn.
* **Durability:** 99.999999999% (11 nines) độ bền dữ liệu.
* **Versioning:** Có thể khôi phục file nếu bị xóa nhầm.

#### Cấu trúc folder trong S3 bucket

```
medi-path-ease-uploads/
├── lab-results/           # Kết quả xét nghiệm
│   └── <patientId>/
│       └── <yyyy>/
│           └── <mm>/
├── prescriptions/         # Đơn thuốc
│   └── <patientId>/
│       └── <yyyy>/
│           └── <mm>/
├── ehr/                  # Hồ sơ bệnh án điện tử
│   └── <patientId>/
│       └── <yyyy>/
│           └── <mm>/
└── chat-attachments/     # File đính kèm chat telemedicine
    └── <patientId>/
        └── <yyyy>/
            └── <mm>/
```

#### Tổng quan về workshop

Trong workshop này, bạn sẽ cấu hình và tích hợp Amazon S3 vào hệ thống Medi Path Ease:

* **Cloud side (AWS):** Tạo S3 bucket với cấu hình bảo mật, IAM policies, presigned URLs.
* **Backend side:** Tích hợp AWS SDK vào Express server, xây dựng API upload/download.
* **Frontend side:** Kết nối React UI với backend để upload file và hiển thị presigned URLs.

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

#### Kết quả sau workshop

Bạn sẽ có:
* S3 bucket được cấu hình với versioning và encryption.
* IAM user với quyền hạn chế (least privilege).
* Backend API hoàn chỉnh cho upload/download file.
* Frontend tích hợp để upload kết quả xét nghiệm và đơn thuốc.
* Presigned URL system để bảo mật truy cập file.

#### Tài liệu tham khảo
* [Amazon S3 Documentation](https://docs.aws.amazon.com/s3/index.html)
* [AWS SDK for JavaScript v3](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/)
* [S3 Presigned URLs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/PresignedUrlUploadObject.html)
* [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)
