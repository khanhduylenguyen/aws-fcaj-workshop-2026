---
title: "Bản đề xuất"
date: 2026-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Nền tảng Mua bán Đồ cũ
## Nền tảng Mua bán Đồ cũ Cloud-Native trên AWS

## 1. Tóm tắt điều hành

Nền tảng Mua bán Đồ cũ là một ứng dụng web dựa trên nền tảng đám mây cho phép người dùng mua và bán các sản phẩm đã qua sử dụng thông qua một sàn giao dịch trực tuyến tập trung. Hệ thống cung cấp xác thực người dùng, quản lý sản phẩm, tải lên hình ảnh, tìm kiếm và quản lý danh mục, đồng thời sử dụng các dịch vụ đám mây AWS để đảm bảo khả năng mở rộng, tính sẵn sàng và đơn giản hóa việc triển khai.

Ứng dụng được phát triển bằng **Node.js**, **Express.js**, **MongoDB Atlas** và **EJS**, trong khi hạ tầng được triển khai trên **Amazon ECS Fargate** phía sau **Application Load Balancer**. Các Docker container được lưu trữ trong **Amazon ECR**, hình ảnh sản phẩm được lưu trữ trong **Amazon S3**, và **AWS CodeBuild** tự động xây dựng và triển khai ứng dụng mỗi khi mã nguồn mới được đẩy lên GitHub. Kiến trúc này cung cấp khả năng triển khai tự động, lưu trữ tập trung và hạ tầng đám mây tối ưu chi phí, phù hợp với các ứng dụng thương mại điện tử quy mô nhỏ và vừa.

---

# 2. Tuyên bố vấn đề

### Vấn đề hiện tại

Nhiều nền tảng mua bán đồ cũ dựa vào các nền tảng mạng xã hội hoặc các trang web được quản lý thủ công, khiến việc quản lý sản phẩm trở nên kém hiệu quả và khó bảo trì. Hình ảnh thường được lưu trữ cục bộ, việc triển khai yêu cầu cập nhật thủ công và việc mở rộng hệ thống trở nên khó khăn khi số lượng người dùng tăng lên. Phương pháp triển khai truyền thống cũng làm tăng thời gian gián đoạn và khối lượng công việc vận hành mỗi khi phát hành tính năng mới hoặc sửa lỗi.

### Giải pháp

Giải pháp được đề xuất xây dựng một nền tảng mua bán đồ cũ cloud-native sử dụng các dịch vụ được quản lý của AWS.

Người dùng có thể đăng ký tài khoản, đăng nhập an toàn, tải lên sản phẩm kèm hình ảnh, duyệt sản phẩm theo danh mục, tìm kiếm sản phẩm và quản lý các tin đăng của mình thông qua một ứng dụng web. Thông tin sản phẩm được lưu trữ trong MongoDB Atlas, trong khi hình ảnh sản phẩm được tải lên sẽ được lưu trữ trong Amazon S3.

Ứng dụng được đóng gói bằng Docker và triển khai trên Amazon ECS Fargate. AWS CodeBuild tự động phát hiện các lần commit mới từ GitHub, xây dựng Docker image, đẩy image lên Amazon ECR và kích hoạt Amazon ECS triển khai phiên bản mới nhất mà không cần can thiệp thủ công.

### Lợi ích và hoàn vốn đầu tư

Nền tảng giúp giảm đáng kể thời gian triển khai thông qua quy trình CI/CD tự động, đồng thời cải thiện khả năng mở rộng và độ tin cậy của hệ thống. Các lập trình viên chỉ cần đẩy mã nguồn lên GitHub và toàn bộ quy trình triển khai sẽ được thực hiện tự động. Kiến trúc đám mây cũng giảm thiểu công việc quản lý máy chủ và tạo nền tảng vững chắc cho việc mở rộng trong tương lai. Trong quá trình phát triển, AWS Free Tier và AWS Academy Credits giúp giảm chi phí vận hành.

---

# 3. Kiến trúc giải pháp

