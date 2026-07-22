---
title: "Proposal"
date: 2026-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Second-Hand Marketplace Platform
## A Cloud-Native Second-Hand Marketplace on AWS

## 1. Executive Summary

Second-Hand Marketplace Platform is a cloud-based web application that enables users to buy and sell second-hand products through a centralized online marketplace. The system provides user authentication, product management, image uploading, searching, and category management while utilizing AWS cloud services to ensure scalability, availability, and simplified deployment.

The application is developed using **Node.js**, **Express.js**, **MongoDB Atlas**, and **EJS**, while the infrastructure is deployed on **Amazon ECS Fargate** behind an **Application Load Balancer**. Docker containers are stored in **Amazon ECR**, product images are stored in **Amazon S3**, and **AWS CodeBuild** automatically builds and deploys the application whenever new code is pushed to GitHub. The architecture provides automatic deployment, centralized storage, and cost-effective cloud infrastructure suitable for small and medium-sized e-commerce applications.

---

# 2. Problem Statement

### Current Problem

Many second-hand marketplaces rely on social media platforms or manually managed websites, making product management inefficient and difficult to maintain. Images are often stored locally, deployments require manual updates, and scaling the application becomes challenging as the number of users increases. Traditional deployment also increases downtime and operational effort whenever new features or bug fixes are released.

### Solution

The proposed solution builds a cloud-native second-hand marketplace platform using AWS managed services.

Users can register accounts, log in securely, upload products with images, browse products by category, search products, and manage their listings through a web application. Product information is stored in MongoDB Atlas, while uploaded product images are stored in Amazon S3.

The application is containerized using Docker and deployed on Amazon ECS Fargate. AWS CodeBuild automatically detects new commits from GitHub, builds a Docker image, pushes it to Amazon ECR, and triggers Amazon ECS to deploy the latest version without manual intervention.

### Benefits and Return on Investment

The platform significantly reduces deployment time through automated CI/CD while improving system scalability and reliability. Developers only need to push source code to GitHub, and the entire deployment process is handled automatically. The cloud architecture also minimizes server management tasks and provides a strong foundation for future expansion. During development, AWS Free Tier and AWS Academy credits help reduce operational costs.

---

# 3. Solution Architecture

The application follows a cloud-native container architecture using AWS managed services.

## Solution Architecture

![System Architecture](/images/2-Proposal/system_architecture.jpg)

### AWS Services Utilized

- **Amazon ECS Fargate** – Runs Docker containers without managing servers.
- **Amazon ECR** – Stores Docker images used by ECS.
- **AWS CodeBuild** – Automatically builds and deploys new application versions from GitHub.
- **Application Load Balancer (ALB)** – Distributes incoming HTTP requests across ECS tasks.
- **Amazon S3** – Stores uploaded product images.
- **MongoDB Atlas** – Stores application data including users, products, and categories.
- **AWS IAM** – Manages permissions for AWS resources.
- **Amazon CloudWatch** – Collects application logs and monitoring information.

### Component Design

**Frontend**

- HTML
- CSS
- Bootstrap
- JavaScript
- EJS Template Engine

**Backend**

- Node.js
- Express.js
- Express Session
- Multer
- AWS SDK for JavaScript

**Database**

- MongoDB Atlas

**Image Storage**

- Amazon S3

**Container Platform**

- Docker
- Amazon ECS Fargate

**Deployment Pipeline**

- GitHub
- AWS CodeBuild
- Amazon ECR
- Amazon ECS

---

# 4. Technical Implementation

## Implementation Phases

The project was implemented through the following phases:

- Research AWS cloud architecture and deployment strategy.
- Design the overall marketplace system architecture.
- Develop the backend using Node.js and Express.js.
- Configure MongoDB Atlas for cloud database storage.
- Integrate Amazon S3 for product image storage.
- Dockerize the application.
- Push Docker images to Amazon ECR.
- Deploy Docker containers on Amazon ECS Fargate.
- Configure Application Load Balancer.
- Implement Continuous Integration and Continuous Deployment using GitHub and AWS CodeBuild.
- Perform testing, optimization, and production deployment.

## Technical Requirements

### Programming Languages

- JavaScript
- HTML
- CSS

### Frameworks

- Express.js
- EJS

### Database

- MongoDB Atlas

### Cloud Services

- Amazon ECS Fargate
- Amazon ECR
- Amazon S3
- AWS CodeBuild
- Application Load Balancer
- Amazon CloudWatch
- AWS IAM

### Development Tools

- Visual Studio Code
- Git
- GitHub
- Docker Desktop
- MongoDB Compass

---

# 5. Roadmap & Milestones

### Week 1

- Requirement analysis
- AWS architecture planning
- Database design

### Week 2

- User authentication
- Product CRUD
- Category management

### Week 3

- MongoDB Atlas integration
- Amazon S3 integration
- Product image upload

### Week 4

- Docker containerization
- Amazon ECR configuration

### Week 5

- Amazon ECS deployment
- Application Load Balancer configuration

### Week 6

- GitHub integration
- AWS CodeBuild configuration
- Automatic deployment pipeline

### Week 7

- Testing
- Bug fixing
- Performance optimization

### Week 8

- Final deployment
- Documentation
- Project presentation

---

# 6. Budget Estimation

## Infrastructure Cost Estimate

| Service | Estimated Cost |
|----------|----------------|
| Amazon ECS Fargate | ~$0.25/month |
| Amazon S3 (Storage & Requests) | ~$0.15/month |
| Amazon ECR | ~$0.03/month |
| AWS CodeBuild | ~$0.05/month |
| Application Load Balancer | ~$0.10/month |
| Amazon CloudWatch | ~$0.02/month |
| **Total Estimate** | **~$0.60 USD/month** |

### Cost Control Guidelines

- **AWS Budgets:** Automated alerts when costs exceed **$5.00** and **$10.00**.
- **Amazon S3 Lifecycle Rules:** Automatically delete unused product images after **30 days**.
- **Amazon ECR Lifecycle Policy:** Automatically remove unused Docker images.
- **AWS CodeBuild:** Trigger builds only when code is pushed to the GitHub repository.
- **Post-demo Cleanup:** Remove ECS services, ECR images, and unused S3 objects after project completion to avoid unnecessary charges.

# 7. Risk Assessment

## Risk Matrix

- ECS deployment failure.
- MongoDB Atlas connectivity issues.
- Amazon S3 upload failures.
- Unexpected AWS service charges.

## Mitigation Strategies

- Enable Amazon CloudWatch monitoring.
- Configure AWS Budgets alerts.
- Version Docker images using Amazon ECR.
- Perform regular MongoDB Atlas backups.
- Apply IAM least privilege policies.

## Contingency Plans

- Roll back to the previous Docker image.
- Redeploy the previous ECS Task Definition.
- Restore MongoDB Atlas backups.
- Redeploy through AWS CodeBuild.

---

# 8. Expected Results

## Technical Results

The completed project will provide:

- A fully containerized cloud-native marketplace platform.
- Automatic CI/CD deployment using GitHub and AWS CodeBuild.
- Reliable image storage using Amazon S3.
- Scalable container deployment using Amazon ECS Fargate.
- Centralized cloud database using MongoDB Atlas.
- Production-ready cloud architecture with load balancing.

## Business Value

The platform demonstrates practical implementation of cloud computing, containerization, and DevOps practices using AWS managed services. The architecture can be expanded in the future to support payment gateways, recommendation systems, notification services, analytics, and microservice-based development while maintaining scalability and operational efficiency.