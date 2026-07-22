---
title : "Containerization"
date : 2026-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

### Goal

Containerize the Second-Hand Marketplace application and prepare the container image for deployment on Amazon ECS.

---

## 1. Overview

In this chapter, you will package the Node.js application into a Docker container and upload the container image to Amazon Elastic Container Registry (Amazon ECR).

Docker provides a consistent runtime environment, while Amazon ECR securely stores container images that will later be deployed to Amazon ECS.

---

## 2. Detailed Practice Content

Complete the following sections in order:

- **5.6.1 Create Dockerfile**
- **5.6.2 Build Docker Image**
- **5.6.3 Push Image to Amazon ECR**

---

## 3. Expected Result

After completing this chapter, you will have:

- A Dockerfile for the application.
- A Docker image built successfully.
- A container image stored in Amazon ECR.
- A container image ready for deployment on Amazon ECS.