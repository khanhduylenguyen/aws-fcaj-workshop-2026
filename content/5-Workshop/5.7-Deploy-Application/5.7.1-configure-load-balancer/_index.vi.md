---
title : "Cấu hình Load Balancer"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.7.1. </b> "
---

## Cấu hình Load Balancer

Trong phần này, bạn sẽ cấu hình Application Load Balancer (ALB) cho ứng dụng Second-Hand Marketplace.

Application Load Balancer tiếp nhận các yêu cầu HTTP từ người dùng và chuyển tiếp đến Amazon ECS thông qua Target Group.

---

## Tạo Target Group

Truy cập:

**AWS Console → EC2 → Target Groups → Create target group**

Cấu hình Target Group theo các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Target type | IP addresses |
| Protocol | HTTP |
| Port | 3000 |
| VPC | production-vpc |
| Target group name | production-target-group |

Chọn **Next**, giữ nguyên cấu hình Health Check mặc định và tạo Target Group.

![Create Target Group](/images/5-Workshop/5.7-Deploy-Application/create-target-group.png)

---

## Tạo Application Load Balancer

Truy cập:

**AWS Console → EC2 → Load Balancers → Create Load Balancer**

Chọn **Application Load Balancer** và cấu hình các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Load Balancer name | production-alb |
| Scheme | Internet-facing |
| IP address type | IPv4 |
| VPC | production-vpc |
| Availability Zones | Public Subnets |

Chọn Security Group dành cho Application Load Balancer và tiếp tục.

![Create Load Balancer](/images/5-Workshop/5.7-Deploy-Application/create-load-balancer.png)

---

## Cấu hình Listener

Cấu hình Listener mặc định cho Application Load Balancer.

| Thuộc tính | Giá trị |
|------------|----------|
| Protocol | HTTP |
| Port | 80 |
| Default action | Forward to production-target-group |

Kiểm tra lại cấu hình và chọn **Create Load Balancer**.

![Configure Listener](/images/5-Workshop/5.7-Deploy-Application/configure-listener.png)

---

## Kiểm tra Load Balancer

Truy cập:

**AWS Console → EC2 → Load Balancers**

Mở Application Load Balancer và xác nhận:

- Load Balancer có trạng thái **Active**.
- Listener đã được cấu hình thành công.
- Target Group đã được liên kết với Load Balancer.

![Load Balancer Details](/images/5-Workshop/5.7-Deploy-Application/load-balancer-details.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một Target Group được tạo.
- Một Application Load Balancer được cấu hình.
- Listener chuyển tiếp lưu lượng đến Target Group.
- Load Balancer sẵn sàng tích hợp với Amazon ECS.