---
title : "Tên miền và HTTPS"
date : 2026-01-01
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

### Mục tiêu

Cấu hình tên miền và kích hoạt HTTPS cho ứng dụng Second-Hand Marketplace.

---

## 1. Tổng quan

Trong chương này, bạn sẽ cấu hình Amazon Route 53 và AWS Certificate Manager (ACM) để cung cấp kết nối an toàn cho ứng dụng.

Amazon Route 53 được sử dụng để quản lý tên miền, trong khi AWS Certificate Manager cấp chứng chỉ SSL/TLS giúp kích hoạt kết nối HTTPS thông qua Application Load Balancer.

Sau khi hoàn thành chương này, người dùng có thể truy cập ứng dụng bằng tên miền riêng thông qua giao thức HTTPS.

---

## 2. Nội dung thực hành

Thực hiện phần sau:

- **5.8.1 Cấu hình Route 53 và AWS Certificate Manager**

---

## 3. Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ có:

- Tên miền được cấu hình trong Amazon Route 53.
- Chứng chỉ SSL/TLS được cấp bởi AWS Certificate Manager.
- HTTPS được kích hoạt thông qua Application Load Balancer.
- Ứng dụng có thể truy cập an toàn bằng tên miền riêng.