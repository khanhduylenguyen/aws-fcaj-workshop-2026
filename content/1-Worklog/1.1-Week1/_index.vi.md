---
title: "Worklog Tuần 1"
date: 2026-04-12
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Mục tiêu tuần 1:
* Làm quen với đội ngũ First Cloud AI Journey 2026 và nắm rõ nội quy, quy trình làm việc tại đơn vị thực tập.
* Hiểu tổng quan AWS Cloud: lịch sử phát triển, mô hình triển khai (IaaS/PaaS/SaaS) và các nhóm dịch vụ cốt lõi.
* Thiết lập môi trường làm việc cục bộ với AWS Console và AWS CLI trên máy tính cá nhân.
* Bắt đầu hành trình Cloud + AI với tài khoản AWS Free Tier và triển khai EC2 instance đầu tiên.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| CN   | - Tham dự lễ khai giảng chương trình FCAJ 2026 <br> - Lắng nghe chia sẻ từ mentor và các thành viên <br> - Nhận tài khoản nội bộ và các kênh trao đổi (Slack/Discord) <br> - Giới thiệu bản thân ngắn gọn trong nhóm                                                                                                                                                                                                                  | 12/04/2026   | 12/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 2   | - Làm quen với các thành viên FCAJ <br> - Đọc và lưu ý các nội quy, quy định tại đơn vị thực tập <br> - Tìm hiểu GitHub và cách quản lý workshop cá nhân bằng Hugo <br> - Cài đặt Visual Studio Code, Git, các extension hữu ích                                                                                                                                                                                                          | 13/04/2026   | 13/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu AWS Cloud là gì, lịch sử và định vị trên thị trường Cloud <br> - Phân loại nhóm dịch vụ AWS: <br>&emsp; + **Compute:** EC2, Lambda, ECS, EKS <br>&emsp; + **Storage:** S3, EBS, EFS, Glacier <br>&emsp; + **Networking:** VPC, CloudFront, Route 53 <br>&emsp; + **Database:** RDS, DynamoDB, Aurora <br>&emsp; + **Security:** IAM, KMS, WAF, Shield <br>&emsp; + **AI/ML:** SageMaker, Bedrock, Rekognition, Comprehend                                              | 14/04/2026   | 14/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tạo AWS Free Tier account (yêu cầu thẻ quốc tế) <br> - Tìm hiểu AWS Console: cách tìm & truy cập dịch vụ theo vùng (Region) <br> - Tìm hiểu AWS CLI: <br>&emsp; + Cài AWS CLI v2 trên Windows/macOS/Linux <br>&emsp; + Cấu hình Access Key, Secret Key, Region mặc định <br> - **Thực hành:** <br>&emsp; + Tạo AWS account <br>&emsp; + Cài AWS CLI & cấu hình `aws configure` <br>&emsp; + Cách sử dụng AWS CLI: `aws sts get-caller-identity`, `aws ec2 describe-regions` | 15/04/2026   | 15/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu dịch vụ EC2 cơ bản: <br>&emsp; + Instance types (t3.micro, t3.small, m5.large,...) <br>&emsp; + AMI (Amazon Machine Image) <br>&emsp; + EBS (Elastic Block Store) & Volume types <br>&emsp; + Security Group vs NACL <br>&emsp; + Key Pair <br> - Các cách remote SSH vào EC2 (SSH client, EC2 Instance Connect) <br> - Tìm hiểu Elastic IP & khác biệt với Public IP                                                                                  | 16/04/2026   | 17/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Thực hành tổng hợp tuần 1:** <br>&emsp; + Tạo EC2 instance (Amazon Linux 2023, t3.micro) <br>&emsp; + Kết nối SSH từ máy local <br>&emsp; + Cài đặt Nginx/Apache và mở port 80/443 trong Security Group <br>&emsp; + Tạo thêm EBS volume và attach vào instance <br>&emsp; + Gắn Elastic IP, restart & kiểm tra persistent IP <br> - Cập nhật worklog tuần 1 & workshop lên GitHub                                                                                                                                  | 17/04/2026   | 17/04/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 1:
* Đã tham gia lễ khai giảng, làm quen với mentor và các thành viên trong chương trình FCAJ 2026; có thể trao đổi qua các kênh nội bộ.
* Nắm được bức tranh tổng thể về AWS Cloud:
  * Lịch sử phát triển từ năm 2006 và các mốc quan trọng
  * Mô hình chia sẻ trách nhiệm (Shared Responsibility Model)
  * Phân biệt IaaS / PaaS / SaaS và dịch vụ tương ứng trong AWS
  * Phạm vi Free Tier (12 tháng, luôn miễn phí, dùng thử)

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cốt lõi:
  * **Compute:** EC2, Lambda, ECS, EKS
  * **Storage:** S3, EBS, EFS, Glacier
  * **Networking:** VPC, CloudFront, Route 53
  * **Database:** RDS, DynamoDB, Aurora
  * **Security:** IAM, KMS, WAF, Shield
  * **AI/ML:** SageMaker, Bedrock, Rekognition, Comprehend

