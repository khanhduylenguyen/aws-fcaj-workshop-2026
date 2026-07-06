---
title: "Worklog Tuần 7"
date: 2026-05-24
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Mục tiêu tuần 7:
* Làm quen với hệ sinh thái AI/ML trên AWS: AI Services (Rekognition, Comprehend, Polly, Translate) và ML Services (SageMaker).
* Nắm vững quy trình huấn luyện và triển khai mô hình ML trên cloud.
* Xây dựng bài toán AI đơn giản end-to-end (NLP + Vision + Speech) và viết blog chia sẻ.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tổng quan AI/ML trên AWS: <br>&emsp; + AI services vs ML services <br>&emsp; + Bedrock, SageMaker, Rekognition, Comprehend, Polly, Translate, Lex <br>&emsp; + Use case thực tế cho từng dịch vụ <br> - Tìm hiểu Amazon Rekognition: <br>&emsp; + Label Detection & Image Properties <br>&emsp; + Facial Analysis (age, emotion, gender) <br>&emsp; + Text in Image (OCR) <br>&emsp; + Moderation labels <br>&emsp; + Custom Labels (huấn luyện model riêng)            | 25/05/2026   | 25/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Amazon Comprehend (NLP): <br>&emsp; + Sentiment Analysis <br>&emsp; + Entity Recognition (PERSON, ORG, LOCATION) <br>&emsp; + Key phrase extraction <br>&emsp; + Topic Modeling <br>&emsp; + Custom Comprehend (custom classifier) <br> - **Thực hành:** Phân tích sentiment cho tập review tiếng Anh, trích xuất entity & key phrase                                                                                                                                                                  | 26/05/2026   | 26/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Amazon Polly & Translate: <br>&emsp; + Polly: Text-to-Speech, SSML, giọng theo ngôn ngữ <br>&emsp; + Translate: Language translation, custom terminology <br> - Tìm hiểu Amazon Lex: <br>&emsp; + Chatbot voice & text <br>&emsp; + Intents, Slots, Utterances <br> - **Thực hành:** Xây dựng chatbot đa ngôn ngữ sử dụng Lambda + Translate + Polly; phát audio qua S3 + CloudFront                                                                                                                       | 27/05/2026   | 27/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu Amazon SageMaker: <br>&emsp; + Notebook Instance <br>&emsp; + Built-in Algorithm (XGBoost, Linear Learner, BlazingText, ...) <br>&emsp; + Training Job với custom Docker <br>&emsp; + Endpoint (Real-time, Serverless) <br>&emsp; + SageMaker Studio (IDE) <br>&emsp; + SageMaker MLOps (Pipelines, Model Registry) <br> - **Thực hành:** Train model dự đoán giá nhà với XGBoost trong SageMaker, deploy thành endpoint, gọi predict từ Lambda                                | 28/05/2026   | 29/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 7:** <br>&emsp; + Ứng dụng web phân tích ảnh (Rekognition) + text (Comprehend) + audio (Polly) <br>&emsp; + API serverless (Lambda + API Gateway) gọi các AI service <br>&emsp; + Frontend React upload ảnh & text, hiển thị kết quả + audio <br>&emsp; + Tổng kết kiến thức AI/ML và cập nhật workshop <br> - Viết blog "AI Services trên AWS - tổng quan cho người mới"                                                                                                                          | 29/05/2026   | 29/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 7:
* Có cái nhìn tổng quan về hệ sinh thái AI/ML trên AWS:
  * Phân biệt rõ AI Services (managed, không cần train) và ML Services (SageMaker, train model riêng)
  * Lựa chọn dịch vụ phù hợp với bài toán thực tế:
    * Cần OCR ảnh → Rekognition
    * Cần phân tích sentiment → Comprehend
    * Cần đọc văn bản thành giọng nói → Polly
    * Cần dịch đa ngôn ngữ → Translate
    * Cần train model riêng với data custom → SageMaker

* Làm chủ các AI Services:
  * **Rekognition:** phát hiện vật thể (label), khuôn mặt (FaceDetail), text trong ảnh (DetectText)
  * **Comprehend:** Sentiment (+/-/Neutral), Entity (PERSON, DATE, MONEY), Key phrase
  * **Polly:** Text-to-Speech với nhiều giọng (Joanna, Matthew cho EN; Giang, Linh cho VI)
  * **Translate:** dịch giữa hơn 70 ngôn ngữ, hỗ trợ custom terminology
  * **Lex:** xây dựng chatbot cho customer service

* Triển khai chatbot đa ngôn ngữ serverless:
  * Frontend web nhập câu hỏi tiếng Việt/Anh
  * Lambda xử lý → Translate → Polly → trả về audio
  * Lưu audio vào S3, phát qua CloudFront
  * Caching translation và audio để giảm chi phí

* Làm quen với SageMaker:
  * Tạo Notebook Instance để khám phá dữ liệu (EDA)
  * Dùng Built-in Algorithm XGBoost train trên tập Boston Housing
  * Deploy model thành Endpoint, gọi inference từ Lambda
  * So sánh Real-time Endpoint (always on) và Serverless Inference (cold start)
  * SageMaker Studio IDE cung cấp môi trường Jupyter đầy đủ tính năng

* Hoàn thành lab tích hợp AI services:
  * Frontend React upload ảnh + nhập đoạn text
  * API Gateway + Lambda gọi Rekognition, Comprehend, Polly
  * Kết quả trả về dạng JSON + audio link
  * CI/CD triển khai lên S3 + CloudFront

* Viết blog chia sẻ về AI/ML trên AWS giúp cộng đồng hiểu rõ hơn.
* Thói quen mới:
  * Luôn đọc kỹ pricing model trước khi dùng AI service (tính theo request/image/character)
  * Caching kết quả AI trong DynamoDB hoặc ElastiCache để tiết kiệm chi phí
  * Đánh giá model bias & safety trước khi đưa vào production
  * Khi dùng SageMaker, tắt notebook khi không dùng để tránh phát sinh chi phí
* ...