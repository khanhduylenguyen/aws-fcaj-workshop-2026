---
title : "Kiểm thử"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.11. </b> "
---

# 5.11. Kiểm thử

Phần này trình bày quá trình triển khai hoàn chỉnh nền tảng thương mại điện tử TechMarket trên AWS. Đồng thời minh họa các giao diện chính của ứng dụng và giải thích cách từng chức năng tương tác với hạ tầng đám mây đã triển khai, bao gồm Amazon ECS Fargate, Amazon S3, MongoDB Atlas, Amazon Route 53, AWS Certificate Manager (ACM), Application Load Balancer, Amazon CloudWatch, Amazon ECR và AWS CodeBuild.

---

# 1. Trang web khách hàng

## A. Trang chủ

Trang chủ là giao diện chính nơi khách hàng duyệt các sản phẩm có trên nền tảng TechMarket.

### Tích hợp hạ tầng

Ứng dụng Node.js Express được triển khai trên Amazon ECS Fargate và được truy cập thông qua Application Load Balancer. Route 53 phân giải tên miền tùy chỉnh trong khi ACM cung cấp kết nối HTTPS bảo mật. Thông tin sản phẩm được lấy từ MongoDB Atlas và hình ảnh sản phẩm được tải từ Amazon S3.

### Luồng dữ liệu

Khách hàng truy cập **https://techmarketstore.store**

↓

Route 53 phân giải tên miền.

↓

Application Load Balancer chuyển tiếp yêu cầu.

↓

Amazon ECS xử lý yêu cầu.

↓

MongoDB Atlas trả về thông tin sản phẩm.

↓

Amazon S3 trả về hình ảnh sản phẩm.

↓

Trang chủ được hiển thị.

<br>

![Homepage](/images/5-Workshop/5.11-Testing/homepage.png)

---

## B. Đăng ký người dùng

Khách hàng có thể đăng ký tài khoản mới trước khi sử dụng các chức năng mua sắm.

### Tích hợp hạ tầng

Yêu cầu đăng ký được xử lý bởi ứng dụng Express đang chạy trên Amazon ECS. Thông tin người dùng được kiểm tra trước khi lưu vào MongoDB Atlas. Mật khẩu được mã hóa trước khi lưu trữ.

![Register Type](/images/5-Workshop/5.11-Testing/register-type.jpg)

![Customer Register](/images/5-Workshop/5.11-Testing/customer-register.jpg)

---

## C. Đăng ký cửa hàng

Khách hàng có thể đăng ký cửa hàng trực tuyến của riêng mình để trở thành người bán trên nền tảng.

### Tích hợp hạ tầng

Yêu cầu đăng ký cửa hàng được gửi đến backend Node.js chạy trên Amazon ECS. Thông tin cửa hàng được lưu vào MongoDB Atlas và chờ quản trị viên phê duyệt.

![Shop Register](/images/5-Workshop/5.11-Testing/shop-register.jpg)

---

## D. Đăng nhập

Người dùng đăng nhập bằng email và mật khẩu trước khi truy cập các chức năng yêu cầu xác thực.

### Tích hợp hạ tầng

Quá trình xác thực được xử lý hoàn toàn bởi ứng dụng Express triển khai trên Amazon ECS. Thông tin đăng nhập được đối chiếu với MongoDB Atlas và phiên đăng nhập được quản lý bằng Express Session.

![Login](/images/5-Workshop/5.11-Testing/login.jpg)

---

## E. Hồ sơ người dùng

Trang hồ sơ cho phép khách hàng cập nhật thông tin cá nhân và tải lên ảnh đại diện.

### Tích hợp hạ tầng

Thông tin hồ sơ được lấy từ MongoDB Atlas. Ảnh đại diện được tải lên Amazon S3, trong khi đường dẫn ảnh được lưu trong MongoDB Atlas.

![Profile](/images/5-Workshop/5.11-Testing/profile.jpg)

---

## F. Chi tiết sản phẩm

Trang chi tiết sản phẩm hiển thị thông số kỹ thuật, mô tả, giá bán, tình trạng tồn kho và các tùy chọn mua hàng.

### Tích hợp hạ tầng

Thông tin sản phẩm được lấy từ MongoDB Atlas trong khi hình ảnh sản phẩm được phục vụ trực tiếp từ Amazon S3. Toàn bộ nghiệp vụ được xử lý bởi ứng dụng Node.js chạy trên Amazon ECS.

![Product Detail](/images/5-Workshop/5.11-Testing/product-detail.jpg)

