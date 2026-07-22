---
title : "Push Image to Amazon ECR"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2. </b> "
---

## Push Image to Amazon ECR

In this section, you will push the Docker image to Amazon Elastic Container Registry (Amazon ECR).

Amazon ECR is a managed container image registry that securely stores Docker images. Amazon ECS will later pull the image from this repository during deployment.

---

## Create an Amazon ECR Repository

Navigate to:

**AWS Console → Amazon ECR → Private repositories → Create repository**

Configure the repository with the following settings.

| Property | Value |
|----------|-------|
| Visibility settings | Private |
| Repository name | secondhand-marketplace |

Choose **Create repository**.

![Create Repository](/images/5-Workshop/5.6-Containerization/create-repository.png)

---

## Authenticate Docker to Amazon ECR

Open a terminal and authenticate Docker with Amazon ECR.

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com
```

After successful authentication, Docker displays:

```text
Login Succeeded
```

---

## Tag the Docker Image

Tag the local Docker image using the Amazon ECR repository URI.

```bash
docker tag secondhand-marketplace:latest <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/secondhand-marketplace:latest
```

---

## Push the Docker Image

Upload the Docker image to Amazon ECR.

```bash
docker push <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/secondhand-marketplace:latest
```

Docker uploads each image layer to Amazon ECR. Depending on the image size and network connection, this process may take several minutes.

---

## Verify the Repository

Navigate to:

**AWS Console → Amazon ECR → Private repositories**

Open the repository and verify that the image has been uploaded successfully.

![Repository Images](/images/5-Workshop/5.6-Containerization/repository-images.png)

---

## Expected Result

After completing this section, you will have:

- An Amazon ECR repository created.
- Docker authenticated with Amazon ECR.
- The Docker image uploaded successfully.
- A container image ready for deployment on Amazon ECS.