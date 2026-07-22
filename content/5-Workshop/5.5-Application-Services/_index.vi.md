---
title : "Dịch vụ ứng dụng"
date : 2026-01-01
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

### Mục tiêu

Cấu hình các dịch vụ cốt lõi cần thiết cho ứng dụng Second-Hand Marketplace.

---

## 1. Tổng quan

Trong chương này, bạn sẽ cấu hình các dịch vụ chính hỗ trợ ứng dụng chạy trên Amazon ECS.

Ứng dụng sử dụng MongoDB Atlas làm cơ sở dữ liệu, Amazon S3 để lưu trữ hình ảnh sản phẩm và AWS Secrets Manager để quản lý an toàn các thông tin nhạy cảm như chuỗi kết nối cơ sở dữ liệu và các khóa bí mật của ứng dụng.

Sau khi hoàn thành chương này, ứng dụng sẽ sẵn sàng kết nối đến các dịch vụ bên ngoài một cách an toàn và ổn định.

---

## 2. Nội dung thực hành

Thực hiện lần lượt các phần sau:

- **5.5.1 Cấu hình MongoDB Atlas**
- **5.5.2 Cấu hình Amazon S3**
- **5.5.3 Cấu hình AWS Secrets Manager**

---

## 3. Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ có:

- MongoDB Atlas được cấu hình để lưu trữ dữ liệu ứng dụng.
- Amazon S3 được cấu hình để lưu trữ hình ảnh sản phẩm.
- AWS Secrets Manager được cấu hình để quản lý thông tin nhạy cảm một cách an toàn.
- Các dịch vụ ứng dụng sẵn sàng cho việc triển khai trên Amazon ECS.