---

## G. Trang thanh toán

Trang thanh toán cho phép khách hàng xem lại thông tin đơn hàng, thông tin khách hàng, phương thức thanh toán và xác nhận đơn hàng.

### Tích hợp hạ tầng

Yêu cầu thanh toán được xử lý bởi ứng dụng Node.js Express chạy trên Amazon ECS. Thông tin đơn hàng được lấy từ MongoDB Atlas trước khi xác nhận thanh toán.

![Payment Page](/images/5-Workshop/5.11-Testing/payment-page.jpg)

---

## H. Đặt hàng thành công

Sau khi xác nhận thanh toán, khách hàng sẽ nhận được thông báo đặt hàng thành công.

### Tích hợp hạ tầng

Ứng dụng ghi nhận đơn hàng hoàn tất vào MongoDB Atlas trước khi trả về trang xác nhận.

![Order Success](/images/5-Workshop/5.11-Testing/order-success.jpg)

---

# 2. Quản lý cửa hàng

## A. Thống kê hoa hồng

Chủ cửa hàng có thể theo dõi doanh thu và hoa hồng được tạo ra từ các đơn hàng đã hoàn thành.

### Tích hợp hạ tầng

Báo cáo doanh thu được tính toán động bởi backend dựa trên dữ liệu lưu trong MongoDB Atlas. Kết quả được hiển thị dưới dạng bảng thống kê.

![Commission Report](/images/5-Workshop/5.11-Testing/commission-report.jpg)

---

# 3. Trang quản trị

## A. Bảng điều khiển quản trị

Quản trị viên có thể theo dõi người dùng, cửa hàng và các thống kê tổng quan của hệ thống.

### Tích hợp hạ tầng

Bảng điều khiển quản trị tổng hợp dữ liệu trực tiếp từ MongoDB Atlas thông qua ứng dụng Express đang chạy trên Amazon ECS.

Thông tin hiển thị bao gồm:

- Tổng số người dùng
- Tổng số cửa hàng
- Trạng thái cửa hàng
- Thông tin người dùng

![Admin Dashboard](/images/5-Workshop/5.11-Testing/admin-dashboard.jpg)

---

## B. Quản lý đơn hàng

Quản trị viên quản lý các đơn hàng của khách hàng, theo dõi trạng thái đơn hàng và cập nhật quá trình xử lý.

### Tích hợp hạ tầng

Dữ liệu đơn hàng được lấy từ MongoDB Atlas thông qua backend Node.js triển khai trên Amazon ECS. Mọi thay đổi trạng thái đơn hàng được đồng bộ ngay với cơ sở dữ liệu sau khi thực hiện.

![Admin Order Management](/images/5-Workshop/5.11-Testing/admin-order-management.jpg)

---

## C. Thống kê hoa hồng

Quản trị viên có thể theo dõi hoa hồng toàn hệ thống, doanh thu theo tháng, số đơn hàng hoàn thành và trạng thái thanh toán hoa hồng.

### Tích hợp hạ tầng

Backend Node.js tổng hợp dữ liệu hoa hồng từ MongoDB Atlas và trả về các thống kê cho bảng điều khiển quản trị. Các phép tính được tạo động dựa trên các đơn hàng đã hoàn thành.

![Admin Commission Report](/images/5-Workshop/5.11-Testing/commission-report.jpg)

---

# Các dịch vụ AWS được sử dụng

Trong quá trình kiểm thử ứng dụng, nền tảng đã tích hợp các dịch vụ AWS sau:

| Dịch vụ AWS | Mục đích |
|-------------|----------|
| Amazon ECS Fargate | Lưu trữ và chạy ứng dụng Node.js Express |
| Amazon ECR | Lưu trữ Docker Image |
| Amazon S3 | Lưu trữ hình ảnh sản phẩm và ảnh đại diện người dùng |
| MongoDB Atlas | Lưu trữ dữ liệu của ứng dụng |
| Amazon Route 53 | Phân giải tên miền |
| AWS Certificate Manager | Cung cấp chứng chỉ HTTPS |
| Application Load Balancer | Phân phối lưu lượng truy cập |
| Amazon CloudWatch | Giám sát tình trạng hoạt động của ứng dụng |
| AWS CodeBuild | Tự động hóa quá trình build và triển khai |

Việc tất cả các giao diện hoạt động thành công chứng minh rằng ứng dụng TechMarket đã được triển khai hoàn chỉnh và vận hành ổn định trên hạ tầng điện toán đám mây AWS.