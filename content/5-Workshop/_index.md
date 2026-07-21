---
title: "Workshop"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Building Medi Path Ease Telemedicine System

#### Overview

**Medi Path Ease** is a telemedicine application designed to connect doctors and patients easily, securely, and effectively. The system provides key features:

* **Patient Record Management** - Securely store and retrieve medical information
* **Remote Consultation (Telemedicine)** - Connect patients with doctors via video calls and chat
* **Medical Document Storage** - Upload and view lab results, prescriptions, medical records
* **AI Virtual Assistant** - Support answering common health questions
* **Online Payment** - PayOS payment gateway integration

#### AWS & Cloud Services Used

| Service | Purpose |
|---------|---------|
| **Amazon RDS PostgreSQL** | Main relational database for the application |
| **Amazon S3** | Store medical documents (lab results, prescriptions, EHR) |
| **Amazon CloudFront** | Global CDN for static content delivery |
| **Amazon Cognito** | User authentication and management |
| **Amazon Bedrock** | AI chatbot virtual assistant |
| **DigitalOcean App Platform** | Node.js/Express application server |
| **PayOS** | VNPay/QrCode payment gateway |

#### Workshop Outcomes

After completing this workshop, you will have a complete telemedicine system with:
* Express.js backend running on DigitalOcean
* PostgreSQL database on AWS RDS
* Secure file storage with S3 + CloudFront
* User authentication with Cognito + JWT
* AI chatbot supporting patients
* Online payment via PayOS

#### Content

1. [Architecture Diagram & Overview](5.1-Architecture/)
2. [RDS PostgreSQL Database Configuration](5.2-RDS-PostgreSQL/)
3. [S3 Storage & CloudFront Service](5.3-S3-CloudFront/)
4. [User Authentication with Amazon Cognito](5.4-Cognito/)
5. [AI Chatbot with Amazon Bedrock](5.5-Bedrock-AI/)
6. [DigitalOcean Configuration & PayOS Integration](5.6-DigitalOcean-PayOS/)
7. [Demo - Real Application Interface](5.7-Demo/)
8. [Cleanup Resources](5.8-Cleanup/)
