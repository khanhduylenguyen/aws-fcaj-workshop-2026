---
title : "Tạo VPC"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

## Tạo VPC

Trong phần này, bạn sẽ tạo một **Virtual Private Cloud (VPC)** để xây dựng môi trường mạng riêng cho ứng dụng trên AWS.

VPC là nền tảng của toàn bộ hạ tầng mạng. Tất cả các tài nguyên như Subnet, Route Table, Internet Gateway, NAT Gateway, Application Load Balancer và Amazon ECS sẽ được triển khai bên trong VPC này.

---

## Tạo Virtual Private Cloud

Truy cập:

**AWS Console → VPC → Your VPCs → Create VPC**

Chọn **VPC only**, sau đó cấu hình như sau:

| Thuộc tính | Giá trị |
|------------|----------|
| Tài nguyên cần tạo | VPC only |
| Tên | production-vpc |
| IPv4 CIDR | 10.0.0.0/16 |
| IPv6 CIDR | None |
| Tenancy | Default |

Kiểm tra lại cấu hình và chọn **Create VPC**.

![Create VPC](/images/5-Workshop/5.4-Networking/create-vpc.png)

---

## Kiểm tra VPC

Truy cập:

**AWS Console → VPC → Your VPCs**

Chọn **production-vpc** và kiểm tra các thông tin sau:

| Thuộc tính | Giá trị mong đợi |
|------------|------------------|
| Trạng thái | Available |
| IPv4 CIDR | 10.0.0.0/16 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

Xác nhận VPC đã được tạo thành công trước khi chuyển sang bước cấu hình mạng.

![VPC Details](/images/5-Workshop/5.4-Networking/vpc-details.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một Virtual Private Cloud tên **production-vpc**.
- Môi trường mạng riêng với dải địa chỉ **10.0.0.0/16**.
- DNS Resolution và DNS Hostnames được bật.
- VPC sẵn sàng để cấu hình Subnet và các tài nguyên mạng ở bước tiếp theo.