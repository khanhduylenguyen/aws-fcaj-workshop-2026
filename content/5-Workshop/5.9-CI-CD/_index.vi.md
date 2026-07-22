---
title : "CI/CD"
date : 2026-01-01
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

### Mục tiêu

Cấu hình quy trình CI/CD cho ứng dụng Second-Hand Marketplace bằng AWS CodeBuild.

---

## 1. Tổng quan

Trong chương này, bạn sẽ cấu hình AWS CodeBuild để tự động hóa quá trình build ứng dụng.

Khi mã nguồn trên GitHub được cập nhật, AWS CodeBuild sẽ build Docker Image, đẩy Image lên Amazon ECR và chuẩn bị phiên bản mới nhất để triển khai.

Quá trình này giúp giảm các thao tác triển khai thủ công và đảm bảo quy trình build được thực hiện nhất quán.

---

## 2. Nội dung thực hành

Thực hiện phần sau:

- **5.9.1 Cấu hình AWS CodeBuild**

---

## 3. Kết quả mong đợi

Sau khi hoàn thành chương này, bạn sẽ có:

- Một dự án AWS CodeBuild kết nối với GitHub.
- Docker Image được build tự động.
- Docker Image được đẩy lên Amazon ECR.
- Quy trình CI/CD có thể lặp lại cho các lần triển khai tiếp theo.