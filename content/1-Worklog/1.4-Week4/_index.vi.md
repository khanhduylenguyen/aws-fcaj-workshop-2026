---
title: "Worklog Tuần 4"
date: 2026-05-03
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Mục tiêu tuần 4:
* Làm chủ mô hình Serverless trên AWS với Lambda, API Gateway, Step Functions.
* Xây dựng REST/HTTP API không cần quản lý máy chủ.
* Kết nối serverless với các dịch vụ lưu trữ (S3, DynamoDB) và message queue (SQS, SNS, EventBridge).
* Hoàn thành ứng dụng serverless đầu tiên (API + Lambda + DynamoDB + Frontend).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu AWS Lambda: <br>&emsp; + Function handler & event payload <br>&emsp; + Runtime: Python 3.12, Node.js 20, Java 21, Go, .NET <br>&emsp; + Cold start vs warm start <br>&emsp; + Concurrency limit & reserved concurrency <br>&emsp; + Timeout & memory size (128 MB - 10240 MB) <br>&emsp; + Lambda Layer để chia sẻ thư viện <br>&emsp; + Container image support <br> - **Thực hành:** Tạo Lambda function xử lý ảnh resize từ S3 trigger, ghi ảnh mới vào bucket khác | 04/05/2026   | 04/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Amazon API Gateway: <br>&emsp; + REST API (có resource, method) vs HTTP API (nhẹ, rẻ hơn) <br>&emsp; + Stages & Deployment <br>&emsp; + Mapping template (request/response) <br>&emsp; + CORS, Throttling, Usage Plan & API Key <br>&emsp; + Custom Domain Name <br>&emsp; + Lambda Authorizer & Cognito Authorizer <br> - **Thực hành:** Xây dựng CRUD API (GET, POST, PUT, DELETE) kết nối Lambda + DynamoDB, test qua Postman | 05/05/2026   | 05/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Amazon SQS & SNS: <br>&emsp; + SQS Standard vs FIFO Queue <br>&emsp; + Visibility Timeout & Long Polling <br>&emsp; + Dead Letter Queue <br>&emsp; + SNS Topic & Subscription (email, SMS, Lambda, SQS) <br>&emsp; + Fan-out pattern <br> - Tìm hiểu Amazon EventBridge: <br>&emsp; + Event bus, rule, target <br>&emsp; + Scheduled job (cron) <br> - **Thực hành:** Tạo workflow Lambda xử lý đơn hàng qua SQS + SNS notification khi có đơn mới | 06/05/2026   | 06/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu AWS Step Functions: <br>&emsp; + State machine (Amazon States Language - ASL) <br>&emsp; + State types: Task, Choice, Parallel, Map, Wait, Pass, Fail, Succeed, Catch/Retry <br>&emsp; + Standard vs Express Workflows <br> - **Thực hành:** Thiết kế pipeline xử lý đơn hàng: nhận đơn → validate → thanh toán (mô phỏng) → lưu DB → gửi email xác nhận qua SNS; xử lý retry khi lỗi | 07/05/2026   | 08/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 4:** <br>&emsp; + REST API (API Gateway) → Lambda → DynamoDB <br>&emsp; + Frontend SPA (React) gọi API, deploy qua S3 + CloudFront <br>&emsp; + CloudWatch theo dõi logs & metrics của Lambda <br>&emsp; + Cấu hình Cognito Authorizer để bảo vệ API <br>&emsp; + Tổng kết & cập nhật workshop <br> - Viết blog chia sẻ "Serverless 101 - xây dựng REST API trên AWS" | 08/05/2026   | 08/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 4:
* Thành thạo AWS Lambda:
  * Tạo function phản ứng event từ S3 (ObjectCreated), API Gateway (proxy), EventBridge (scheduled)
  * Tối ưu package bằng cách loại bỏ thư viện không dùng → cold start giảm ~40%
  * Dùng Lambda Layers để chia sẻ thư viện chung (ví dụ: boto3, pandas) giữa nhiều function
  * Cấu hình reserved concurrency để giới hạn tối đa cho từng function, bảo vệ downstream services
  * Biết khi nào nên dùng Container Image (Lambda Image) thay vì ZIP package (ví dụ: 500 MB, custom ML model)

* Xây dựng REST API hoàn chỉnh:
  * Triển khai CRUD thông qua API Gateway (REST API) + Lambda
  * Cấu hình CORS để frontend ở domain khác gọi được
  * Throttling (100 RPS), usage plan cho gói Free vs Pro
  * Kết nối API Gateway với DynamoDB thông qua IAM Role của Lambda
  * Biết khi nào chọn HTTP API (rẻ hơn 70%, đủ cho use case đơn giản) vs REST API (nhiều tính năng hơn)
  * Cấu hình Custom Domain Name với ACM certificate

* Hiểu & sử dụng messaging service:
  * Phân biệt SQS (queue, pull) và SNS (pub/sub, push)
  * SQS FIFO Queue đảm bảo thứ tự xử lý cho bài toán cần strict ordering
  * Cấu hình Dead Letter Queue cho Lambda consumer khi message xử lý thất bại sau N lần
  * Sử dụng EventBridge để lập lịch job (cron `0 2 * * ? *` chạy 2h sáng mỗi ngày)
  * Pattern fan-out: SNS topic → nhiều SQS subscriber để xử lý đồng thời

* Thiết kế workflow phức tạp với Step Functions:
  * State machine mô tả toàn bộ pipeline nghiệp vụ dưới dạng JSON ASL
  * Parallel State xử lý song song nhiều task (ví dụ: gửi email + cập nhật inventory)
  * Map State xử lý từng item trong array
  * Retry & Catch xử lý lỗi tự động, tránh vòng lặp retry vô hạn
  * Trực quan hoá execution trên console, debug dễ dàng với execution history

* Hoàn thành ứng dụng serverless đầu tiên:
  * React frontend lưu trên S3, phân phối qua CloudFront
  * Backend là REST API (API Gateway + Lambda) gọi DynamoDB
  * Cognito User Pool cung cấp JWT token cho frontend, Authorizer xác thực mỗi request
  * CloudWatch dashboard theo dõi số lượng invocations, errors, throttles, duration

* Đã viết bài blog "Serverless 101" chia sẻ lại trên workshop.
* Thói quen mới:
  * Mỗi Lambda function đều có retry policy rõ ràng (maxRetryAttempts: 3, maxEventAgeInSeconds: 3600)
  * Biết dùng X-Ray để trace request giữa API Gateway → Lambda → DynamoDB
  * Đặt tên function theo convention: `service-stage-function`, ví dụ `order-prod-createOrder`
* ...