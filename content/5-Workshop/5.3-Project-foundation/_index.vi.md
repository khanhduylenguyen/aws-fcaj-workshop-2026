---
title : "Chuẩn bị dự án"
date : 2026-01-01
weight : 3
chapter : false
pre : " <b> 5.3. </b> "
---

## Chuẩn bị dự án

Trong phần này, bạn sẽ chuẩn bị mã nguồn ứng dụng trước khi triển khai lên AWS. Bao gồm tải mã nguồn, cài đặt các thư viện cần thiết, cấu hình biến môi trường và kiểm tra ứng dụng có thể chạy thành công trên môi trường cục bộ.

---

## Tải mã nguồn

Clone mã nguồn dự án từ GitHub.

```bash
git clone <repository-url>
cd <project-folder>
```

---

## Cài đặt thư viện

Cài đặt tất cả các thư viện cần thiết cho dự án.

```bash
npm install
```

Đợi quá trình cài đặt hoàn tất.

---

## Cấu hình biến môi trường

Tạo tệp `.env` trong thư mục gốc của dự án và cấu hình các biến môi trường cần thiết.

Ví dụ:

```text
PORT=3000
MONGODB_URI=<your-mongodb-uri>
SESSION_SECRET=<your-session-secret>
AWS_REGION=ap-southeast-1
AWS_S3_BUCKET=<your-s3-bucket>
AWS_ACCESS_KEY_ID=<your-access-key>
AWS_SECRET_ACCESS_KEY=<your-secret-key>
```

![Cấu hình biến môi trường](/images/5-Workshop/5.3-Project-foundation/env-file.png)

---

## Chạy ứng dụng

Khởi động ứng dụng.

```bash
npm start
```

Sau khi ứng dụng khởi động thành công, mở trình duyệt và truy cập:

```text
http://localhost:3000
```

![Chạy ứng dụng](/images/5-Workshop/5.3-Project-foundation/run-localhost.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ:

- Tải thành công mã nguồn dự án.
- Cài đặt đầy đủ các thư viện cần thiết.
- Cấu hình thành công các biến môi trường.
- Chạy thành công ứng dụng trên môi trường cục bộ.