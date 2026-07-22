+++
title = "Dọn dẹp tài nguyên"
date = 2024-01-01
weight = 12
chapter = false
pre = "<b>5.12. </b>"
+++

# 5.12. Dọn dẹp tài nguyên

## Tổng quan

Sau khi triển khai và kiểm thử thành công ứng dụng TechMarket, các tài nguyên AWS đã tạo trong workshop nên được xóa để tránh phát sinh chi phí không cần thiết.

Phần này hướng dẫn bạn xóa toàn bộ hạ tầng đã triển khai theo đúng thứ tự.

---

## Các bước dọn dẹp

Các tài nguyên AWS cần được xóa theo trình tự sau:

1. Amazon ECS Service
2. Amazon ECS Cluster
3. Amazon ECR Repository
4. Amazon S3 Bucket
5. Application Load Balancer
6. Target Group
7. Amazon CloudWatch Alarm
8. AWS CodeBuild Project
9. Chứng chỉ AWS Certificate Manager (ACM)
10. Amazon Route 53 Hosted Zone

---

## 1. Xóa Amazon ECS Service

Truy cập:

**Amazon ECS → Clusters → wed-mbdc-cluster → Services**

Chọn:

- wed-mbdc-service

Nhấn:

**Delete Service**

Đợi trạng thái của Service chuyển sang **Inactive**.

![Delete ECS Service](/images/5-Workshop/5.12-Cleanup/delete-ecs-service.png)

---

## 2. Xóa Amazon ECS Cluster

Truy cập:

**Amazon ECS → Clusters**

Chọn:

- wed-mbdc-cluster

Nhấn:

**Delete Cluster**

![Delete ECS Cluster](/images/5-Workshop/5.12-Cleanup/delete-cluster.png)

![Delete ECS Cluster Result](/images/5-Workshop/5.12-Cleanup/delete-ecs-cluster.png)

---

## 3. Xóa Amazon ECR Repository

Truy cập:

**Amazon ECR → Private Repositories**

Chọn:

- wed-mbdc

Nhấn:

**Delete**

Xác nhận xóa Repository.

![Delete Amazon ECR Repository](/images/5-Workshop/5.12-Cleanup/delete-ecr.png)

---

## 4. Xóa Amazon S3 Bucket

Truy cập:

**Amazon S3**

Chọn:

- wed-mbdc-uploads

Làm rỗng Bucket.

Sau đó xóa Bucket.

![Delete Amazon S3 Bucket](/images/5-Workshop/5.12-Cleanup/delete-s3-bucket.png)

---

## 5. Xóa Application Load Balancer

Truy cập:

**EC2 → Load Balancers**

Chọn:

- production-alb

Nhấn:

**Delete**

![Delete Load Balancer](/images/5-Workshop/5.12-Cleanup/delete-load-balancer.png)

---

## 6. Xóa Target Group

Truy cập:

**EC2 → Target Groups**

Chọn:

- production-tg

Nhấn:

**Delete**

![Delete Target Group](/images/5-Workshop/5.12-Cleanup/delete-target-group.png)

---

## 7. Xóa Amazon CloudWatch Alarm

Truy cập:

**Amazon CloudWatch → Alarms**

Chọn:

- production-service-cpu-alarm

Nhấn:

**Delete**

![Delete CloudWatch Alarm](/images/5-Workshop/5.12-Cleanup/delete-cloudwatch-alarm.png)

---

## 8. Xóa AWS CodeBuild Project

Truy cập:

**AWS CodeBuild → Build Projects**

Chọn:

- wed-mbdc-build

Nhấn:

**Delete**

![Delete CodeBuild Project](/images/5-Workshop/5.12-Cleanup/delete-codebuild-project.png)

![Delete CodeBuild Result](/images/5-Workshop/5.12-Cleanup/delete-codebuild.png)

---

## 9. Xóa chứng chỉ ACM

Truy cập:

**AWS Certificate Manager**

Chọn chứng chỉ được sử dụng cho tên miền của ứng dụng.

Nhấn:

**Delete**

![Delete ACM Certificate](/images/5-Workshop/5.12-Cleanup/delete-acm-certificate.png)

---

## 10. Xóa Amazon Route 53 Hosted Zone

Truy cập:

**Amazon Route 53 → Hosted Zones**

Chọn:

- techmarketstore.store

Xóa tất cả các bản ghi DNS do người dùng tạo, ngoại trừ hai bản ghi mặc định **NS** và **SOA**.

Sau đó xóa Hosted Zone.

![Delete Route 53 Hosted Zone](/images/5-Workshop/5.12-Cleanup/delete-route53-hosted-zone.png)

---

## Kết quả

Sau khi hoàn thành tất cả các bước trên:

- Amazon ECS Service đã được xóa.
- Amazon ECS Cluster đã được xóa.
- Amazon ECR Repository đã được xóa.
- Amazon S3 Bucket đã được xóa.
- Application Load Balancer đã được xóa.
- Target Group đã được xóa.
- Amazon CloudWatch Alarm đã được xóa.
- AWS CodeBuild Project đã được xóa.
- Chứng chỉ AWS Certificate Manager (ACM) đã được xóa.
- Amazon Route 53 Hosted Zone đã được xóa.

Toàn bộ tài nguyên AWS được tạo trong workshop đã được dọn dẹp thành công, giúp tránh phát sinh các chi phí không cần thiết.