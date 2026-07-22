---
title : "Tổng quan Workshop"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

### Mục tiêu

Workshop này hướng dẫn triển khai ứng dụng **Second-Hand Marketplace** trên nền tảng AWS bằng cách sử dụng kiến trúc Cloud-Native, các dịch vụ được quản lý (Managed Services), triển khai container và quy trình CI/CD tự động. Sau khi hoàn thành workshop, bạn sẽ có thể triển khai một ứng dụng web hoàn chỉnh với khả năng mở rộng, tính sẵn sàng cao và bảo mật.

---

## 1. Giới thiệu bài toán và giải pháp

**Second-Hand Marketplace** là một ứng dụng web cho phép người dùng đăng bán, tìm kiếm và mua các sản phẩm đã qua sử dụng. Hệ thống hỗ trợ các chức năng như đăng ký tài khoản, đăng nhập, quản lý danh mục, quản lý sản phẩm, tải lên hình ảnh sản phẩm và tìm kiếm sản phẩm.

Thay vì triển khai ứng dụng trên một máy chủ truyền thống, workshop này áp dụng kiến trúc Cloud-Native trên AWS. Ứng dụng được đóng gói bằng **Docker** và triển khai trên **Amazon ECS Fargate**, hình ảnh sản phẩm được lưu trữ trên **Amazon S3**, trong khi dữ liệu được lưu trữ trên **MongoDB Atlas**.

Để tăng cường tính bảo mật và khả năng quản lý, các thông tin nhạy cảm được lưu trong **AWS Secrets Manager**. Đồng thời, **Application Load Balancer**, **Amazon Route 53** và **AWS Certificate Manager (ACM)** được sử dụng để cung cấp truy cập an toàn thông qua giao thức HTTPS. Quá trình triển khai được tự động hóa bằng **AWS CodeBuild**, và hệ thống được giám sát thông qua **Amazon CloudWatch**.

---

## 2. Kiến trúc hệ thống

Kiến trúc của hệ thống bao gồm các thành phần chính sau:

- Người dùng (Client)
- Tên miền và HTTPS
- Hạ tầng mạng
- Ứng dụng chạy trên Container
- Dịch vụ lưu trữ
- Quy trình CI/CD
- Giám sát hệ thống

**Hình 1 – Kiến trúc hệ thống Second-Hand Marketplace**

![Kiến trúc hệ thống](/images/5-Workshop/5.1-Workshop-overview/system_architecture.jpg)

---

## 3. Quy trình hoạt động của hệ thống

Luồng xử lý chính của hệ thống diễn ra theo các bước sau:

1. Người dùng truy cập website thông qua tên miền được quản lý bởi **Amazon Route 53**.

2. **AWS Certificate Manager (ACM)** cung cấp chứng chỉ SSL/TLS để mã hóa toàn bộ kết nối HTTPS.

3. Mọi yêu cầu từ người dùng được chuyển đến **Application Load Balancer (ALB)**.

4. ALB phân phối lưu lượng truy cập đến các container đang chạy trên **Amazon ECS Fargate**.

5. Ứng dụng Node.js xử lý nghiệp vụ và giao tiếp với **MongoDB Atlas** để lưu trữ cũng như truy xuất dữ liệu.

6. Hình ảnh sản phẩm được tải lên và lưu trữ trên **Amazon S3**.

7. Các thông tin cấu hình nhạy cảm như chuỗi kết nối cơ sở dữ liệu được lấy từ **AWS Secrets Manager**.

8. Nhật ký hoạt động (Logs) và các chỉ số hệ thống (Metrics) được gửi đến **Amazon CloudWatch** để phục vụ việc giám sát và xử lý sự cố.

9. Khi mã nguồn được cập nhật lên GitHub, **AWS CodeBuild** sẽ tự động xây dựng Docker Image, đẩy Image lên **Amazon ECR** và triển khai phiên bản mới lên **Amazon ECS**.

---

## 4. Các dịch vụ được sử dụng

Workshop sử dụng các dịch vụ AWS sau:

### Hạ tầng mạng

- Amazon VPC
- Public Subnet
- Private Subnet
- Internet Gateway
- NAT Gateway
- Security Groups

### Dịch vụ tính toán

- Amazon ECS Fargate
- Application Load Balancer

### Lưu trữ

- Amazon S3
- MongoDB Atlas

### Container

- Docker
- Amazon Elastic Container Registry (Amazon ECR)

### Bảo mật

- AWS IAM
- AWS Secrets Manager
- AWS Certificate Manager (ACM)

### Tên miền

- Amazon Route 53

### CI/CD

- GitHub
- AWS CodeBuild

### Giám sát

- Amazon CloudWatch

---

## 5. Kết quả đạt được

Sau khi hoàn thành workshop, bạn sẽ có thể:

- Triển khai ứng dụng Node.js dưới dạng container trên Amazon ECS Fargate.
- Xây dựng hạ tầng mạng bằng Amazon VPC.
- Kết nối và sử dụng MongoDB Atlas làm cơ sở dữ liệu.
- Lưu trữ hình ảnh sản phẩm trên Amazon S3.
- Bảo vệ thông tin cấu hình bằng AWS Secrets Manager.
- Cấu hình tên miền và HTTPS với Amazon Route 53 và AWS Certificate Manager.
- Thiết lập quy trình CI/CD tự động bằng GitHub, AWS CodeBuild, Amazon ECR và Amazon ECS.
- Giám sát hoạt động của ứng dụng thông qua Amazon CloudWatch.
- Xóa toàn bộ tài nguyên AWS sau khi hoàn thành workshop để tránh phát sinh chi phí.