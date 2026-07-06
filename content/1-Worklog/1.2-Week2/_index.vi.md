---
title: "Worklog Tuần 2"
date: 2026-04-19
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Mục tiêu tuần 2:
* Làm chủ dịch vụ lưu trữ Amazon S3: bucket, object, storage class, versioning, static website hosting.
* Nắm vững AWS IAM với các khái niệm User, Group, Role, Policy và cách áp dụng nguyên tắc least privilege.
* Hiểu mô hình mạng VPC cơ bản: CIDR, Subnet, Route Table, Internet Gateway, NAT Gateway.
* Triển khai bài lab tổng hợp EC2 + S3 + IAM và đẩy một static website lên S3.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu Amazon S3: <br>&emsp; + Khái niệm Bucket (global unique name) & Object (key + value + metadata) <br>&emsp; + Storage class: S3 Standard, IA, One Zone-IA, Glacier, Glacier Deep Archive <br>&emsp; + Versioning <br>&emsp; + Lifecycle rule <br>&emsp; + Cross-Region Replication <br>&emsp; + Object Lock & WORM <br> - **Thực hành:** Tạo bucket đầu tiên, upload & download file qua Console, upload qua AWS CLI (`aws s3 cp / sync`) <br> - Cấu hình Static Website Hosting cho bucket           | 20/04/2026   | 20/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu AWS IAM: <br>&emsp; + Khái niệm User, Group, Role, Policy <br>&emsp; + AWS Managed Policy vs Customer Managed Policy vs Inline Policy <br>&emsp; + Cấu trúc JSON Policy (Effect, Action, Resource, Condition) <br>&emsp; + Principle of least privilege <br>&emsp; + IAM Access Analyzer <br> - **Thực hành:** Tạo IAM user mới cho developer & admin riêng, gán policy, bật MFA, tạo access key an toàn cho CI/CD                                                                                                                                   | 21/04/2026   | 21/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Amazon VPC cơ bản: <br>&emsp; + VPC & CIDR block (10.0.0.0/16) <br>&emsp; + Subnet Public/Private <br>&emsp; + Route Table & Route propagation <br>&emsp; + Internet Gateway (IGW) <br>&emsp; + NAT Gateway & NAT Instance <br>&emsp; + DHCP Option Set <br> - **Thực hành:** Phân tích địa chỉ IP cho VPC với 2 AZ, lên kế hoạch dải CIDR cho từng subnet                                                                                                                                                   | 22/04/2026   | 22/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu Security Group & NACL: <br>&emsp; + Stateful vs Stateless <br>&emsp; + Rule evaluation order <br> - **Thực hành:** Tạo VPC tuỳ chỉnh với 2 subnet (public/private) ở 2 AZ, khởi tạo EC2 trong private subnet, cấu hình Bastion Host ở public subnet, NAT Gateway để private subnet ra internet <br> - Test SSH từ Bastion → EC2 private                                                                                                                                                                              | 23/04/2026   | 24/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 2:** <br>&emsp; + Tạo S3 bucket lưu trữ ảnh cho web tĩnh <br>&emsp; + Cấu hình IAM Role cho EC2 truy cập S3 (không dùng Access Key cứng) <br>&emsp; + Upload ảnh từ EC2 lên S3 bằng AWS CLI trong private subnet <br>&emsp; + Cấu hình S3 static website hosting, public read <br>&emsp; + Bật CloudFront trước S3 để cache & HTTPS <br> - Cập nhật tiến độ worklog & workshop lên GitHub                                                                                                                                  | 24/04/2026   | 24/04/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 2:
* Nắm vững dịch vụ Amazon S3 và các khái niệm:
  * **Bucket & Object:** tên bucket là global unique, key có thể chứa `/` để mô phỏng thư mục
  * **Storage class:** lựa chọn theo tần suất truy cập & chi phí (Standard cho hot, IA cho warm, Glacier cho archive)
  * **Versioning:** bật để giữ nhiều phiên bản object, dễ rollback khi xoá nhầm
  * **Lifecycle rule:** tự động chuyển object sang class rẻ hơn hoặc xoá sau N ngày
  * **Static website hosting:** host HTML/CSS/JS tĩnh, tích hợp CloudFront để cache toàn cầu
  * **Object Lock / WORM:** lưu trữ bất biến cho dữ liệu tuân thủ (compliance)
  * **Event Notifications:** kết nối S3 → SQS / SNS / Lambda để xử lý sự kiện (upload, delete)

* Tạo và quản lý IAM hiệu quả:
  * Phân biệt được IAM User (người), Group (nhóm), Role (vai trò cho service)
  * Hiểu cấu trúc JSON Policy: `Effect`, `Action`, `Resource`, `Condition`
  * Viết được policy theo nguyên tắc least privilege (chỉ cấp đúng quyền cần thiết)
  * Bật MFA cho user có quyền quản trị và tạo IAM Access Key riêng cho CI/CD
  * Sử dụng IAM Role thay vì Access Key hardcode khi EC2 cần truy cập S3
  * Biết dùng IAM Access Analyzer để phát hiện policy quá rộng

* Hiểu và triển khai được mô hình VPC cơ bản:
  * **CIDR block & Subnet planning:** chia VPC /16 thành các subnet /24 để có ~256 IP mỗi subnet
  * **Phân biệt Public subnet** (route 0.0.0.0/0 → IGW) và **Private subnet** (route qua NAT Gateway)
  * **Route Table & Route propagation:** liên kết subnet với route table tương ứng
  * **Internet Gateway:** cho phép resource trong public subnet ra internet hai chiều
  * **NAT Gateway:** cho phép resource trong private subnet đi ra internet một chiều (download)
  * **Bastion Host:** EC2 ở public subnet dùng để SSH "jump" vào EC2 trong private subnet

* Phân biệt được:
  * **Security Group** (stateful, áp dụng ở ENI, rule chỉ cần in là có out tự động)
  * **NACL** (stateless, áp dụng ở subnet, rule có cả in & out, xử lý theo rule order)

* Hoàn thành lab tổng hợp EC2 + S3 + IAM, deploy một static website đơn giản:
  * Tạo 2 bucket: một lưu ảnh, một host HTML
  * EC2 trong private subnet vẫn upload ảnh lên S3 nhờ IAM Role
  * Public có thể truy cập static website qua endpoint S3 hoặc CloudFront
  * Bật Block Public Access cho bucket ảnh, chỉ cho phép EC2 Role đọc

* Cập nhật workshop cá nhân trên GitHub thường xuyên; commit có ý nghĩa, có README cho từng tuần.
* Thói quen mới:
  * Mỗi policy IAM đều kèm comment giải thích lý do cấp quyền
  * Không bao giờ nhập Access Key vào source code, dùng biến môi trường hoặc Secrets Manager (sẽ học tuần sau)
  * Thiết kế VPC trước khi launch instance để tránh phải rebuild
* ...