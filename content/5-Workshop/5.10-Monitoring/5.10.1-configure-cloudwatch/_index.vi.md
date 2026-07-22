---
title : "Cấu hình Amazon CloudWatch"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.10.1. </b> "
---

# Cấu hình Amazon CloudWatch

Trong phần này, bạn sẽ cấu hình Amazon CloudWatch để giám sát ứng dụng đang chạy trên Amazon ECS.

Amazon CloudWatch là dịch vụ giám sát của AWS, cho phép thu thập các chỉ số (Metrics) từ các tài nguyên AWS và đánh giá chúng theo các ngưỡng đã được cấu hình. Điều này giúp quản trị viên theo dõi tình trạng hoạt động của ứng dụng, phát hiện sớm các dấu hiệu bất thường và hỗ trợ xử lý sự cố.

Trong dự án này, Amazon CloudWatch được sử dụng để giám sát mức sử dụng CPU của Amazon ECS Service thông qua CloudWatch Alarm.

---

## Cấu hình CloudWatch Logging

Amazon ECS hỗ trợ tích hợp với Amazon CloudWatch thông qua **awslogs log driver**. Trong Task Definition, cấu hình này xác định Log Group, AWS Region và Stream Prefix để container có thể gửi log trong quá trình chạy lên CloudWatch.

Trong dự án, cấu hình CloudWatch Logging được khai báo như sau:

```json
"logConfiguration": {
  "logDriver": "awslogs",
  "options": {
    "awslogs-group": "/ecs/wed-mbdc-task",
    "awslogs-create-group": "true",
    "awslogs-region": "ap-southeast-1",
    "awslogs-stream-prefix": "ecs"
  }
}
```

![CloudWatch Logging Configuration](/images/5-Workshop/5.10-Monitoring/ecs-log-configuration.png)
---

## Tạo CloudWatch Alarm

Để theo dõi trạng thái hoạt động của ứng dụng sau khi triển khai, một CloudWatch Alarm được cấu hình cho Amazon ECS Service.

Alarm sẽ theo dõi chỉ số **CPU Utilization** của ECS Service và đánh giá giá trị trung bình theo chu kỳ 5 phút. Nếu mức sử dụng CPU vượt quá ngưỡng đã thiết lập, CloudWatch sẽ tự động chuyển trạng thái từ **OK** sang **ALARM**, giúp quản trị viên nhanh chóng phát hiện tình trạng sử dụng tài nguyên bất thường và có phương án xử lý phù hợp.

CloudWatch Alarm được cấu hình với các thông số sau:

| Thuộc tính | Giá trị |
|------------|----------|
| Alarm name | production-service-cpu-alarm |
| Namespace | AWS/ECS |
| Metric | CPUUtilization |
| Threshold | 80% |
| Chu kỳ đánh giá | 5 phút |
| ECS Cluster | production-cluster |
| ECS Service | production-service |

![CloudWatch Alarm List](/images/5-Workshop/5.10-Monitoring/cpu-alarm-list.png)

---

## Kiểm tra trạng thái Alarm

Sau khi CloudWatch Alarm được tạo, Amazon CloudWatch sẽ liên tục theo dõi chỉ số CPU Utilization của Amazon ECS Service.

Trong điều kiện hoạt động bình thường, khi mức sử dụng CPU thấp hơn ngưỡng đã cấu hình, Alarm sẽ ở trạng thái **OK**. Nếu CPU Utilization vượt quá **80%** trong khoảng thời gian đánh giá, Alarm sẽ tự động chuyển sang trạng thái **ALARM** để cảnh báo quản trị viên.

Trang chi tiết của Alarm cũng cung cấp nhiều thông tin quan trọng phục vụ việc giám sát, bao gồm:

- Trạng thái hiện tại của Alarm.
- Biểu đồ CPU Utilization theo thời gian.
- Ngưỡng cảnh báo đã cấu hình.
- Chu kỳ đánh giá (Evaluation Period).
- Tên Amazon ECS Cluster.
- Tên Amazon ECS Service.
- Metric và Namespace đang được theo dõi.

Các thông tin này giúp quản trị viên đánh giá nhanh tình trạng hoạt động của hệ thống, phát hiện sớm các vấn đề về hiệu năng và có phương án xử lý trước khi ảnh hưởng đến người dùng.

![CloudWatch Alarm Details](/images/5-Workshop/5.10-Monitoring/cpu-alarm-details.png)

---

## Kết quả mong đợi

Sau khi hoàn thành phần này, bạn sẽ:

- Cấu hình Amazon ECS sử dụng **awslogs log driver**.
- Tạo CloudWatch Alarm để giám sát mức sử dụng CPU của Amazon ECS Service.
- Theo dõi trạng thái hoạt động của ứng dụng thông qua Amazon CloudWatch.
- Xây dựng cơ chế giám sát cơ bản giúp phát hiện sớm tình trạng sử dụng tài nguyên bất thường và hỗ trợ vận hành hệ thống ổn định.