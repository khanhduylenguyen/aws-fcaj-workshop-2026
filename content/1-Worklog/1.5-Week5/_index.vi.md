---
title: "Worklog Tuần 5"
date: 2026-05-10
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Mục tiêu tuần 5:
* Làm quen với Infrastructure as Code (IaC) thông qua AWS CloudFormation và AWS CDK.
* Thiết lập quy trình CI/CD với GitHub Actions & AWS CodePipeline/CodeBuild.
* Tự động hoá triển khai toàn bộ hạ tầng & ứng dụng từ Git đến Production.
* Hiểu được các best practice về version hoá, rollback và tách môi trường dev/staging/prod.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu Infrastructure as Code (IaC): <br>&emsp; + Khái niệm & lợi ích (tái sử dụng, version hoá, audit) <br>&emsp; + Declarative vs Imperative <br>&emsp; + So sánh CloudFormation, Terraform, Pulumi <br> - Tìm hiểu AWS CloudFormation: <br>&emsp; + Template (YAML/JSON) <br>&emsp; + Stack & Nested Stack <br>&emsp; + Change Set & Drift Detection <br>&emsp; + Outputs, Parameters, Mappings <br>&emsp; + Custom Resource <br>&emsp; + StackSets (multi-account, multi-region) | 11/05/2026   | 11/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Thực hành:** Viết CloudFormation template triển khai VPC + 2 Subnet + EC2 + Security Group + IAM Role <br> - Dùng Change Set để preview thay đổi trước khi apply <br> - Tìm hiểu AWS CDK (Cloud Development Kit): <br>&emsp; + Viết IaC bằng Python/TypeScript/Java <br>&emsp; + Cấu trúc: App → Stack → Construct <br>&emsp; + Cú pháp `cdk init / synth / deploy / diff / destroy`                                                                                                                                  | 12/05/2026   | 12/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu GitHub Actions: <br>&emsp; + Workflow, Job, Step, Action <br>&emsp; + Trigger: push, pull_request, schedule, workflow_dispatch <br>&emsp; + Secrets & Environment (production) <br>&emsp; + Self-hosted runner <br>&emsp; + Matrix build <br> - Tìm hiểu AWS CodePipeline & CodeBuild: <br>&emsp; + Stage & Action <br>&emsp; + Source (CodeCommit/S3/Bitbucket) <br>&emsp; + Build artifact (S3) <br>&emsp; + Deploy (CloudFormation, ECS, Lambda)                                                                  | 13/05/2026   | 13/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Thực hành:** Xây dựng CI/CD pipeline hoàn chỉnh: <br>&emsp; + GitHub Actions chạy test & build ứng dụng Node.js/React <br>&emsp; + Push Docker image lên Amazon ECR <br>&emsp; + Deploy container lên ECS Fargate hoặc EC2 thông qua task definition update <br>&emsp; + Cấu hình secret an toàn qua GitHub Secrets và AWS Secrets Manager <br>&emsp; + Tự động chạy `cdk deploy` hoặc `sam deploy` khi merge vào main                                                                                                       | 14/05/2026   | 15/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 5:** <br>&emsp; + CloudFormation triển khai toàn bộ stack (VPC + ALB + ASG + RDS) <br>&emsp; + GitHub Actions tự động chạy lint/test khi push PR <br>&emsp; + CodePipeline auto deploy khi có tag release mới <br>&emsp; + Cấu hình rollback tự động khi deploy thất bại (CloudWatch alarm → CodeDeploy rollback) <br>&emsp; + Cập nhật worklog & workshop, viết bài blog "IaC & CI/CD on AWS" | 15/05/2026   | 15/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 5:
* Nắm vững tư duy Infrastructure as Code:
  * Quản lý hạ tầng bằng file (YAML/JSON) thay vì click chuột
  * Khả năng version hoá & review cấu hình hạ tầng trên Git
  * Reproducible environment ở nhiều region/account
  * Cả team dùng chung code hạ tầng, dễ onboard thành viên mới

* Thành thạo AWS CloudFormation:
  * Viết template triển khai VPC + 2 Subnet + EC2 + SG + IAM Role
  * Sử dụng Parameter & Mapping để tuỳ biến theo môi trường (dev/prod)
  * Quản lý Stack, Change Set preview trước khi apply
  * Drift Detection phát hiện resource bị thay đổi ngoài CloudFormation
  * StackSets triển khai cùng một stack sang nhiều account/region

* Trải nghiệm AWS CDK:
  * Khởi tạo project CDK bằng `cdk init app --language python`
  * Sử dụng L2 Construct (ví dụ: `aws_ec2.Vpc`) thay vì viết raw CFN
  * `cdk synth` sinh template CloudFormation
  * `cdk diff` so sánh sự khác biệt giữa code và hạ tầng hiện tại
  * `cdk deploy` tự động apply Change Set

* Xây dựng CI/CD pipeline hoàn chỉết:
  * GitHub Actions workflow:
    * Trigger: `on: push: branches: [main]` và `pull_request`
    * Job `test`: chạy ESLint, unit test với Jest, coverage report
    * Job `build`: docker build & push lên ECR với tag theo commit SHA
    * Job `deploy`: cập nhật ECS task definition, trigger deploy
  * CodePipeline: kết nối GitHub → CodeBuild (test + build) → CodeDeploy (deploy ECS)
  * Tự động đẩy Docker image lên Amazon ECR
  * Auto deploy lên ECS khi có tag release mới (`v*.*.*`)
  * Cấu hình secret an toàn qua GitHub Secrets và AWS Secrets Manager

* Tích hợp CloudFormation + CodePipeline:
  * Pipeline tự chạy `cloudformation deploy` khi có thay đổi template
  * Theo dõi trạng thái stack trên giao diện Pipeline
  * Rollback tự động khi deploy thất bại: CloudWatch alarm → CodeDeploy rollback

* Hiểu được các best practice:
  * Tách Stack: Network Stack (VPC) → Application Stack (ALB, ASG) → Database Stack (RDS)
  * Quản lý parameter qua SSM Parameter Store hoặc Secrets Manager
  * Không commit credentials vào Git, dùng OIDC role để GitHub Actions truy cập AWS không cần access key

* Hoàn thành lab tổng hợp và commit đầy đủ source IaC + workflow lên GitHub.
* Đã viết bài blog chia sẻ về IaC & CI/CD trên AWS.
* Thói quen mới:
  * Mọi thay đổi infra đều qua Pull Request, có review bởi ít nhất 1 người khác
  * Đặt tên stack theo convention: `service-environment-region`, ví dụ `workshop-prod-ap-southeast-1`
  * Dùng Git tag semantic versioning (v1.2.3) để trigger deploy
* ...