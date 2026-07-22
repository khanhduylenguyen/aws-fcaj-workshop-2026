---
title : "Cấu hình Route 53 và AWS Certificate Manager"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.8.1. </b> "
---

## Cấu hình Route 53 và AWS Certificate Manager

Trong phần này, bạn sẽ cấu hình Amazon Route 53 và AWS Certificate Manager (ACM) để kích hoạt HTTPS cho ứng dụng Second-Hand Marketplace.

AWS Certificate Manager được sử dụng để cấp chứng chỉ SSL/TLS, trong khi Amazon Route 53 quản lý tên miền và định tuyến yêu cầu của người dùng đến Application Load Balancer.

---

## Yêu cầu chứng chỉ SSL/TLS

Truy cập:

**AWS Console → AWS Certificate Manager → Request**

Chọn **Request a public certificate**.

Cấu hình chứng chỉ theo các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Domain name | your-domain.com |
| Validation method | DNS validation |
| Key algorithm | RSA 2048 |

Chọn **Request**.

![Request Certificate](/images/5-Workshop/5.8-Domain-and-HTTPS/request-certificate.png)

---

## Xác thực chứng chỉ

Sau khi yêu cầu chứng chỉ, ACM sẽ cung cấp bản ghi DNS để xác thực tên miền.

Nếu tên miền được quản lý bởi Amazon Route 53, chọn **Create records in Route 53**.

Đợi đến khi trạng thái chứng chỉ chuyển sang **Issued**.

![Certificate Issued](/images/5-Workshop/5.8-Domain-and-HTTPS/certificate-issued.png)

---

## Cấu hình tên miền trong Amazon Route 53

Truy cập:

**AWS Console → Route 53 → Hosted Zones**

Mở Hosted Zone của tên miền và tạo bản ghi **A – Alias**.

Cấu hình như sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Record type | A |
| Alias | Yes |
| Route traffic to | Application Load Balancer |

Lưu cấu hình.

![Hosted Zone](/images/5-Workshop/5.8-Domain-and-HTTPS/hosted-zone.png)

---

## Cấu hình HTTPS cho Application Load Balancer

Truy cập:

**AWS Console → EC2 → Load Balancers**

Mở Application Load Balancer và thêm Listener HTTPS.

Cấu hình:

| Thuộc tính | Giá trị |
|------------|----------|
| Protocol | HTTPS |
| Port | 443 |
| Certificate | ACM Certificate |
| Default action | Forward to Target Group |

Lưu cấu hình.

![HTTPS Listener](/images/5-Workshop/5.8-Domain-and-HTTPS/https-listener.png)

---

## Kiểm tra truy cập HTTPS

Mở trình duyệt và truy cập ứng dụng bằng tên miền đã cấu hình.

Xác nhận:

- Ứng dụng truy cập thành công.
- HTTPS đã được kích hoạt.
- Trình duyệt hiển thị kết nối an toàn.

![HTTPS Website](/images/5-Workshop/5.8-Domain-and-HTTPS/https-website.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Chứng chỉ SSL/TLS được cấp bởi AWS Certificate Manager.
- Tên miền được cấu hình trong Amazon Route 53.
- HTTPS được kích hoạt trên Application Load Balancer.
- Ứng dụng có thể truy cập an toàn bằng HTTPS.