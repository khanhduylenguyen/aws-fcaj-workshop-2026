---
title : "Hạ tầng mạng"
date : 2026-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Mục tiêu

Xây dựng hạ tầng mạng cần thiết để triển khai ứng dụng Second-Hand Marketplace một cách an toàn trên AWS.

---

## 1. Tổng quan

Hạ tầng mạng là nền tảng của toàn bộ hệ thống trên đám mây. Trong chương này, bạn sẽ tạo Virtual Private Cloud (VPC) và cấu hình các thành phần mạng cần thiết để triển khai ứng dụng.

Kiến trúc mạng bao gồm Public Subnet, Private Subnet, Internet Gateway, NAT Gateway, Route Table và Security Group. Các thành phần này giúp Application Load Balancer, Amazon ECS, các dịch vụ AWS và dịch vụ bên ngoài như MongoDB Atlas có thể giao tiếp an toàn với nhau.

---

---

## 3. Nội dung thực hành

Thực hiện lần lượt các phần sau:

- **5.4.1 Tạo VPC**
- **5.4.2 Cấu hình mạng**

---

## 4. Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ có:

- Một Virtual Private Cloud (VPC) được tạo thành công.
- Public Subnet và Private Subnet được cấu hình.
- Internet Gateway và NAT Gateway được thiết lập.
- Route Table được cấu hình chính xác.
- Security Group cho Application Load Balancer và Amazon ECS được thiết lập.
- Hạ tầng mạng sẵn sàng cho việc triển khai ứng dụng.