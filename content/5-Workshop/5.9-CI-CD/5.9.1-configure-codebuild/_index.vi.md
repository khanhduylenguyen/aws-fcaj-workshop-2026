---
title : "Cấu hình AWS CodeBuild"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.9.1. </b> "
---

## Cấu hình AWS CodeBuild

Trong phần này, bạn sẽ cấu hình AWS CodeBuild để tự động hóa quá trình build ứng dụng Second-Hand Marketplace.

AWS CodeBuild lấy mã nguồn từ GitHub, thực thi các lệnh được định nghĩa trong tệp `buildspec.yml`, build Docker Image và đẩy Image lên Amazon Elastic Container Registry (Amazon ECR).

---

## Tạo CodeBuild Project

Truy cập:

**AWS Console → AWS CodeBuild → Build projects → Create build project**

Cấu hình Project theo các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Project name | wed-mbdc-build |
| Source provider | GitHub |
| Repository | GitHub Repository |

Chọn **Next** để tiếp tục.

![Create Project](/images/5-Workshop/5.9-CI-CD/create-project.png)

---

## Cấu hình Build Environment

Cấu hình môi trường build.

| Thuộc tính | Giá trị |
|------------|----------|
| Environment image | Managed image |
| Operating system | Ubuntu |
| Runtime | Standard |
| Privileged mode | Enabled |

Bật **Privileged mode** để CodeBuild có thể build Docker Image.

![Build Environment](/images/5-Workshop/5.9-CI-CD/build-environment.png)

---

## Cấu hình Build Specification

Dự án sử dụng tệp `buildspec.yml` được lưu trong GitHub Repository.

Tệp này định nghĩa các bước:

- Đăng nhập Amazon ECR.
- Build Docker Image.
- Tag Docker Image.
- Đẩy Docker Image lên Amazon ECR.

![Buildspec](/images/5-Workshop/5.9-CI-CD/buildspec.png)

---

## Chạy Build

Mở CodeBuild Project và chọn **Start build**.

Đợi đến khi quá trình build hoàn tất.

![Build History](/images/5-Workshop/5.9-CI-CD/build-history.png)

---

## Kiểm tra kết quả Build

Truy cập:

**AWS Console → AWS CodeBuild → Build history**

Xác nhận:

- Build có trạng thái **Succeeded**.
- Tất cả các giai đoạn build hoàn thành thành công.
- Docker Image đã được đẩy lên Amazon ECR.

![Build Success](/images/5-Workshop/5.9-CI-CD/build-success.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một AWS CodeBuild Project kết nối với GitHub.
- Docker Image được build thành công.
- Docker Image được đẩy lên Amazon ECR.
- Quy trình build tự động cho ứng dụng.