Ứng dụng tuân theo kiến trúc container cloud-native sử dụng các dịch vụ được quản lý của AWS.

## Kiến trúc giải pháp

![System Architecture](/images/2-Proposal/system_architecture.jpg)

### Các dịch vụ AWS được sử dụng

- **Amazon ECS Fargate** – Chạy các Docker container mà không cần quản lý máy chủ.
- **Amazon ECR** – Lưu trữ các Docker image được sử dụng bởi ECS.
- **AWS CodeBuild** – Tự động xây dựng và triển khai các phiên bản ứng dụng mới từ GitHub.
- **Application Load Balancer (ALB)** – Phân phối các yêu cầu HTTP đến các ECS Task.
- **Amazon S3** – Lưu trữ hình ảnh sản phẩm được tải lên.
- **MongoDB Atlas** – Lưu trữ dữ liệu của ứng dụng bao gồm người dùng, sản phẩm và danh mục.
- **AWS IAM** – Quản lý quyền truy cập đối với các tài nguyên AWS.
- **Amazon CloudWatch** – Thu thập nhật ký ứng dụng và thông tin giám sát.

### Thiết kế thành phần

**Giao diện người dùng**

- HTML
- CSS
- Bootstrap
- JavaScript
- EJS Template Engine

**Phần phụ trợ**

- Node.js
- Express.js
- Express Session
- Multer
- AWS SDK for JavaScript

**Cơ sở dữ liệu**

- MongoDB Atlas

**Lưu trữ hình ảnh**

- Amazon S3

**Nền tảng Container**

- Docker
- Amazon ECS Fargate

**Quy trình triển khai**

- GitHub
- AWS CodeBuild
- Amazon ECR
- Amazon ECS
---

# 4. Triển khai kỹ thuật

## Các giai đoạn triển khai

Dự án được triển khai thông qua các giai đoạn sau:

- Nghiên cứu kiến trúc đám mây AWS và chiến lược triển khai.
- Thiết kế kiến trúc tổng thể của hệ thống sàn giao dịch.
- Phát triển phần phụ trợ bằng Node.js và Express.js.
- Cấu hình MongoDB Atlas để lưu trữ cơ sở dữ liệu trên nền tảng đám mây.
- Tích hợp Amazon S3 để lưu trữ hình ảnh sản phẩm.
- Đóng gói ứng dụng bằng Docker.
- Đẩy Docker image lên Amazon ECR.
- Triển khai Docker container trên Amazon ECS Fargate.
- Cấu hình Application Load Balancer.
- Triển khai Continuous Integration và Continuous Deployment bằng GitHub và AWS CodeBuild.
- Thực hiện kiểm thử, tối ưu hóa và triển khai lên môi trường thực tế.

## Yêu cầu kỹ thuật

### Ngôn ngữ lập trình

- JavaScript
- HTML
- CSS

### Framework

- Express.js
- EJS

### Cơ sở dữ liệu

- MongoDB Atlas

### Dịch vụ đám mây

- Amazon ECS Fargate
- Amazon ECR
- Amazon S3
- AWS CodeBuild
- Application Load Balancer
- Amazon CloudWatch
- AWS IAM

### Công cụ phát triển

- Visual Studio Code
- Git
- GitHub
- Docker Desktop
- MongoDB Compass

---

# 5. Lộ trình & Mốc triển khai

### Tuần 1

- Phân tích yêu cầu.
- Lập kế hoạch kiến trúc AWS.
- Thiết kế cơ sở dữ liệu.

### Tuần 2

- Xác thực người dùng.
- CRUD sản phẩm.
- Quản lý danh mục.

### Tuần 3

- Tích hợp MongoDB Atlas.
- Tích hợp Amazon S3.
- Tải lên hình ảnh sản phẩm.

### Tuần 4

- Đóng gói ứng dụng bằng Docker.
- Cấu hình Amazon ECR.

### Tuần 5

- Triển khai Amazon ECS.
- Cấu hình Application Load Balancer.

