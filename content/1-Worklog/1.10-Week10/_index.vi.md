---
title: "Worklog Tuần 10"
date: 2026-06-14
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Mục tiêu tuần 10:
* Củng cố kiến thức Security & Identity trong hệ sinh thái AWS.
* Tìm hiểu chiến lược tối ưu chi phí vận hành (Cost Optimization) với AWS Budgets, Cost Explorer và Savings Plan.
* Rà soát & bảo mật workshop cá nhân đang host trên AWS, viết checklist áp dụng được.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Ôn tập AWS Shared Responsibility Model <br> - Tìm hiểu AWS Organizations & AWS Control Tower: <br>&emsp; + Multi-account architecture (dev, staging, prod) <br>&emsp; + Service Control Policy (SCP) chặn region/dịch vụ <br>&emsp; + Organizational Units (OU) theo team/dự án <br>&emsp; + AWS RAM chia sẻ resource giữa account                                                                                                                                                                       | 15/06/2026   | 15/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu AWS IAM Identity Center (SSO): <br>&emsp; + Permission Sets <br>&emsp; + Account assignment <br>&emsp; + Tích hợp với Azure AD / Okta <br> - Tìm hiểu AWS Cognito: <br>&emsp; + User Pool (authentication) <br>&emsp; + Identity Pool (authorization) <br>&emsp; + JWT token & refresh token <br>&emsp; + Hosted UI <br> - **Thực hành:** Bảo vệ API Gateway bằng Cognito Authorizer; user pool cho web app React                                                                  | 16/06/2026   | 16/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu AWS KMS & Secrets Manager: <br>&emsp; + Customer Managed Key (CMK) <br>&emsp; + AWS Managed Key <br>&emsp; + Encryption at-rest & in-transit <br>&emsp; + Key Rotation <br>&emsp; + Secrets Manager rotate secret tự động (RDS password) <br>&emsp; + Cross-account access <br> - **Thực hành:** Mã hoá EBS, RDS, S3 bằng KMS, lưu DB password trong Secrets Manager và truy xuất trong Lambda                                                                                                                                                                       | 17/06/2026   | 17/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu AWS Cost Explorer & Budgets: <br>&emsp; + Cost & Usage Report (CUR) <br>&emsp; + Reserved Instance & Savings Plan <br>&emsp; + Cost anomaly detection <br>&emsp; + Tagging policy <br>&emsp; + Spot Instance cho workload linh hoạt <br> - **Thực hành:** Lập budget cảnh báo khi chi phí vượt ngưỡng ($20/tháng), phân tích cost theo service/tag qua Cost Explorer; bật anomaly detection                                                                                            | 18/06/2026   | 19/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Rà soát workshop cá nhân:** <br>&emsp; + Xoá tài nguyên thừa (EC2, EBS, Elastic IP không dùng) <br>&emsp; + Dùng tag theo dõi chi phí (Environment, Project) <br>&emsp; + Tắt EC2 ngoài giờ làm việc (Lambda + EventBridge) <br>&emsp; + Audit IAM access key cũ, xoá key không dùng <br>&emsp; + Bật CloudTrail ở region đang dùng <br> - Viết checklist bảo mật & cost trên workshop                                                                                                                                       | 19/06/2026   | 19/06/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 10:
* Nắm vững mô hình bảo mật AWS:
  * Hiểu rõ AWS chịu trách nhiệm về "security OF the cloud" (hardware, hypervisor, region)
  * Khách hàng chịu trách nhiệm về "security IN the cloud" (OS, app, data, IAM)
  * Áp dụng vào các dịch vụ đang sử dụng

* Quản trị đa tài khoản với Organizations:
  * Tạo nhiều account (dev / staging / prod) để cô lập môi trường
  * Gán SCP chặn region không sử dụng (chỉ allow `ap-southeast-1`, `us-east-1`)
  * Tổ chức OU theo team/dự án, dễ áp dụng policy đồng loạt
  * AWS RAM chia sẻ VPC Subnet, Transit Gateway giữa các account

* Cấu hình SSO & quản lý truy cập:
  * IAM Identity Center hỗ trợ đăng nhập một lần nhiều account
  * Permission Sets gán quyền theo role (Admin, Developer, Read-only)
  * Cognito User Pool cung cấp authentication cho web/mobile app
  * Cognito Identity Pool cấp AWS credential tạm thời cho user
  * API Gateway + Cognito Authorizer bảo vệ REST API

* Bảo mật dữ liệu:
  * Mã hoá EBS, RDS, S3 bằng KMS CMK
  * S3 default encryption (SSE-S3 hoặc SSE-KMS)
  * RDS encryption at-rest + in-transit (Force SSL)
  * Secrets Manager xoay vòng password tự động (Lambda rotate function)
  * Truy xuất secret qua Lambda mà không hardcode
  * Xử lý key policy: chỉ service cần thiết được dùng key

* Tối ưu chi phí:
  * Phát hiện tài nguyên idle (EC2, EBS, Elastic IP) qua Cost Explorer
  * Đặt Budget alarm khi vượt $20/tháng
  * Bật Cost Anomaly Detection để nhận cảnh báo khi chi phí đột biến
  * Phân tích cost theo service, tag, region
  * Áp dụng Savings Plan cho workload ổn định (EC2, Lambda)
  * Dùng Spot Instance cho workload linh hoạt (dev, batch)

* Các best practice đã áp dụng:
  * Tắt EC2 không dùng bằng Lambda + EventBridge (schedule)
  * Auto delete EBS volume khi EC2 bị xoá (`DeleteOnTermination: true`)
  * Release Elastic IP không gắn vào instance
  * Xoá access key > 90 ngày không dùng
  * Bật MFA cho tất cả user có quyền quản trị

* Hoàn thành rà soát workshop cá nhân, viết checklist bảo mật & tối ưu chi phí lên GitHub.
* Thói quen mới:
  * Mỗi resource đều có tag chuẩn: Project, Environment, Owner, CostCenter
  * Mỗi tuần dành 30 phút xem Cost Explorer để theo dõi chi phí
  * Không bao giờ chia sẻ access key qua Slack/email, dùng role + OIDC
  * Mỗi lần tạo S3 đều bật Block Public Access mặc định
* ...