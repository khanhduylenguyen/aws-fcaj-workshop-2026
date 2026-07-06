---
title: "Worklog Tuần 11"
date: 2026-06-21
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Mục tiêu tuần 11:
* Xây dựng & demo dự án Capstone tích hợp toàn bộ kiến thức Cloud + AI/ML đã học.
* Phối hợp với mentor/nhóm để review & chỉnh sửa sản phẩm.
* Hoàn thiện documentation cho dự án cuối kỳ (README, API doc, Architecture diagram).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Review & chốt ý tưởng Capstone project với mentor <br> - Lên kế hoạch chi tiết: scope, timeline, kiến trúc, stack công nghệ <br> - Tạo GitHub repo cho dự án, phân công issue nếu làm nhóm <br> - Phân tích đối tượng người dùng & use case chính <br> - Xác định KPIs thành công (latency < 2s, accuracy > 85%, chi phí < $X)                                                                                                                                                | 22/06/2026   | 22/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Xây dựng backend & infrastructure (AWS): <br>&emsp; + CloudFormation triển khai VPC (2 AZ) + ECS Fargate + RDS + S3 + CloudWatch <br>&emsp; + API Gateway + Lambda cho các AI service <br>&emsp; + Frontend React build & deploy qua S3 + CloudFront <br>&emsp; + Cognito User Pool cho authentication <br>&emsp; + Setup CI/CD với GitHub Actions                                                                                                                                            | 23/06/2026   | 23/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tích hợp Bedrock (RAG + Agent) vào sản phẩm <br> - Thiết lập CI/CD với GitHub Actions & CodePipeline (build → ECR → ECS / CloudFront) <br> - Bổ sung CloudWatch Alarm + SNS cho các dịch vụ quan trọng <br> - Tạo dashboard theo dõi toàn hệ thống <br> - Viết README giới thiệu dự án & sơ đồ kiến trúc                                                                                                                                                                       | 24/06/2026   | 24/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành End-to-end:** <br>&emsp; + Test toàn bộ luồng nghiệp vụ: đăng ký → upload tài liệu → hỏi đáp → xuất báo cáo <br>&emsp; + Viết test case cho các Lambda & API (pytest, jest) <br>&emsp; + Kiểm thử trên nhiều thiết bị (mobile, tablet, desktop) <br>&emsp; + Bảo mật: kiểm tra IAM, KMS, secret rotation, Guardrails cho Bedrock                                                                                                                                          | 25/06/2026   | 26/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Demo nội bộ cho mentor & nhóm <br> - Thu thập feedback, lên kế hoạch cải thiện cho tuần cuối <br> - Viết báo cáo tiến độ Capstone trên workshop <br> - Cập nhật portfolio & CV với dự án Capstone & các chứng chỉ AWS đã đạt được                                                                                                                                                                                                                  | 26/06/2026   | 26/06/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 11:
* Chốt được đề tài Capstone & kế hoạch chi tiết:
  * Phạm vi rõ ràng, khả thi trong thời gian còn lại
  * Kiến trúc hệ thống minh hoạ đầy đủ AWS services đã học
  * Xác định rõ KPIs: latency, accuracy, chi phí

* Triển khai thành công hạ tầng & ứng dụng:
  * CloudFormation tự động triển khai môi trường (VPC, subnets, NAT, ALB, ECS, RDS, S3)
  * ECS Fargate chạy container backend, CloudFront + S3 host frontend React
  * API Gateway + Lambda kết nối Bedrock (RAG + Agent)
  * Cognito User Pool cung cấp authentication đa user
  * CloudWatch dashboard theo dõi toàn hệ thống

* Tích hợp Generative AI:
  * Bedrock Knowledge Base truy vấn tài liệu nghiệp vụ
  * Bedrock Agent xử lý tác vụ nhiều bước (gọi Lambda action)
  * Guardrails bảo vệ đầu ra (deny topics, PII redaction)
  * Embedding sử dụng Titan Embeddings v2 cho tiếng Việt + tiếng Anh

* CI/CD hoàn chỉết:
  * GitHub Actions lint/test/build mỗi PR
  * CodePipeline auto deploy lên ECS / CloudFront
  * Rollback tự động khi deploy thất bại (CloudWatch alarm trigger)

* Bảo mật & vận hành:
  * IAM Role thay vì access key cho mọi service
  * Secrets lưu trong Secrets Manager, rotate tự động
  * KMS mã hoá tất cả data at-rest
  * Guardrails cho Bedrock, WAF cho ALB

* Testing:
  * Unit test cho Lambda (pytest, coverage > 80%)
  * Integration test cho luồng API end-to-end
  * Manual test trên Chrome, Firefox, Safari, iOS Safari
  * Security test: thử XSS, SQL injection, prompt injection

* Demo nội bộ thành công:
  * Trình bày trước mentor và các bạn FCAJ
  * Nhận feedback về UX (cải thiện flow upload), security (thêm MFA), và performance (cache response)

* Đã cập nhật CV & portfolio với dự án Capstone.
* Thói quen mới:
  * Viết unit test song song với feature (TDD)
  * Mỗi PR đều có ít nhất 1 reviewer, không tự merge
  * Document mọi quyết định kiến trúc lớn trong ADR (Architecture Decision Record)
  * Runbook cho mỗi service trên production
* ...