### Tuần 6

- Tích hợp GitHub.
- Cấu hình AWS CodeBuild.
- Quy trình triển khai tự động.

### Tuần 7

- Kiểm thử.
- Sửa lỗi.
- Tối ưu hiệu năng.

### Tuần 8

- Triển khai cuối cùng.
- Tài liệu.
- Trình bày dự án.

---

# 6. Ước tính ngân sách

## Ước tính chi phí hạ tầng

| Dịch vụ | Chi phí ước tính |
|----------|------------------|
| Amazon ECS Fargate | ~$0.25/tháng |
| Amazon S3 (Lưu trữ & Yêu cầu truy cập) | ~$0.15/tháng |
| Amazon ECR | ~$0.03/tháng |
| AWS CodeBuild | ~$0.05/tháng |
| Application Load Balancer | ~$0.10/tháng |
| Amazon CloudWatch | ~$0.02/tháng |
| **Tổng chi phí ước tính** | **~$0.60 USD/tháng** |

### Hướng dẫn kiểm soát chi phí

- **AWS Budgets:** Thiết lập cảnh báo tự động khi chi phí vượt **5.00 USD** và **10.00 USD**.
- **Amazon S3 Lifecycle Rules:** Tự động xóa hình ảnh sản phẩm không còn sử dụng sau **30 ngày**.
- **Amazon ECR Lifecycle Policy:** Tự động xóa các Docker image cũ để giảm chi phí lưu trữ.
- **AWS CodeBuild:** Chỉ kích hoạt quá trình build khi có mã nguồn mới được đẩy lên GitHub.
- **Dọn dẹp sau khi trình diễn:** Xóa các ECS Service, Docker image trong Amazon ECR và các đối tượng không sử dụng trong Amazon S3 để tránh phát sinh chi phí.
---

# 7. Đánh giá rủi ro

## Ma trận rủi ro

- Triển khai ECS thất bại.
- Sự cố kết nối MongoDB Atlas.
- Lỗi tải lên Amazon S3.
- Chi phí dịch vụ AWS phát sinh ngoài dự kiến.

## Chiến lược giảm thiểu

- Kích hoạt giám sát bằng Amazon CloudWatch.
- Cấu hình cảnh báo AWS Budgets.
- Quản lý phiên bản Docker image bằng Amazon ECR.
- Thực hiện sao lưu MongoDB Atlas định kỳ.
- Áp dụng nguyên tắc phân quyền tối thiểu (Least Privilege) của IAM.

## Kế hoạch dự phòng

- Quay lại Docker image trước đó.
- Triển khai lại ECS Task Definition trước đó.
- Khôi phục bản sao lưu MongoDB Atlas.
- Triển khai lại thông qua AWS CodeBuild.

---

# 8. Kết quả kỳ vọng

## Kết quả kỹ thuật

Dự án hoàn thành sẽ cung cấp:
 
- Một nền tảng mua bán đồ cũ cloud-native được đóng gói hoàn toàn bằng container.
- Quy trình triển khai CI/CD tự động sử dụng GitHub và AWS CodeBuild.
- Hệ thống lưu trữ hình ảnh đáng tin cậy sử dụng Amazon S3.
- Khả năng triển khai container có thể mở rộng bằng Amazon ECS Fargate.
- Cơ sở dữ liệu tập trung trên nền tảng đám mây sử dụng MongoDB Atlas.
- Kiến trúc đám mây sẵn sàng cho môi trường thực tế với cân bằng tải.

## Giá trị kinh doanh

Nền tảng thể hiện việc triển khai thực tế các công nghệ điện toán đám mây, container hóa và DevOps bằng các dịch vụ được quản lý của AWS. Kiến trúc có thể được mở rộng trong tương lai để hỗ trợ cổng thanh toán, hệ thống gợi ý, dịch vụ thông báo, phân tích dữ liệu và phát triển theo kiến trúc microservices, đồng thời vẫn duy trì khả năng mở rộng và hiệu quả vận hành.
