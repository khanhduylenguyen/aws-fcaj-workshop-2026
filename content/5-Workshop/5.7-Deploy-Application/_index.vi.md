---
title : "Triển khai ứng dụng"
date : 2026-01-01
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

### Mục tiêu

Triển khai ứng dụng Second-Hand Marketplace lên Amazon ECS sử dụng AWS Fargate.

---

## 1. Tổng quan

Trong chương này, bạn sẽ triển khai ứng dụng đã được đóng gói thành container lên Amazon Elastic Container Service (Amazon ECS).

Amazon ECS quản lý các container của ứng dụng, trong khi AWS Fargate cung cấp môi trường chạy serverless mà không cần quản lý máy chủ EC2.

Quá trình triển khai bao gồm tạo ECS Cluster, cấu hình Task Definition, tạo ECS Service và kiểm tra ứng dụng sau khi triển khai.

---

## 2. Nội dung thực hành

Thực hiện lần lượt các phần sau:

- **5.7.1 Tạo ECS Cluster**
- **5.7.2 Tạo Task Definition**
- **5.7.3 Tạo ECS Service**
- **5.7.4 Kiểm tra triển khai**

---

## 3. Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ có:

- Một Amazon ECS Cluster được tạo.
- Task Definition được cấu hình.
- ECS Service chạy trên AWS Fargate.
- Ứng dụng được triển khai thành công và sẵn sàng phục vụ người dùng.