* Đã tạo và cấu hình AWS Free Tier account thành công; bật MFA cho root account & IAM user.
* Làm quen với AWS Management Console:
  * Biết chuyển đổi Region, tìm dịch vụ bằng thanh search
  * Đọc các thông số CPU, RAM, Network trên dashboard EC2
  * Sử dụng AWS Budgets để cảnh báo chi phí vượt ngưỡng

* Cài đặt và cấu hình AWS CLI trên máy tính bao gồm:
  * Access Key & Secret Key (không commit lên Git)
  * Region mặc định (ví dụ: ap-southeast-1 - Singapore)
  * Output format mặc định (json)
  * Sử dụng named profile để chuyển đổi nhanh giữa các tài khoản

* Sử dụng AWS CLI để thực hiện các thao tác cơ bản như:
  * `aws sts get-caller-identity` - kiểm tra thông tin tài khoản & cấu hình
  * `aws ec2 describe-regions` - lấy danh sách region
  * `aws ec2 describe-instances` - xem dịch vụ EC2
  * `aws ec2 create-key-pair` / `delete-key-pair` - tạo và quản lý key pair
  * `aws ec2 describe-volumes` - kiểm tra thông tin EBS volume đang gắn
  * `aws ec2 associate-address` - gán Elastic IP

* Khởi tạo và cấu hình EC2 instance đầu tiên:
  * Chọn Amazon Linux 2023 AMI (tối ưu cho EC2)
  * Chọn instance type t3.micro (Free Tier eligible)
  * Cấu hình Security Group mở port 22 (SSH) & 80 (HTTP)
  * Gắn EBS volume mặc định 8 GB gp3

* Kết nối SSH thành gống:
  * Dùng terminal với lệnh `ssh -i key.pem ec2-user@<public-ip>`
  * Dùng EC2 Instance Connect từ trình duyệt (không cần key)

* Gắn thêm EBS volume thứ hai vào instance:
  * Tạo EBS 20 GB gp3 ở cùng AZ với instance
  * Attach volume vào `/dev/sdf`
  * Mount & format bằng `mkfs.xfs` và `mount`

* Nắm được khái niệm Elastic IP và cách gán địa chỉ IP tĩnh cho instance; hiểu sự khác biệt với Public IP (đổi khi stop/start).
* Có khả năng kết nối giữa giao diện web (Console) và CLI để quản lý tài nguyên AWS song song.
* Biết cách tag tài nguyên (Name, Environment, Owner, Project) để dễ theo dõi & phân bổ chi phí.
* Đã commit worklog tuần 1 lên GitHub, biết cách tạo Pull Request & review code với teammate.
* Thói quen mới hình thành:
  * Mỗi ngày dành 30 phút đọc tài liệu AWS Skill Builder & AWS Whitepaper
  * Viết lại kiến thức vừa học vào notebook để nhớ lâu hơn
  * Đặt câu hỏi trong group FCAJ khi gặp khó khăn
* ...