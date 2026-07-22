---
title : "Điều kiện chuẩn bị"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

### Mục tiêu

Đảm bảo người đọc có thể truy cập AWS Management Console, chuẩn bị đầy đủ các công cụ phát triển cần thiết và tải mã nguồn dự án trước khi bắt đầu triển khai.

---

## 1. Công cụ cần chuẩn bị

Workshop này sử dụng **AWS Management Console (Web UI)** để tạo và quản lý các tài nguyên AWS. Không yêu cầu sử dụng AWS CLI hoặc các công cụ Infrastructure as Code (IaC).

Chuẩn bị các phần mềm sau:

- **Node.js (v18+)**: Dùng để chạy ứng dụng trên máy cục bộ.
- **Git**: Quản lý và tải mã nguồn dự án.
- **Docker Desktop**: Dùng để xây dựng Docker Image.
- **Visual Studio Code (hoặc IDE khác)**: Dùng để chỉnh sửa mã nguồn.

---

## 2. Các bước thực hiện

**Đăng nhập AWS Console:** Đăng nhập vào AWS Management Console và đảm bảo Region được chọn là **ap-southeast-1 (Singapore)**.

**Checkpoint:** Xác nhận Region đang sử dụng là **ap-southeast-1** trước khi tạo tài nguyên.

**Kiểm tra công cụ cục bộ:** Đảm bảo các phần mềm đã được cài đặt thành công.

```bash
node --version
npm --version
git --version
docker --version
```

**Checkpoint:** Tất cả các lệnh đều trả về phiên bản hợp lệ.

**Tải mã nguồn dự án:** Clone mã nguồn từ GitHub và mở dự án bằng Visual Studio Code.

**Checkpoint:** Dự án được mở thành công và sẵn sàng cho quá trình triển khai.

---

## 3. Kết quả mong đợi

- Đăng nhập thành công vào AWS Management Console.
- Chuẩn bị đầy đủ môi trường phát triển.
- Tải thành công mã nguồn dự án.
- Hoàn thành các điều kiện chuẩn bị trước khi chuyển sang chương tiếp theo.