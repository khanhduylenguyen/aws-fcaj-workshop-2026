---
title: "Worklog Tuần 12"
date: 2026-06-28
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Mục tiêu tuần 12:
* Hoàn thiện Capstone dựa trên feedback tuần 11.
* Tổ chức buổi demo cuối kỳ trước mentor & cộng đồng FCAJ.
* Tổng kết toàn bộ quá trình thực tập 12 tuần, hoàn thành báo cáo và cập nhật portfolio.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Khắc phục các feedback từ tuần 11: <br>&emsp; + Cải thiện UX (upload flow, loading state, error message) <br>&emsp; + Bật MFA cho Cognito User Pool <br>&emsp; + Cache response Bedrock với ElastiCache (Redis) <br> - Tối ưu hiệu năng: <br>&emsp; + Provisioned Concurrency cho Lambda hot path <br>&emsp; + CloudFront caching cho static assets & API response <br>&emsp; + Tối ưu query Athena (partition + Parquet)                                                       | 29/06/2026   | 29/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Hoàn thiện workshop cá nhân trên GitHub: <br>&emsp; + Bổ sung hình minh hoạ (diagram kiến trúc, screenshot console) cho mỗi tuần <br>&emsp; + Sửa broken link, format markdown chuẩn <br>&emsp; + Kiểm tra Hugo build thành công, deploy lên GitHub Pages <br>&emsp; + Thêm phần "Tài liệu tham khảo" ở cuối mỗi tuần                                                                                                                                                | 30/06/2026   | 30/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tổ chức buổi demo cuối kỳ: <br>&emsp; + Đặt lịch với mentor & cộng đồng FCAJ <br>&emsp; + Chuẩn bị slide thuyết trình (problem, solution, architecture, demo, results, lesson learned) <br>&emsp; + Kịch bản demo 5-10 phút, có backup video dự phòng <br>&emsp; + Câu hỏi dự kiến & câu trả lời                                                                                                                                              | 01/07/2026   | 02/07/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tổng hợp số liệu & viết báo cáo thực tập cuối kỳ: <br>&emsp; + Tổng kết kiến thức đã học qua 12 tuần (theo bảng kỹ năng) <br>&emsp; + Đánh giá ưu/nhược điểm bản thân (điểm mạnh, điểm cần cải thiện) <br>&emsp; + Định hướng phát triển tiếp theo (chứng chỉ AWS, lộ trình nghề nghiệp) <br>&emsp; + Lời cảm ơn mentor, đồng đội                                                                                                                              | 02/07/2026   | 03/07/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Gửi báo cáo & nhận phản hồi từ mentor <br> - Tham gia buổi tổng kết toàn chương trình FCAJ <br> - Chia sẻ kinh nghiệm cho các bạn khoá sau <br> - Cập nhật CV & portfolio với dự án Capstone & các chứng chỉ AWS đã đạt được <br> - Lưu trữ source code, tài liệu vào GitHub archive cho tham khảo sau này                                                                                                                                                                                              | 03/07/2026   | 03/07/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 12:
* Hoàn thiện Capstone project:
  * Áp dụng feedback để cải thiện UX (multi-step upload, skeleton loading, toast thông báo)
  * Bật MFA cho Cognito User Pool, tăng cường bảo mật
  * Cache response Bedrock với ElastiCache (giảm 40% chi phí token)
  * Provisioned Concurrency cho Lambda ở hot path, cold start giảm từ 3s xuống 200ms
  * Tối ưu query Athena với partition + Parquet, chi phí query giảm 80%

* Hoàn thiện workshop cá nhân trên GitHub:
  * Hugo build thành công, deploy lên GitHub Pages
  * Tất cả 12 tuần worklog có hình minh hoạ (diagram kiến trúc, screenshot console)
  * Hai ngôn ngữ VI/EN đồng bộ
  * Mỗi tuần có phần "Tài liệu tham khảo" với link AWS chính thức
  * README tổng quan giúp người mới dễ theo dõi hành trình

* Demo cuối kỳ thành công:
  * Thuyết trình trước mentor và cộng đồng FCAJ (30+ người tham dự)
  * Sản phẩm hoạt động ổn định, không có bug nghiêm trọng
  * Có video backup dự phòng trong trường hợp mất mạng
  * Trả lời thành công các câu hỏi của audience về kiến trúc, chi phí, security
  * Nhận đánh giá tốt về tính khả thi & khả năng ứng dụng thực tế

* Hoàn thành báo cáo thực tập:
  * **Tổng kết kiến thức 12 tuần:**
    * Tuần 1-3: Cloud fundamentals (EC2, S3, IAM, VPC, RDS, CloudWatch)
    * Tuần 4-6: Serverless & Container (Lambda, API Gateway, ECS, CI/CD)
    * Tuần 7-9: AI/ML (Rekognition, Comprehend, SageMaker, Bedrock)
    * Tuần 10: Security & Cost Optimization
    * Tuần 11-12: Capstone & Demo
  * **Tự đánh giá:**
    * Điểm mạnh: tự học tốt, làm chủ được hệ sinh thái AWS, biết cách debug vấn đề
    * Điểm cần cải thiện: prompt engineering chuyên sâu, MLOps, tiếng Anh chuyên ngành
  * **Định hướng tiếp theo:**
    * Thi chứng chỉ AWS Solutions Architect Associate
    * Học sâu về MLOps & LLM fine-tuning
    * Đóng góp cho cộng đồng AWS Việt Nam (viết blog, mentor khoá sau)
  * **Lời cảm ơn:** mentor, teammates, FCAJ program team, AWS Vietnam

* Tham gia buổi tổng kết chương trình & chia sẻ kinh nghiệm cho khoá sau.
* Cập nhật CV & portfolio, sẵn sàng cho cơ hội nghề nghiệp tiếp theo.

* **Bài học kinh nghiệm 12 tuần:**
  1. **Học qua làm:** làm lab thực tế nhớ lâu hơn đọc lý thuyết
  2. **Đọc tài liệu gốc từ AWS:** cập nhật & chính xác nhất
  3. **Hỏi khi bí:** thầy cô & cộng đồng FCAJ luôn sẵn sàng hỗ trợ
  4. **Tự tạo project:** workshop cá nhân giúp củng cố kiến thức
  5. **Quản lý chi phí:** luôn đặt budget, audit tài nguyên hàng tuần
  6. **Bảo mật là mặc định:** không phải tính năng thêm vào sau
  7. **IaC & CI/CD từ đầu:** tránh nợ kỹ thuật về sau
  8. **AI là công cụ, không thay thế tư duy:** cần hiểu use case trước khi dùng

* Thói quen mang theo sau kỳ thực tập:
  * Mỗi dự án mới đều bắt đầu bằng sơ đồ kiến trúc & ước lượng chi phí
  * Mọi kiến thức mới đều viết blog chia sẻ để củng cố
  * Cập nhật CV mỗi quý với thành tích mới
  * Đặt mục tiêu học 1 chứng chỉ AWS mỗi năm
  * Tham gia cộng đồng AWS User Group tại TP.HCM
* ...