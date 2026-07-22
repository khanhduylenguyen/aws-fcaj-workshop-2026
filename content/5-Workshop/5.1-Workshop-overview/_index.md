---
title : "Workshop Overview"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

### Goal

This workshop demonstrates how to deploy a cloud-native **Second-Hand Marketplace** application on AWS using containerized services, secure networking, automated deployment, and managed cloud services. After completing this workshop, you will be able to deploy a production-ready web application with high availability, scalability, and security.

---

## 1. Use Case & Solution Overview

The **Second-Hand Marketplace** is a web application that allows users to buy and sell second-hand products online. The system provides essential features such as user authentication, product management, category management, product image uploads, and product searching.

Instead of deploying the application on a traditional virtual machine, this workshop adopts a modern cloud-native architecture on AWS. The application is containerized using Docker and deployed on **Amazon ECS Fargate**, while product images are stored in **Amazon S3** and application data is stored in **MongoDB Atlas**.

To improve security and maintainability, sensitive configuration values are stored in **AWS Secrets Manager**, while **Application Load Balancer**, **Amazon Route 53**, and **AWS Certificate Manager (ACM)** provide secure public access through HTTPS. The deployment process is automated using **AWS CodeBuild**, and application health is monitored using **Amazon CloudWatch**.

---

## 2. Architecture Diagram

The architecture consists of several major components:

- Client Access
- Domain & HTTPS
- Networking Infrastructure
- Containerized Application
- Storage Services
- CI/CD Pipeline
- Monitoring & Logging

**Figure 1 – Second-Hand Marketplace Architecture**

![System Architecture](/images/5-Workshop/5.1-Workshop-overview/system_architecture.jpg)

---

## 3. System Workflow

The application processes user requests through the following workflow:

1. Users access the application using a custom domain managed by **Amazon Route 53**.

2. HTTPS certificates issued by **AWS Certificate Manager (ACM)** encrypt all communications.

3. Incoming requests are routed through the **Application Load Balancer (ALB)**.

4. The ALB forwards traffic to containerized services running on **Amazon ECS Fargate**.

5. The Node.js application processes business logic and communicates with **MongoDB Atlas** to store and retrieve application data.

6. Product images are uploaded and stored in **Amazon S3**.

7. Sensitive application configuration such as database credentials is retrieved securely from **AWS Secrets Manager**.

8. Application logs and metrics are collected by **Amazon CloudWatch** for monitoring and troubleshooting.

9. When source code is pushed to GitHub, **AWS CodeBuild** automatically builds a Docker image, pushes it to **Amazon ECR**, and deploys the latest version to **Amazon ECS**.

---

## 4. In-Scope Services

The AWS services implemented in this workshop include:

### Networking

- Amazon VPC
- Public Subnet
- Private Subnet
- Internet Gateway
- NAT Gateway
- Security Groups

### Compute

- Amazon ECS Fargate
- Application Load Balancer

### Storage

- Amazon S3
- MongoDB Atlas

### Container

- Docker
- Amazon Elastic Container Registry (Amazon ECR)

### Security

- AWS IAM
- AWS Secrets Manager
- AWS Certificate Manager (ACM)

### Domain

- Amazon Route 53

### CI/CD

- GitHub
- AWS CodeBuild

### Monitoring

- Amazon CloudWatch

---

## 5. Expected Outcomes

Upon completing this workshop, you will be able to:

- Deploy a containerized Node.js application on Amazon ECS Fargate.
- Configure a secure networking environment using Amazon VPC.
- Store application data in MongoDB Atlas.
- Store product images in Amazon S3.
- Secure sensitive application configuration using AWS Secrets Manager.
- Configure HTTPS using ACM and Route 53.
- Build an automated deployment pipeline using GitHub, CodeBuild, Amazon ECR, and Amazon ECS.
- Monitor application logs and system health using Amazon CloudWatch.
- Remove all AWS resources to avoid unnecessary costs.