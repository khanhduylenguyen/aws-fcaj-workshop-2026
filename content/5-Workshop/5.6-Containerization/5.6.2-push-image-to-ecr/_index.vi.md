---
title : "Đẩy Image lên Amazon ECR"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.6.2. </b> "
---

## Đẩy Image lên Amazon ECR

Trong phần này, bạn sẽ đẩy Docker Image lên Amazon Elastic Container Registry (Amazon ECR).

Amazon ECR là dịch vụ lưu trữ Docker Image được quản lý bởi AWS. Amazon ECS sẽ sử dụng Image này để triển khai ứng dụng ở các bước tiếp theo.

---

## Tạo Amazon ECR Repository

Truy cập:

**AWS Console → Amazon ECR → Private repositories → Create repository**

Cấu hình Repository theo các thông số sau.

| Thuộc tính | Giá trị |
|------------|----------|
| Visibility settings | Private |
| Repository name | secondhand-marketplace |

Chọn **Create repository**.

![Create Repository](/images/5-Workshop/5.6-Containerization/create-repository.png)

---

## Đăng nhập Amazon ECR

Mở Terminal và đăng nhập Docker vào Amazon ECR.

```bash
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com
```

Sau khi đăng nhập thành công, Docker sẽ hiển thị:

```text
Login Succeeded
```

---

## Tag Docker Image

Gắn thẻ Docker Image bằng địa chỉ Repository của Amazon ECR.

```bash
docker tag secondhand-marketplace:latest <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/secondhand-marketplace:latest
```

---

## Đẩy Docker Image lên Amazon ECR

Thực hiện lệnh sau để tải Docker Image lên Repository.

```bash
docker push <account-id>.dkr.ecr.ap-southeast-1.amazonaws.com/secondhand-marketplace:latest
```

Docker sẽ lần lượt tải các lớp (layers) của Image lên Amazon ECR. Thời gian thực hiện phụ thuộc vào kích thước Image và tốc độ mạng.

---

## Kiểm tra Repository

Truy cập:

**AWS Console → Amazon ECR → Private repositories**

Mở Repository và xác nhận Docker Image đã được tải lên thành công.

![Repository Images](/images/5-Workshop/5.6-Containerization/repository-images.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ có:

- Một Amazon ECR Repository được tạo.
- Docker đăng nhập thành công vào Amazon ECR.
- Docker Image được tải lên Amazon ECR.
- Docker Image sẵn sàng để triển khai trên Amazon ECS.