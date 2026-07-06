---
title: "Worklog Tuần 6"
date: 2026-05-17
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Mục tiêu tuần 6:
* Nắm vững Container với Docker: image, container, Dockerfile, Docker Compose.
* Làm chủ Amazon ECR, ECS (Fargate & EC2) và tìm hiểu Kubernetes cơ bản với EKS.
* Thực hành triển khai microservice trên ECS Fargate đằng sau ALB với Auto Scaling và Blue/Green deployment.
* Viết blog chia sẻ kinh nghiệm container trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu Docker cơ bản: <br>&emsp; + Khái niệm Image vs Container <br>&emsp; + Dockerfile & câu lệnh (FROM, RUN, COPY, CMD, ENTRYPOINT, EXPOSE) <br>&emsp; + Multi-stage build <br>&emsp; + Layer & caching <br>&emsp; + Docker Compose (multi-container) <br>&emsp; + Volume, Network, Bind Mount <br> - **Thực hành:** Docker hoá ứng dụng Node.js (Express) và Python (FastAPI), đẩy lên Docker Hub | 18/05/2026   | 18/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Amazon ECR: <br>&emsp; + Push & pull image qua Docker CLI <br>&emsp; + Lifecycle policy (xoá image cũ) <br>&emsp; + Image scanning (CVE) <br>&emsp; + Cross-region replication <br> - Tìm hiểu Amazon ECS: <br>&emsp; + Cluster, Service, Task Definition <br>&emsp; + Capacity provider: Fargate vs EC2 <br>&emsp; + Service Auto Scaling <br>&emsp; + Task IAM Role <br>&emsp; + Container Insights (CloudWatch) | 19/05/2026   | 19/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Thực hành:** Triển khai microservice trên ECS Fargate: <br>&emsp; + Tạo Task Definition cho web Node.js (CPU 256, Memory 512) <br>&emsp; + Tạo Service kết nối ALB <br>&emsp; + Cấu hình Auto Scaling theo CPU <br>&emsp; + Cập nhật image thông qua CodeDeploy (Blue/Green) <br>&emsp; + Enable Container Insights để theo dõi                                                                                                                                       | 20/05/2026   | 20/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu Kubernetes cơ bản: <br>&emsp; + Kiến trúc: Control Plane, Worker Node, kubelet <br>&emsp; + Pod, Deployment, Service, ReplicaSet <br>&emsp; + Namespace, ConfigMap, Secret <br>&emsp; + kubectl cơ bản: get, describe, logs, exec <br>&emsp; + YAML manifest <br> - Tìm hiểu Amazon EKS: <br>&emsp; + Managed Node Group <br>&emsp; + Fargate profile <br>&emsp; + EKS Add-ons (VPC CNI, kube-proxy, ...)                                                                                                                                                  | 21/05/2026   | 22/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Lab tổng hợp tuần 6:** <br>&emsp; + Docker hoá ứng dụng web (frontend React) + database (Postgres) <br>&emsp; + Push image lên ECR <br>&emsp; + Triển khai web trên ECS Fargate với ALB <br>&emsp; + Database chạy trên RDS trong private subnet <br>&emsp; + Cập nhật workshop & viết bài blog "Container & ECS Fargate 101"                                                                                                                                  | 22/05/2026   | 22/05/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 6:
* Thành thạo Docker:
  * Viết Dockerfile multi-stage build giảm 70% dung lượng image cuối cùng
  * Sử dụng `docker-compose.yml` cho môi trường development (web + db + redis)
  * Hiểu khái niệm layer, cache, image registry
  * Debug container với `docker logs`, `docker exec -it <container> sh`
  * Đẩy image lên Docker Hub và ECR

* Làm chủ Amazon ECS:
  * Thiết kế Task Definition chuẩn: container definitions, CPU, memory, env, port mapping
  * Triển khai Service với Fargate (serverless container, không cần quản lý EC2)
  * Cấu hình ALB + Auto Scaling cho Service (target tracking CPU 70%)
  * Blue/Green deployment qua CodeDeploy: 50% traffic sang bản mới, sau đó chuyển hết
  * Container Insights trên CloudWatch giám sát CPU, memory, network của từng container

* Tìm hiểu Kubernetes & EKS:
  * Nắm cấu trúc Pod, Deployment, Service, Namespace
  * Tạo manifest YAML để deploy ứng dụng (`kubectl apply -f deployment.yaml`)
  * Tạo EKS Cluster với Managed Node Group và Fargate Profile
  * Dùng eksctl và AWS Console để quản lý cluster
  * Helm chart để đóng gói manifest (tìm hiểu sơ)

* Hoàn thành lab tổng hợp container + ECS Fargate + ALB:
  * Docker Compose ở local để dev
  * Push image lên ECR từ GitHub Actions (kết hợp tuần 5)
  * Triển khai lên ECS Fargate thông qua CodePipeline
  * RDS chạy trong private subnet, chỉ ECS Service có thể truy cập qua Security Group

* So sánh & lựa chọn được giữa các deployment option:
  * **ECS Fargate:** đơn giản, tích hợp chặt với AWS, không cần quản cluster
  * **ECS EC2:** rẻ hơn khi workload ổn định, nhưng phải tự quản EC2
  * **EKS:** cần Kubernetes ecosystem phong phú, multi-cloud, nhưng phức tạp hơn

* Viết blog chia sẻ trải nghiệm học Docker + ECS trên workshop.
* Thói quen mới:
  * Luôn `.dockerignore` để tránh copy `node_modules`, `.git`, `.env` vào image
  * Scan image bằng `trivy` hoặc ECR scan trước khi deploy
  * Tách rõ base image dùng chung để tiết kiệm dung lượng layer
  * Đặt tag image theo commit SHA, không dùng `latest` trong production
* ...