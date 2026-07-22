---
title : "Cấu hình mạng"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

## Cấu hình mạng

Sau khi tạo Virtual Private Cloud (VPC), bước tiếp theo là cấu hình các thành phần mạng cần thiết cho ứng dụng.

Trong phần này, bạn sẽ tạo các Public Subnet và Private Subnet, cấu hình Internet Gateway, NAT Gateway, Route Table và Security Group. Những thành phần này giúp ứng dụng giao tiếp an toàn giữa Internet, Application Load Balancer, Amazon ECS và các dịch vụ AWS khác.

---

## Tạo Public Subnet và Private Subnet

Truy cập:

**AWS Console → VPC → Subnets → Create subnet**

Tạo bốn Subnet với cấu hình sau:

| Name | Availability Zone | IPv4 CIDR |
|------|-------------------|------------|
| public-subnet-a | ap-southeast-1a | 10.0.1.0/24 |
| public-subnet-b | ap-southeast-1b | 10.0.2.0/24 |
| private-subnet-a | ap-southeast-1a | 10.0.3.0/24 |
| private-subnet-b | ap-southeast-1b | 10.0.4.0/24 |

Đối với hai Public Subnet, bật tùy chọn **Auto-assign public IPv4 address**.

Sau khi tạo hoàn tất, kiểm tra danh sách Subnet để đảm bảo tất cả các Subnet đã được tạo thành công.

![Subnets](/images/5-Workshop/5.4-Networking/subnets.png)

---

## Cấu hình Internet Gateway

Truy cập:

**AWS Console → VPC → Internet Gateways → Create internet gateway**

Cấu hình:

| Thuộc tính | Giá trị |
|------------|----------|
| Name | production-igw |

Sau khi tạo:

- Chọn **Attach to VPC**
- Chọn **production-vpc**

Kiểm tra trạng thái Internet Gateway là **Attached**.

![Internet Gateway](/images/5-Workshop/5.4-Networking/internet-gateway.png)

---

## Cấu hình NAT Gateway

Truy cập:

**AWS Console → VPC → NAT Gateways → Create NAT gateway**

Cấu hình:

| Thuộc tính | Giá trị |
|------------|----------|
| Name | production-nat |
| Subnet | public-subnet-a |
| Connectivity type | Public |
| Elastic IP | Allocate Elastic IP |

Đợi NAT Gateway chuyển sang trạng thái **Available** trước khi tiếp tục.

![NAT Gateway](/images/5-Workshop/5.4-Networking/nat-gateway.png)

---

## Cấu hình Route Table

Truy cập:

**AWS Console → VPC → Route Tables**

Tạo hai Route Table:

| Route Table | Associated Subnets | Default Route |
|-------------|--------------------|---------------|
| public-rt | public-subnet-a, public-subnet-b | Internet Gateway |
| private-rt | private-subnet-a, private-subnet-b | NAT Gateway |

Cấu hình Route:

### Public Route Table

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | Internet Gateway |

### Private Route Table

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | NAT Gateway |

Sau khi cấu hình, liên kết (Associate) đúng Route Table với các Subnet tương ứng.

![Route Tables](/images/5-Workshop/5.4-Networking/route-tables.png)

---

## Cấu hình Security Group

Truy cập:

**AWS Console → EC2 → Security Groups**

Tạo hai Security Group.

### Application Load Balancer Security Group

| Thuộc tính | Giá trị |
|------------|----------|
| Name | production-alb-sg |
| VPC | production-vpc |

Inbound Rules

| Type | Port | Source |
|------|------|---------|
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

Outbound Rules

| Type | Destination |
|------|-------------|
| All Traffic | 0.0.0.0/0 |

---

### Amazon ECS Security Group

| Thuộc tính | Giá trị |
|------------|----------|
| Name | production-ecs-sg |
| VPC | production-vpc |

Inbound Rules

| Type | Port | Source |
|------|------|---------|
| Custom TCP | 3000 | production-alb-sg |

Outbound Rules

| Type | Destination |
|------|-------------|
| All Traffic | 0.0.0.0/0 |

Sau khi hoàn tất, xác nhận cả hai Security Group đã được tạo thành công.

![Security Groups](/images/5-Workshop/5.4-Networking/security-groups.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Hai Public Subnet và hai Private Subnet được tạo.
- Internet Gateway được gắn vào VPC.
- NAT Gateway hoạt động ở trạng thái Available.
- Public Route Table và Private Route Table được cấu hình đúng.
- Security Group cho Application Load Balancer và Amazon ECS được thiết lập đầy đủ.
- Hạ tầng mạng sẵn sàng cho việc triển khai ứng dụng ở các chương tiếp theo.