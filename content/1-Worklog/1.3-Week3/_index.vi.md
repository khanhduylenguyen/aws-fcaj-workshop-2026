---
title: "Worklog Tuần 3"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Mục tiêu tuần 3:
* Nắm vững các dịch vụ Database trên AWS: RDS (relational) và DynamoDB (NoSQL).
* Thiết lập hệ thống monitoring & logging với Amazon CloudWatch và CloudTrail.
* Triển khai hạ tầng có tính sẵn sàng cao (High Availability) với ALB + Auto Scaling Group trên nhiều AZ.
* Hoàn thành lab 3-tier hoàn chỉnh (Web + ALB + ASG + RDS).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu Relational Database Service (RDS): <br>&emsp; + Hỗ trợ MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora <br>&emsp; + DB Instance class & Storage type <br>&emsp; + Multi-AZ Deployment (synchronous replication) <br>&emsp; + Read Replica (asynchronous replication) <br>&emsp; + Parameter Group & Option Group <br>&emsp; + Automated Backup & Point-in-Time Recovery <br>&emsp; + RDS Proxy <br> - **Thực hành:** Tạo RDS MySQL instance, kết nối từ EC2, tạo schema & table mẫu               | 27/04/2026   | 27/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Amazon DynamoDB: <br>&emsp; + NoSQL key-value & document <br>&emsp; + Partition Key & Sort Key <br>&emsp; + Secondary Index (GSI / LSI) <br>&emsp; + Capacity mode (On-demand / Provisioned) <br>&emsp; + Auto Scaling & TTL <br>&emsp; + Global Tables (multi-region) <br>&emsp; + DAX (in-memory cache) <br> - **Thực hành:** Tạo bảng DynamoDB, thao tác CRUD với AWS CLI & SDK Python (boto3)                                                                                                                                            | 28/04/2026   | 28/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Amazon CloudWatch: <br>&emsp; + Metrics & Namespaces (CPU, NetworkIn, StatusCheckFailed...) <br>&emsp; + Alarms & Actions (Auto Scaling, SNS) <br>&emsp; + Logs & Log Groups (CloudWatch Agent) <br>&emsp; + Dashboards <br>&emsp; + CloudWatch Logs Insights (query log) <br> - Tìm hiểu AWS CloudTrail để audit API call <br> - **Thực hành:** Cài CloudWatch Agent trên EC2, đẩy log lên CloudWatch Logs, tạo alarm CPU > 70% → gửi SNS notification | 29/04/2026   | 29/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu High Availability & Load Balancing: <br>&emsp; + Elastic Load Balancer: ALB (Layer 7) vs NLB (Layer 4) vs GLB (Gateway) <br>&emsp; + Target Group & Health Check <br>&emsp; + Auto Scaling Group (min/desired/max) <br>&emsp; + Launch Template vs Launch Configuration <br>&emsp; + Scaling policies: target tracking, step, simple <br> - **Thực hành:** Triển khai ASG + ALB cho ứng dụng web trên nhiều AZ, mô phỏng mất 1 instance và quan sát ASG tạo lại                                                                                                                                                | 30/04/2026   | 01/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 3:** <br>&emsp; + Web app trên EC2 (Auto Scaling) đằng sau ALB <br>&emsp; + Backend lưu dữ liệu vào RDS MySQL trong private subnet <br>&emsp; + CloudWatch Alarm khi CPU > 70% → scale out + SNS notification <br>&emsp; + Tổng kết & commit code lên GitHub <br>&emsp; + Viết report PDF cho lab kèm sơ đồ kiến trúc                                                                                                                                                                            | 01/05/2026   | 01/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 3:
* Triển khai và vận hành được Amazon RDS:
  * Tạo RDS MySQL Multi-AZ; standby replica ở AZ khác, tự động failover khi primary lỗi
  * Cấu hình Security Group chỉ cho phép EC2 truy cập port 3306
  * Tạo Read Replica và test tải đọc bằng sysbench
  * Bật Automated Backup giữ 7 ngày, cho phép Point-in-Time Recovery
  * Dùng Parameter Group để tinh chỉnh `max_connections`, `slow_query_log`

* Thành thạo DynamoDB cho bài toán NoSQL:
  * Thiết kế Partition Key / Sort Key hợp lý (ví dụ: `PK = USER#123`, `SK = ORDER#2026-04-28`)
  * CRUD bằng AWS CLI và boto3 Python SDK
  * Hiểu sự khác biệt giữa On-demand (pay per request) và Provisioned (RCU/WCU cố định)
  * Bật TTL tự động xoá session sau N ngày
  * Dùng GSI để truy vấn theo thuộc tính không phải key chính
  * Nắm được đơn vị tính giá: Read Capacity Unit (1 strongly consistent read 4KB), Write Capacity Unit (1 write 1KB)

* Cấu hình được CloudWatch:
  * Tạo custom metric cho EC2 (RAM, disk) bằng CloudWatch Agent
  * Cấu hình Alarm kích hoạt hành động (scale out / SNS email)
  * Tạo Dashboard theo dõi CPU, Network, 5xx errors
  * Bật CloudTrail ở tất cả region để audit mọi thay đổi trong tài khoản
  * Dùng Logs Insights để query: `fields @timestamp, @message | filter @message like /ERROR/`

* Xây dựng hạ tầng có tính sẵn sàng cao:
  * Auto Scaling Group tự động co giãn theo CPU (target tracking 60%)
  * Application Load Balancer phân phối traffic đa AZ với thuật toán round-robin
  * Health check mỗi 30s giúp loại bỏ instance lỗi khỏi rotation
  * Launch Template giúp tái sử dụng cấu hình instance (AMI, security group, user data)
  * Warm-up time 60s tránh đè tải lên instance vừa mới spin up

* Hoàn thành lab 3-tier (Web + ALB + ASG + RDS) và viết báo cáo trên workshop với sơ đồ kiến trúc rõ ràng.
* Đã quen với việc thiết kế kiến trúc "defense in depth":
  * Multi-AZ cho database và application
  * Security Group tách biệt cho từng tier (web, app, db)
  * CloudWatch monitor toàn bộ layer
* Thói quen mới:
  * Mọi thay đổi infra đều tag `Environment: dev/staging/prod` để theo dõi chi phí
  * Mỗi RDS đều bật encryption at-rest với KMS và deletion protection
  * Có runbook cho từng alarm CloudWatch: ai nhận, xử lý thế nào
* ...