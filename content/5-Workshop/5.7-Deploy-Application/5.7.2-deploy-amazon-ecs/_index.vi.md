---
title : "Triển khai ứng dụng lên Amazon ECS"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.7.2. </b> "
---

## Triển khai ứng dụng lên Amazon ECS

Trong phần này, bạn sẽ triển khai ứng dụng Second-Hand Marketplace lên Amazon ECS sử dụng AWS Fargate.

Quá trình triển khai bao gồm tạo Task Definition, cấu hình ECS Service và liên kết Service với Application Load Balancer đã tạo ở bước trước.

---

## Tạo Task Definition

Truy cập:

**AWS Console → Amazon ECS → Task definitions → Create new task definition**

Cấu hình Task Definition theo các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Launch type | AWS Fargate |
| Task definition family | production-task |
| Operating system | Linux |
| CPU | 1 vCPU |
| Memory | 2 GB |

Chọn **Next** để cấu hình Container.

![Task Definition](/images/5-Workshop/5.7-Deploy-Application/task-definition.png)

---

## Cấu hình Container

Cấu hình Container sử dụng Docker Image được lưu trong Amazon ECR.

| Thuộc tính | Giá trị |
|------------|----------|
| Container name | wed-mbdc |
| Image URI | Amazon ECR Image |
| Container port | 3000 |

Cấu hình các biến môi trường và Secrets cần thiết, sau đó tạo Task Definition.

![Container Configuration](/images/5-Workshop/5.7-Deploy-Application/container-configuration.png)

---

## Tạo ECS Service

Truy cập:

**Amazon ECS → Clusters → production-cluster → Create**

Cấu hình Service.

| Thuộc tính | Giá trị |
|------------|----------|
| Launch type | AWS Fargate |
| Task definition | production-task |
| Service name | production-service |
| Desired tasks | 1 |

Tiếp tục đến phần cấu hình mạng.

![Create Service](/images/5-Workshop/5.7-Deploy-Application/create-service.png)

---

## Cấu hình Networking

Cấu hình mạng cho ECS Service.

| Thuộc tính | Giá trị |
|------------|----------|
| VPC | production-vpc |
| Subnets | Private Subnets |
| Security Group | production-ecs-sg |
| Public IP | Disabled |

Trong phần **Load Balancer**:

- Chọn **Use existing load balancer**.
- Chọn Application Load Balancer đã tạo ở bước trước.
- Chọn Target Group tương ứng.

Kiểm tra lại cấu hình và chọn **Create**.

![Networking Configuration](/images/5-Workshop/5.7-Deploy-Application/networking.png)

---

## Kiểm tra triển khai

Truy cập:

**Amazon ECS → Clusters → production-cluster → Services**

Xác nhận:

- Service có trạng thái **Active**.
- Số lượng Running Tasks bằng Desired Tasks.
- Task có trạng thái **Running**.

![Service Running](/images/5-Workshop/5.7-Deploy-Application/service-running.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một Task Definition được tạo.
- ECS Service được triển khai thành công.
- Ứng dụng chạy trên AWS Fargate.
- Amazon ECS tích hợp với Application Load Balancer.