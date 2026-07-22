---
title : "CI/CD"
date : 2026-01-01
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

### Goal

Configure a Continuous Integration and Continuous Deployment (CI/CD) workflow for the Second-Hand Marketplace application using AWS CodeBuild.

---

## 1. Overview

In this chapter, you will configure AWS CodeBuild to automate the build process of the application.

Whenever the project source code is updated in GitHub, AWS CodeBuild builds the Docker image, pushes it to Amazon ECR, and prepares the latest container image for deployment.

This automation reduces manual deployment steps and ensures a consistent build process.

---

## 2. Detailed Practice Content

Complete the following section:

- **5.9.1 Configure AWS CodeBuild**

---

## 3. Expected Result

After completing this chapter, you will have:

- A CodeBuild project connected to GitHub.
- Docker images automatically built.
- Docker images pushed to Amazon ECR.
- A repeatable CI/CD build workflow.