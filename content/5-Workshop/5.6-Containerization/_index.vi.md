---
title : "Đóng gói ứng dụng"
date : 2026-01-01
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

### Mục tiêu

Đóng gói ứng dụng Second-Hand Marketplace thành Docker container và chuẩn bị image để triển khai trên Amazon ECS.

---

## 1. Tổng quan

Trong chương này, bạn sẽ đóng gói ứng dụng Node.js thành Docker container và tải container image lên Amazon Elastic Container Registry (Amazon ECR).

Docker giúp tạo môi trường chạy thống nhất cho ứng dụng, trong khi Amazon ECR lưu trữ container image để sử dụng khi triển khai lên Amazon ECS.

---

## 2. Nội dung thực hành

Thực hiện lần lượt các phần sau:

- **5.6.1 Tạo Dockerfile**
- **5.6.2 Build Docker Image**
- **5.6.3 Đẩy Image lên Amazon ECR**

---

## 3. Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ có:

- Dockerfile cho ứng dụng.
- Docker Image được build thành công.
- Docker Image được lưu trữ trên Amazon ECR.
- Docker Image sẵn sàng để triển khai trên Amazon ECS.