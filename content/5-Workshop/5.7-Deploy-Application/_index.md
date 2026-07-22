---
title : "Deploy Application"
date : 2026-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

### Goal

Deploy the Second-Hand Marketplace application to Amazon ECS using AWS Fargate.

---

## 1. Overview

In this chapter, you will deploy the containerized application to Amazon Elastic Container Service (Amazon ECS).

Amazon ECS manages the application containers, while AWS Fargate provides a serverless compute environment without requiring you to provision or manage EC2 instances.

The deployment includes creating an ECS cluster, configuring a task definition, creating an ECS service, and verifying that the application is running successfully.

---

## 2. Detailed Practice Content

Complete the following sections in order:

- **5.7.1 Create ECS Cluster**
- **5.7.2 Create Task Definition**
- **5.7.3 Create ECS Service**
- **5.7.4 Verify Deployment**

---

## 3. Expected Result

After completing this chapter, you will have:

- An Amazon ECS cluster created.
- A task definition configured.
- An ECS service running on AWS Fargate.
- The application successfully deployed and ready to receive traffic.