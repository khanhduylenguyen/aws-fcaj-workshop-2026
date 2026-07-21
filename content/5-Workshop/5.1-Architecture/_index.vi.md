---
title: "Sơ đồ kiến trúc & Tổng quan"
date: 2026-07-20
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Sơ đồ kiến trúc hệ thống

```mermaid
flowchart TB
    subgraph Client["Frontend Client"]
        WebApp[React Web App<br/>Vite + TailwindCSS]
        MobileApp[Mobile PWA<br/>Responsive Design]
    end

    subgraph AWS["AWS Cloud"]
        subgraph Storage["Lưu trữ & CDN"]
            S3[(S3 Bucket<br/>medi-path-ease-uploads)]
            CloudFront[CloudFront CDN<br/>Global Distribution]
        end

        subgraph Database["Cơ sở dữ liệu"]
            RDS[(RDS PostgreSQL<br/>Multi-AZ)]
        end

        subgraph Auth["Xác thực"]
            Cognito[Amazon Cognito<br/>User Pools]
        end

        subgraph AI["Trí tuệ nhân tạo"]
            Bedrock[Amazon Bedrock<br/>Claude 3.5 Sonnet]
        end
    end

    subgraph DigitalOcean["DigitalOcean Cloud"]
        ExpressAPI[Express.js API<br/>Node.js Backend]
        MongoDB[(MongoDB<br/>Session Cache]
    end

    subgraph Payment["Thanh toán"]
        PayOS[PayOS Gateway<br/>VNPay/QR Code]
    end

    WebApp -->|HTTPS REST API| ExpressAPI
    MobileApp -->|HTTPS REST API| ExpressAPI

    ExpressAPI -->|Query/Write| RDS
    ExpressAPI -->|Cache Session| MongoDB
    ExpressAPI -->|Upload/Download| S3
    S3 -->|CDN Cache| CloudFront

    ExpressAPI -->|Authenticate| Cognito
    Cognito -->|JWT Token| WebApp
    Cognito -->|JWT Token| MobileApp

    ExpressAPI -->|AI Chat| Bedrock
    Bedrock -->|Response| ExpressAPI

    ExpressAPI -->|Payment| PayOS
    PayOS -->|Callback| ExpressAPI
```

> **📸 [HÌNH 1]: Sơ đồ kiến trúc tổng thể hệ thống Medi Path Ease**

#### Các thành phần chính

##### 1. Frontend (Client Layer)

| Thành phần | Công nghệ | Mô tả |
|-----------|-----------|--------|
| Web App | React + Vite + TailwindCSS | Ứng dụng web responsive |
| Mobile PWA | React + PWA | Progressive Web App cho di động |
| State Management | Zustand | Quản lý state ứng dụng |

##### 2. Backend (Server Layer)

| Thành phần | Công nghệ | Vai trò |
|-----------|-----------|---------|
| API Server | Express.js + Node.js | Xử lý business logic |
| Session Cache | MongoDB | Lưu session và cache tạm |
| Job Queue | Bull + Redis | Xử lý tác vụ nền |

##### 3. Cơ sở dữ liệu (Data Layer)

| Thành phần | Dịch vụ | Mục đích |
|-----------|---------|-----------|
| Primary DB | AWS RDS PostgreSQL | Lưu trữ dữ liệu chính |
| File Storage | Amazon S3 | Lưu trữ tài liệu y tế |
| CDN | CloudFront | Phân phối file toàn cầu |

##### 4. Bảo mật & Xác thực (Security Layer)

| Thành phần | Dịch vụ | Mô tả |
|-----------|---------|--------|
| Authentication | Amazon Cognito | Xác thực người dùng |
| Authorization | JWT + RBAC | Phân quyền theo vai trò |
| Encryption | TLS 1.3 | Mã hóa truyền dữ liệu |

##### 5. AI & Chatbot (Intelligence Layer)

| Thành phần | Dịch vụ | Chức năng |
|-----------|---------|-----------|
| AI Assistant | Amazon Bedrock | Trả lời câu hỏi y tế |
| LLM Model | Claude 3.5 Sonnet | Xử lý ngôn ngữ tự nhiên |

##### 6. Thanh toán (Payment Layer)

| Thành phần | Dịch vụ | Mô tả |
|-----------|---------|--------|
| Payment Gateway | PayOS | Thanh toán VNPay/QR |
| Payment API | REST | Tích hợp vào backend |

#### Luồng dữ liệu chính

##### Luồng 1: Đăng nhập & Xác thực

```
1. User mở app → Frontend
2. Frontend gọi /api/auth/login
3. Backend → Cognito: Verify credentials
4. Cognito → Backend: Return tokens
5. Backend → Frontend: JWT access token + refresh token
6. Frontend lưu token vào localStorage
7. Các request tiếp theo attach JWT token
```

> **📸 [HÌNH 2]: Sơ đồ luồng đăng nhập**

##### Luồng 2: Upload tài liệu y tế

```
1. Doctor/Patient chọn file cần upload
2. Frontend gửi file lên /api/uploads/:folder
3. Backend nhận file → validate → upload lên S3
4. S3 trả về key và metadata
5. Backend lưu metadata vào RDS
6. Backend trả về success cho Frontend
7. Frontend hiển thị thông báo thành công
```

> **📸 [HÌNH 3]: Sơ đồ luồng upload tài liệu**

##### Luồng 3: Chat với AI Assistant

```
1. User nhập câu hỏi vào chat box
2. Frontend gửi message đến /api/chat
3. Backend gọi Bedrock API
4. Bedrock xử lý với Claude 3.5 Sonnet
5. Bedrock trả về response
6. Backend format response → Frontend
7. Frontend hiển thị câu trả lời
```

> **📸 [HÌNH 4]: Sơ đồ luồng chat AI**

#### Danh sách hình ảnh cần chuẩn bị

| STT | Mô tả hình | Tên file đề xuất |
|-----|------------|-------------------|
| 1 | Sơ đồ kiến trúc tổng thể | `arch-01-overview.png` |
| 2 | Luồng đăng nhập | `arch-02-auth-flow.png` |
| 3 | Luồng upload tài liệu | `arch-03-upload-flow.png` |
| 4 | Luồng chat AI | `arch-04-chat-flow.png` |
| 5 | Sơ đồ database schema | `arch-05-db-schema.png` |
| 6 | Dashboard admin | `arch-06-admin-dashboard.png` |

#### Thông tin môi trường

| Môi trường | URL | Ghi chú |
|------------|-----|---------|
| Development | http://localhost:5173 | Frontend local |
| Staging | https://staging.medipathease.com | Test environment |
| Production | https://mediathease.com | Live environment |

> ⚠️ **Lưu ý:** Các hình ảnh cần thêm vào thư mục `/images/5-Workshop/5.1-Architecture/` theo tên file đã đề xuất ở bảng trên.
