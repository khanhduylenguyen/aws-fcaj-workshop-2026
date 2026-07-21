---
title: "Workshop"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng Hệ thống Y tế Từ xa Medi Path Ease

#### Tổng quan

**Medi Path Ease** là một ứng dụng y tế từ xa (Telemedicine) được xây dựng với mục tiêu kết nối bác sĩ và bệnh nhân một cách dễ dàng, an toàn và hiệu quả. Hệ thống cung cấp các tính năng chính:

* **Quản lý hồ sơ bệnh nhân** - Lưu trữ và truy xuất thông tin y tế một cách bảo mật
* **Tư vấn từ xa (Telemedicine)** - Kết nối bệnh nhân với bác sĩ qua video call và chat
* **Lưu trữ tài liệu y tế** - Upload và xem kết quả xét nghiệm, đơn thuốc, hồ sơ bệnh án
* **Trợ lý ảo AI** - Hỗ trợ trả lời câu hỏi thường gặp về sức khỏe
* **Thanh toán trực tuyến** - Tích hợp cổng thanh toán PayOS

#### Các dịch vụ AWS & Cloud sử dụng

| Dịch vụ | Mục đích sử dụng |
|---------|-------------------|
| **Amazon RDS PostgreSQL** | Cơ sở dữ liệu quan hệ chính cho ứng dụng |
| **Amazon S3** | Lưu trữ tài liệu y tế (lab results, prescriptions, EHR) |
| **Amazon CloudFront** | CDN phân phối nội dung tĩnh toàn cầu |
| **Amazon Cognito** | Xác thực và quản lý người dùng |
| **Amazon Bedrock** | Trợ lý ảo AI chatbot |
| **DigitalOcean App Platform** | Máy chủ ứng dụng Node.js/Express |
| **PayOS** | Cổng thanh toán VNPay/QrCode |

#### Kết quả đạt được sau workshop

Sau khi hoàn thành, bạn sẽ có một hệ thống telemedicine hoàn chỉnh với:
* Backend Express.js chạy trên DigitalOcean
* Cơ sở dữ liệu PostgreSQL trên AWS RDS
* Hệ thống lưu trữ file an toàn với S3 + CloudFront
* Xác thực người dùng với Cognito + JWT
* Chatbot AI hỗ trợ bệnh nhân
* Thanh toán trực tuyến qua PayOS

#### Nội dung workshop

1. [Sơ đồ kiến trúc & Tổng quan](5.1-Architecture/)
2. [Cấu hình Cơ sở dữ liệu RDS PostgreSQL](5.2-RDS-PostgreSQL/)
3. [Dịch vụ Lưu trữ S3 & CloudFront](5.3-S3-CloudFront/)
4. [Xác thực người dùng với Amazon Cognito](5.4-Cognito/)
5. [Trợ lý ảo AI Chatbot với Amazon Bedrock](5.5-Bedrock-AI/)
6. [Cấu hình DigitalOcean & Tích hợp PayOS](5.6-DigitalOcean-PayOS/)
7. [Demo Giao diện Ứng dụng Thực tế](5.7-Demo/)
8. [Dọn dẹp tài nguyên](5.8-Cleanup/)
