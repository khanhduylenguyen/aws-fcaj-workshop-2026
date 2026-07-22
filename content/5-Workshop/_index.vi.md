---
title: "Workshop"
date: 2026-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Triển khai nền tảng thương mại điện tử TechMarket trên AWS

#### Tổng quan

Trong workshop này, chúng ta sẽ xây dựng và triển khai **nền tảng thương mại điện tử TechMarket** bằng kiến trúc cloud-native trên AWS.

Giải pháp sử dụng các dịch vụ AWS như **Amazon ECS Fargate**, **Amazon ECR**, **Amazon S3**, **AWS CodeBuild**, **Application Load Balancer**, **Amazon CloudWatch**, **Amazon Route 53** và **AWS Certificate Manager (ACM)**, kết hợp với **MongoDB Atlas** nhằm xây dựng một nền tảng có khả năng mở rộng, bảo mật, tính sẵn sàng cao và hỗ trợ triển khai tự động.

Trong suốt workshop này, bạn sẽ chuẩn bị môi trường dự án, cấu hình hạ tầng mạng, tích hợp các dịch vụ của ứng dụng, đóng gói ứng dụng bằng Docker, triển khai lên Amazon ECS Fargate, cấu hình tên miền và HTTPS, tự động hóa quá trình triển khai bằng AWS CodeBuild, giám sát hệ thống, thực hiện kiểm thử toàn bộ ứng dụng và cuối cùng dọn dẹp tất cả tài nguyên AWS đã tạo.

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview/)
2. [Điều kiện chuẩn bị](5.2-Prerequiste/)
3. [Chuẩn bị nền tảng dự án](5.3-Project-foundation/)
4. [Cấu hình hạ tầng mạng](5.4-Networking/)
5. [Cấu hình các dịch vụ ứng dụng](5.5-Application-Services/)
6. [Đóng gói ứng dụng bằng Docker](5.6-Containerization/)
7. [Triển khai ứng dụng](5.7-Deploy-Application/)
8. [Cấu hình tên miền và HTTPS](5.8-Domain-and-HTTPS/)
9. [Thiết lập CI/CD](5.9-CI-CD/)
10. [Giám sát hệ thống](5.10-Monitoring/)
11. [Kiểm thử hệ thống](5.11-Testing/)
12. [Dọn dẹp tài nguyên](5.12-Cleanup/)