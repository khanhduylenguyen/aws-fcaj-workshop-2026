---
title : "Cấu hình AWS Secrets Manager"
date : 2026-01-01
weight : 3
chapter : false
pre : " <b> 5.5.3. </b> "
---

## Cấu hình AWS Secrets Manager

Trong phần này, bạn sẽ cấu hình AWS Secrets Manager để lưu trữ an toàn các thông tin nhạy cảm của ứng dụng Second-Hand Marketplace.

Thay vì lưu trực tiếp các thông tin này trong mã nguồn, AWS Secrets Manager giúp quản lý và bảo vệ chúng một cách tập trung và an toàn.

---

## Tạo Secret

Truy cập:

**AWS Console → AWS Secrets Manager → Secrets → Store a new secret**

Chọn **Other type of secret**.

Cấu hình Secret theo các thông tin sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Secret type | Other type of secret |
| Key | MONGODB_URI |
| Value | Chuỗi kết nối MongoDB Atlas |

Chọn **Next** để tiếp tục.

![Create Secret](/images/5-Workshop/5.5-Application-Services/create-secret.png)

---

## Cấu hình thông tin Secret

Đặt tên cho Secret.

Ví dụ:

| Thuộc tính | Giá trị |
|------------|----------|
| Secret name | production/mongodb |
| Description | MongoDB connection string |

Chọn **Next** và giữ nguyên các thiết lập mặc định.

![Secret Details](/images/5-Workshop/5.5-Application-Services/secret-details.png)

---

## Kiểm tra Secret

Truy cập:

**AWS Console → Secrets Manager → Secrets**

Xác nhận Secret vừa tạo xuất hiện trong danh sách.

Ứng dụng sẽ sử dụng Secret này khi được triển khai trên Amazon ECS.

![Secret List](/images/5-Workshop/5.5-Application-Services/secret-list.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một Secret được tạo trong AWS Secrets Manager.
- Chuỗi kết nối MongoDB Atlas được lưu trữ an toàn.
- Secret sẵn sàng để Amazon ECS sử dụng.