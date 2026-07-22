---
title : "Prerequisite"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Goal

Ensure readers have access to the AWS Management Console, prepare the required local development tools, and clone the project source code before starting the deployment.

---

## 1. Tools to Prepare

This workshop uses the **AWS Management Console (Web UI)** to create and manage AWS resources. No AWS CLI or Infrastructure as Code (IaC) tools are required.

Please prepare the following software:

- **Node.js (v18+)**: Required for running the application locally.
- **Git**: Source control management.
- **Docker Desktop**: Required for building Docker images.
- **Visual Studio Code (or any preferred IDE)**: For editing the project source code.

---

## 2. Steps

**Log in to AWS Console:** Sign in to the AWS Management Console using your AWS account and ensure the Region is set to **ap-southeast-1 (Singapore)**.

**Checkpoint:** Confirm that the console is using **ap-southeast-1** before creating any resources.

**Verify Local Tools:** Make sure the required software is installed successfully.

```bash
node --version
npm --version
git --version
docker --version
```

**Checkpoint:** All commands should return valid version numbers.

**Clone the Project:** Clone the project source code from GitHub and open it using Visual Studio Code.

**Checkpoint:** The project can be opened successfully and is ready for deployment.

---

## 3. Expected Result

- Successfully access the AWS Management Console.
- Prepare the local development environment.
- Clone the project source code successfully.
- Complete all prerequisites before proceeding to the next chapter.