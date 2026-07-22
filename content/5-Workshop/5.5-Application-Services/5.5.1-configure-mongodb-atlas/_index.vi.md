---
title : "Cấu hình MongoDB Atlas"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.5.1. </b> "
---

## Cấu hình MongoDB Atlas

Trong phần này, bạn sẽ cấu hình MongoDB Atlas để làm cơ sở dữ liệu cho ứng dụng Second-Hand Marketplace.

MongoDB Atlas là dịch vụ cơ sở dữ liệu trên nền tảng đám mây, dùng để lưu trữ dữ liệu như người dùng, sản phẩm, danh mục và đơn hàng.

---

## Tạo Database Cluster

Đăng nhập MongoDB Atlas và truy cập:

**Deployment → Database**

Tạo một Cluster mới hoặc sử dụng Cluster hiện có của dự án.

Sau khi tạo xong, kiểm tra trạng thái của Cluster phải là **Available**.

![MongoDB Cluster](/images/5-Workshop/5.5-Application-Services/mongodb-cluster.png)

---

## Tạo Database User

Truy cập:

**Security → Database Access**

Tạo một Database User với quyền phù hợp.

Ví dụ cấu hình:

| Thuộc tính | Giá trị |
|------------|----------|
| Authentication Method | Password |
| Username | admin |
| Database User Privileges | Atlas Admin |

Lưu lại tên đăng nhập và mật khẩu để sử dụng ở các bước tiếp theo.

![Database User](/images/5-Workshop/5.5-Application-Services/database-user.png)

---

## Cấu hình Network Access

Truy cập:

**Security → Network Access**

Thêm địa chỉ IP được phép kết nối đến cơ sở dữ liệu.

Trong quá trình phát triển, bạn có thể tạm thời cho phép mọi địa chỉ IP.

| Thuộc tính | Giá trị |
|------------|----------|
| Access List Entry | 0.0.0.0/0 |

Sau khi triển khai thực tế, nên thay bằng địa chỉ IP hoặc dải mạng phù hợp.

![Network Access](/images/5-Workshop/5.5-Application-Services/network-access.png)

---

## Lấy Connection String

Mở Cluster và chọn **Connect**.

Chọn **Drivers** và sao chép chuỗi kết nối MongoDB.

Ví dụ:

```text
mongodb+srv://admin:<password>@cluster0.xxxxx.mongodb.net/secondhand
```

Chuỗi kết nối này sẽ được lưu trữ an toàn bằng **AWS Secrets Manager** ở phần tiếp theo.

![Connection String](/images/5-Workshop/5.5-Application-Services/connection-string.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một MongoDB Atlas Cluster sẵn sàng sử dụng.
- Database User được cấu hình.
- Network Access được cấu hình.
- Chuỗi kết nối MongoDB sẵn sàng để ứng dụng sử dụng.