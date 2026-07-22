---
title : "Application Services"
date : 2026-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

### Goal

Configure the core application services required for the Second-Hand Marketplace application.

---

## 1. Overview

In this chapter, you will configure the core services that support the application running on Amazon ECS.

The application uses MongoDB Atlas as the database, Amazon S3 to store product images, and AWS Secrets Manager to securely manage sensitive information such as database connection strings and application secrets.

After completing this chapter, the application will be ready to access external services securely and reliably.

---

## 2. Detailed Practice Content

Complete the following sections in order:

- **5.5.1 Configure MongoDB Atlas**
- **5.5.2 Configure Amazon S3**
- **5.5.3 Configure AWS Secrets Manager**

---

## 3. Expected Result

After completing this chapter, you will have:

- MongoDB Atlas configured for application data.
- Amazon S3 configured for storing product images.
- AWS Secrets Manager configured for secure secret management.
- All application services ready for deployment on Amazon ECS.