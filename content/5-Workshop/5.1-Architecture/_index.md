---
title: "Architecture Diagram & Overview"
date: 2026-07-20
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### System Architecture Diagram

```mermaid
flowchart TB
    subgraph Client["Frontend Client"]
        WebApp[React Web App<br/>Vite + TailwindCSS]
        MobileApp[Mobile PWA<br/>Responsive Design]
    end

    subgraph AWS["AWS Cloud"]
        subgraph Storage["Storage & CDN"]
            S3[(S3 Bucket<br/>medi-path-ease-uploads)]
            CloudFront[CloudFront CDN<br/>Global Distribution]
        end

        subgraph Database["Database"]
            RDS[(RDS PostgreSQL<br/>Multi-AZ)]
        end

        subgraph Auth["Authentication"]
            Cognito[Amazon Cognito<br/>User Pools]
        end

        subgraph AI["Artificial Intelligence"]
            Bedrock[Amazon Bedrock<br/>Claude 3.5 Sonnet]
        end
    end

    subgraph DigitalOcean["DigitalOcean Cloud"]
        ExpressAPI[Express.js API<br/>Node.js Backend]
        MongoDB[(MongoDB<br/>Session Cache]
    end

    subgraph Payment["Payment"]
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

> **📸 [IMAGE 1]: Overall system architecture diagram for Medi Path Ease**

#### Main Components

##### 1. Frontend (Client Layer)

| Component | Technology | Description |
|-----------|-----------|-------------|
| Web App | React + Vite + TailwindCSS | Responsive web application |
| Mobile PWA | React + PWA | Progressive Web App for mobile |
| State Management | Zustand | Application state management |

##### 2. Backend (Server Layer)

| Component | Technology | Role |
|-----------|-----------|------|
| API Server | Express.js + Node.js | Business logic processing |
| Session Cache | MongoDB | Session and cache storage |
| Job Queue | Bull + Redis | Background job processing |

##### 3. Database (Data Layer)

| Component | Service | Purpose |
|-----------|---------|---------|
| Primary DB | AWS RDS PostgreSQL | Main data storage |
| File Storage | Amazon S3 | Medical document storage |
| CDN | CloudFront | Global file distribution |

##### 4. Security & Authentication (Security Layer)

| Component | Service | Description |
|-----------|---------|-------------|
| Authentication | Amazon Cognito | User authentication |
| Authorization | JWT + RBAC | Role-based access control |
| Encryption | TLS 1.3 | Data transmission encryption |

##### 5. AI & Chatbot (Intelligence Layer)

| Component | Service | Function |
|-----------|---------|----------|
| AI Assistant | Amazon Bedrock | Answer medical questions |
| LLM Model | Claude 3.5 Sonnet | Natural language processing |

##### 6. Payment (Payment Layer)

| Component | Service | Description |
|-----------|---------|-------------|
| Payment Gateway | PayOS | VNPay/QR payment |
| Payment API | REST | Backend integration |

#### Main Data Flows

##### Flow 1: Login & Authentication

```
1. User opens app → Frontend
2. Frontend calls /api/auth/login
3. Backend → Cognito: Verify credentials
4. Cognito → Backend: Return tokens
5. Backend → Frontend: JWT access token + refresh token
6. Frontend saves token in localStorage
7. Subsequent requests attach JWT token
```

> **📸 [IMAGE 2]: Login flow diagram**

##### Flow 2: Medical Document Upload

```
1. Doctor/Patient selects file to upload
2. Frontend sends file to /api/uploads/:folder
3. Backend receives file → validates → uploads to S3
4. S3 returns key and metadata
5. Backend saves metadata to RDS
6. Backend returns success to Frontend
7. Frontend shows success notification
```

> **📸 [IMAGE 3]: Document upload flow diagram**

##### Flow 3: Chat with AI Assistant

```
1. User enters question in chat box
2. Frontend sends message to /api/chat
3. Backend calls Bedrock API
4. Bedrock processes with Claude 3.5 Sonnet
5. Bedrock returns response
6. Backend formats response → Frontend
7. Frontend displays answer
```

> **📸 [IMAGE 4]: AI chat flow diagram**

#### Required Images List

| No. | Image Description | Suggested Filename |
|-----|-----------------|-------------------|
| 1 | Overall architecture | `arch-01-overview.png` |
| 2 | Login flow | `arch-02-auth-flow.png` |
| 3 | Document upload flow | `arch-03-upload-flow.png` |
| 4 | AI chat flow | `arch-04-chat-flow.png` |
| 5 | Database schema | `arch-05-db-schema.png` |
| 6 | Admin dashboard | `arch-06-admin-dashboard.png` |

#### Environment Information

| Environment | URL | Notes |
|------------|-----|-------|
| Development | http://localhost:5173 | Local frontend |
| Staging | https://staging.medipathease.com | Test environment |
| Production | https://mediathease.com | Live environment |

> ⚠️ **Note:** Images should be added to `/images/5-Workshop/5.1-Architecture/` directory with the suggested filenames from the table above.
