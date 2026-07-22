---
title : "Cấu hình Amazon S3"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.5.2. </b> "
---

## Cấu hình Amazon S3

Trong phần này, bạn sẽ cấu hình một Amazon S3 Bucket để lưu trữ hình ảnh sản phẩm cho ứng dụng Second-Hand Marketplace.

Thay vì lưu ảnh trực tiếp trên máy chủ ứng dụng, Amazon S3 cung cấp dịch vụ lưu trữ đối tượng có khả năng mở rộng và độ bền cao, giúp ứng dụng chạy trên Amazon ECS truy cập hình ảnh một cách hiệu quả.

---

## Tạo S3 Bucket

Truy cập:

**AWS Console → Amazon S3 → Buckets → Create bucket**

Cấu hình Bucket theo các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Bucket name | *your-bucket-name* |
| AWS Region | ap-southeast-1 |
| Object Ownership | ACLs disabled |
| Block Public Access | Enabled |

Kiểm tra lại cấu hình và chọn **Create bucket**.

![Create Bucket](/images/5-Workshop/5.5-Application-Services/create-bucket.png)

---

## Tải lên hình ảnh sản phẩm

Mở Bucket và chọn **Upload**.

Tải lên một hoặc nhiều hình ảnh sản phẩm sẽ được sử dụng trong ứng dụng.

Sau khi tải lên thành công, kiểm tra các đối tượng đã xuất hiện trong Bucket.

![Upload Objects](/images/5-Workshop/5.5-Application-Services/upload-images.png)

---

## Kiểm tra nội dung Bucket

Truy cập:

**Amazon S3 → Buckets → Bucket của bạn**

Xác nhận các hình ảnh đã được lưu trong Bucket.

Các hình ảnh này sẽ được ứng dụng sử dụng khi hiển thị thông tin sản phẩm.

![Bucket Objects](/images/5-Workshop/5.5-Application-Services/bucket-object.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một Amazon S3 Bucket được tạo.
- Hình ảnh sản phẩm được tải lên thành công.
- Các đối tượng được lưu trong Amazon S3 và sẵn sàng để ứng dụng sử dụng.