+++
title = "Cleanup Resources"
date = 2024-01-01
weight = 12
chapter = false
pre = "<b>5.12. </b>"
+++

# 5.12. Cleanup

## Overview

After successfully deploying and testing the TechMarket application, the AWS resources created during this workshop should be removed to avoid unnecessary charges.

This section guides you through deleting the deployed infrastructure in a safe order.

---

## Cleanup Steps

The following AWS resources should be removed in sequence:

1. Amazon ECS Service
2. Amazon ECS Cluster
3. Amazon ECR Repository
4. Amazon S3 Bucket
5. Application Load Balancer
6. Target Group
7. Amazon CloudWatch Alarm
8. AWS CodeBuild Project
9. AWS Certificate Manager (ACM) Certificate
10. Amazon Route 53 Hosted Zone

---

## 1. Delete ECS Service

Navigate to:

**Amazon ECS → Clusters → wed-mbdc-cluster → Services**

Select:

- wed-mbdc-service

Choose:

**Delete Service**

Wait until the service status becomes **Inactive**.

![Delete ECS Service](/images/5-Workshop/5.12-Cleanup/delete-ecs-service.png)

---

## 2. Delete ECS Cluster

Navigate to:

**Amazon ECS → Clusters**

Select:

- wed-mbdc-cluster

Choose:

**Delete Cluster**

![Delete ECS Cluster](/images/5-Workshop/5.12-Cleanup/delete-cluster.png)

![Delete ECS Cluster Result](/images/5-Workshop/5.12-Cleanup/delete-ecs-cluster.png)

---

## 3. Delete Amazon ECR Repository

Navigate to:

**Amazon ECR → Private Repositories**

Select:

- wed-mbdc

Choose:

**Delete**

Confirm repository deletion.

![Delete Amazon ECR Repository](/images/5-Workshop/5.12-Cleanup/delete-ecr.png)

---

## 4. Delete Amazon S3 Bucket

Navigate to:

**Amazon S3**

Select:

- wed-mbdc-uploads

Empty the bucket.

Delete the bucket.

![Delete Amazon S3 Bucket](/images/5-Workshop/5.12-Cleanup/delete-s3-bucket.png)

---

## 5. Delete Application Load Balancer

Navigate to:

**EC2 → Load Balancers**

Select:

- production-alb

Choose:

**Delete**

![Delete Load Balancer](/images/5-Workshop/5.12-Cleanup/delete-load-balancer.png)

---

## 6. Delete Target Group

Navigate to:

**EC2 → Target Groups**

Select:

- production-tg

Choose:

**Delete**

![Delete Target Group](/images/5-Workshop/5.12-Cleanup/delete-target-group.png)

---

## 7. Delete Amazon CloudWatch Alarm

Navigate to:

**Amazon CloudWatch → Alarms**

Select:

- production-service-cpu-alarm

Choose:

**Delete**

![Delete CloudWatch Alarm](/images/5-Workshop/5.12-Cleanup/delete-cloudwatch-alarm.png)

---

## 8. Delete AWS CodeBuild Project

Navigate to:

**AWS CodeBuild → Build Projects**

Select:

- wed-mbdc-build

Choose:

**Delete**

![Delete CodeBuild Project](/images/5-Workshop/5.12-Cleanup/delete-codebuild-project.png)

![Delete CodeBuild Result](/images/5-Workshop/5.12-Cleanup/delete-codebuild.png)

---

## 9. Delete ACM Certificate

Navigate to:

**AWS Certificate Manager**

Select the certificate associated with the application domain.

Choose:

**Delete**

![Delete ACM Certificate](/images/5-Workshop/5.12-Cleanup/delete-acm-certificate.png)

---

## 10. Delete Amazon Route 53 Hosted Zone

Navigate to:

**Amazon Route 53 → Hosted Zones**

Select:

- techmarketstore.store

Delete all custom DNS records except the default **NS** and **SOA** records.

Delete the hosted zone.

![Delete Route 53 Hosted Zone](/images/5-Workshop/5.12-Cleanup/delete-route53-hosted-zone.png)

---

## Result

After completing all cleanup steps:

- Amazon ECS Service has been deleted.
- Amazon ECS Cluster has been deleted.
- Amazon ECR repository has been removed.
- Amazon S3 bucket has been removed.
- Application Load Balancer has been removed.
- Target Group has been removed.
- Amazon CloudWatch alarm has been removed.
- AWS CodeBuild project has been removed.
- AWS Certificate Manager certificate has been removed.
- Amazon Route 53 hosted zone has been removed.

All AWS resources created during this workshop have been cleaned up successfully, preventing unnecessary AWS charges.