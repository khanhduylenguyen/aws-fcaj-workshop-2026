---
title : "Build Docker Image"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.6.1. </b> "
---

## Build Docker Image

Trong phần này, bạn sẽ tạo Dockerfile và build Docker Image cho ứng dụng Second-Hand Marketplace.

Docker giúp đóng gói ứng dụng cùng các thư viện cần thiết vào một container, đảm bảo môi trường chạy nhất quán giữa môi trường phát triển và triển khai.

---

## Tạo Dockerfile

Mở thư mục dự án và tạo tệp **Dockerfile** tại thư mục gốc của dự án.

Dockerfile được sử dụng trong dự án như sau.

```dockerfile
FROM node:20-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

Lưu Dockerfile sau khi hoàn tất cấu hình.

![Dockerfile](/images/5-Workshop/5.6-Containerization/dockerfile.png)

---

## Build Docker Image

Sau khi tạo Dockerfile, mở Terminal tại thư mục gốc của dự án và thực hiện build Docker Image.

Chạy lệnh sau:

```bash
docker build -t secondhand-marketplace .
```

Trong quá trình build, Docker sẽ thực hiện các bước sau:

1. Tải Node.js base image nếu chưa có trên máy.
2. Tạo thư mục làm việc bên trong container.
3. Sao chép toàn bộ mã nguồn của dự án vào container.
4. Cài đặt các thư viện của ứng dụng bằng **npm install**.
5. Đóng gói toàn bộ ứng dụng thành một Docker Image.

Sau khi build thành công, Docker sẽ hiển thị thông báo tương tự:

```text
Successfully built <IMAGE_ID>
Successfully tagged secondhand-marketplace:latest
```

Để kiểm tra Docker Image vừa tạo, chạy lệnh:

```bash
docker images
```

Lệnh này sẽ hiển thị danh sách Docker Image trên máy. Xác nhận Docker Image vừa build xuất hiện với tag **latest**.

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Dockerfile được tạo thành công.
- Docker Image được build thành công.
- Docker Image sẵn sàng để đẩy lên Amazon ECR.