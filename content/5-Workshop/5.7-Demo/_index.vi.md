---
title: "Demo Giao diện Ứng dụng Thực tế"
date: 2026-07-20
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

#### Tổng quan Demo

Trong phần này, chúng ta sẽ xem qua các giao diện chính của ứng dụng Medi Path Ease và cách các thành phần AWS tích hợp với nhau.

> **📸 [HÌNH 1]: Trang chủ Medi Path Ease**

#### 1. Trang Đăng nhập / Đăng ký

**Chức năng:** Xác thực người dùng với Amazon Cognito

```
┌─────────────────────────────────────────┐
│           Medi Path Ease                │
│                                         │
│    [🔐 Logo]                           │
│                                         │
│    Chào mừng đến với Medi Path Ease   │
│    Kết nối bác sĩ và bệnh nhân         │
│                                         │
│    ┌─────────────────────────────┐     │
│    │ Email                        │     │
│    └─────────────────────────────┘     │
│    ┌─────────────────────────────┐     │
│    │ Mật khẩu              👁️   │     │
│    └─────────────────────────────┘     │
│                                         │
│    [      Đăng nhập      ]             │
│                                         │
│    Quên mật khẩu?                      │
│    ─────────────────────────────        │
│    Chưa có tài khoản?                  │
│    [      Đăng ký      ]               │
│                                         │
│    Đăng nhập với:                      │
│    [G] [F] [Apple]                     │
└─────────────────────────────────────────┘
```

> **📸 [HÌNH 2]: Trang Login với Cognito Authentication**

**AWS Services sử dụng:**
- **Amazon Cognito** - User Pool cho xác thực
- **Cognito JWT Tokens** - Access Token, ID Token, Refresh Token

> **📸 [HÌNH 3]: Cognito User Pool trên AWS Console**

#### 2. Dashboard Bệnh nhân

**Chức năng:** Tổng quan thông tin bệnh nhân

```
┌────────────────────────────────────────────────────────────────┐
│  [👤] Medi Path Ease           [🔔] [⚙️] [🚪]                │
├──────────┬─────────────────────────────────────────────────────┤
│          │                                                     │
│ Dashboard│  Chào buổi sáng, Nguyễn Văn A                       │
│ ────────│                                                     │
│ 👤 Hồ sơ│  ┌──────────┐ ┌──────────┐ ┌──────────┐              │
│ 📅 Lịch │  │ Cuộc hẹn │ │ Tin nhắn │ │  Thanh   │              │
│ 💬 Chat │  │    3     │ │    5     │ │   toán   │              │
│ 📋 KQXN │  └──────────┘ └──────────┘ └──────────┘              │
│ 💊 Đơn  │                                                     │
│ 💊 TT   │  Lịch hẹn sắp tới                                   │
│ 🤖 AI   │  ┌─────────────────────────────────────────┐         │
│ 💳 TT   │  │ 📅 20/07/2026 - 09:00                  │         │
│ ────────│  │ Bác sĩ: Nguyễn Thị B                   │         │
│ ⚙️ Cài │  │ Chuyên khoa: Nội tổng quát             │         │
│  đặt  │  └─────────────────────────────────────────┘         │
│          │  ┌─────────────────────────────────────────┐         │
│          │  │ 📅 22/07/2026 - 14:00                  │         │
│          │  │ Bác sĩ: Trần Văn C                    │         │
│          │  │ Chuyên khoa: Da liễu                  │         │
│          │  └─────────────────────────────────────────┘         │
│          │                                                     │
│          │  Kết quả xét nghiệm mới                            │
│          │  ┌─────────────────────────────────────────┐         │
│          │  │ 📄 Xét nghiệm máu - 18/07/2026         │         │
│          │  │    [Xem chi tiết]                      │         │
│          │  └─────────────────────────────────────────┘         │
│          │                                                     │
└──────────┴─────────────────────────────────────────────────────┘
```

> **📸 [HÌNH 4]: Patient Dashboard**

**AWS Services sử dụng:**
- **Amazon RDS PostgreSQL** - Lấy dữ liệu lịch hẹn, hồ sơ
- **Amazon S3** - Hiển thị hình ảnh hồ sơ

#### 3. Upload Kết quả Xét nghiệm

**Chức năng:** Upload tài liệu y tế lên S3

```
┌────────────────────────────────────────────────────────────────┐
│  Upload Kết quả Xét nghiệm                                     │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │                                                          │   │
│  │              📄                                          │   │
│  │                                                          │   │
│  │        Kéo thả file vào đây                            │   │
│  │        hoặc click để chọn                              │   │
│  │                                                          │   │
│  │        Định dạng: PDF, JPG, PNG                         │   │
│  │        Dung lượng tối đa: 10MB                          │   │
│  │                                                          │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  File đã chọn:                                                 │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ 📄 xet_nghiem_mau.pdf                           [✕]   │   │
│  │ ████████████████████░░░░░░░░░░░░░  65%               │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  Mô tả (tùy chọn):                                            │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ Xét nghiệm máu định kỳ tháng 7                        │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  [                    Upload                        ]           │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

> **📸 [HÌNH 5]: Upload Lab Results UI**

**AWS Services sử dụng:**
- **Amazon S3** - Lưu trữ file
- **S3 Presigned URL** - Upload trực tiếp
- **Amazon CloudFront** - CDN cho file tĩnh

> **📸 [HÌNH 6]: S3 Bucket với các file đã upload**

#### 4. Trợ lý AI Chatbot

**Chức năng:** Chat với AI trợ lý y tế

```
┌────────────────────────────────────────────────────────────────┐
│  Trợ lý Y tế AI                                        [─][×] │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ [🤖] Chào bạn! Tôi là trợ lý y tế của Medi Path Ease. │   │
│  │      Tôi có thể giúp gì cho bạn hôm nay?              │   │
│  │      10:30                                            │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │                                              [👤]     │   │
│  │ Tôi bị đau đầu thường xuyên, nên làm gì?   10:31    │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ [🤖] Xin chào! Đau đầu có thể do nhiều nguyên nhân   │   │
│  │      như: thiếu ngủ, căng thẳng, thiếu nước...      │   │
│  │                                                          │   │
│  │      💡 Bạn nên:                                        │   │
│  │      1. Ngủ đủ 7-8 tiếng mỗi đêm                     │   │
│  │      2. Uống đủ 2-3 lít nước/ngày                   │   │
│  │      3. Giảm stress, nghỉ ngơi đầy đủ               │   │
│  │                                                          │   │
│  │      ⚠️ Nếu đau đầu kéo dài > 3 ngày, tôi khuyên     │   │
│  │         bạn nên đặt lịch khám với bác sĩ nhé!       │   │
│  │      10:31                                            │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│  [💬 Nhập câu hỏi của bạn...                    ] [Gửi]     │
│                                                                │
│  💡 Gợi ý:                                                    │
│  • Triệu chứng COVID-19 thường gặp                           │
│  • Cách chăm sóc sức khỏe mùa hè                            │
│  • Khi nào cần tiêm vaccine?                                  │
└────────────────────────────────────────────────────────────────┘
```

> **📸 [HÌNH 7]: AI Chatbot Interface**

**AWS Services sử dụng:**
- **Amazon Bedrock** - Claude 3.5 Sonnet
- **Bedrock Guardrails** - Lọc nội dung

> **📸 [HÌNH 8]: Amazon Bedrock Playground - Test Claude**

#### 5. Thanh toán

**Chức năng:** Thanh toán qua PayOS

```
┌────────────────────────────────────────────────────────────────┐
│  Thanh toán                                                  │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  Chi tiết thanh toán                                          │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ 🏥 Phí tư vấn - Bác sĩ Nguyễn Thị B                   │   │
│  │ 📅 20/07/2026 - 09:00                                 │   │
│  ├────────────────────────────────────────────────────────┤   │
│  │                                                        │   │
│  │ Tạm tính:                          200,000 đ          │   │
│  │ Phí dịch vụ:                            10,000 đ      │   │
│  │ ─────────────────────────────────────────────────     │   │
│  │ Tổng cộng:                          210,000 đ        │   │
│  │                                                        │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  Phương thức thanh toán                                       │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ ○ [💳] Thẻ ATM/Visa/MasterCard                        │   │
│  │ ○ [📱] VNPay QR Code                                  │   │
│  │ ○ [📱] Ví điện tử (Momo, ZaloPay...)                 │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ [           Thanh toán ngay - 210,000đ           ]      │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  🔒 Thanh toán an toàn qua PayOS                              │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

> **📸 [HÌNH 9]: Payment Page với PayOS Integration**

#### 6. Trang Quản trị (Admin)

**Chức năng:** Quản lý hệ thống

> **📸 [HÌNH 10]: Admin Dashboard**

**AWS Services sử dụng:**
- **Amazon RDS PostgreSQL** - Dữ liệu người dùng
- **Amazon CloudWatch** - Monitoring

#### Tổng kết hình ảnh cần chuẩn bị

| STT | Mô tả hình | Tên file |
|-----|------------|----------|
| 1 | Trang chủ Medi Path Ease | `demo-01-homepage.png` |
| 2 | Trang Login với Cognito | `demo-02-login.png` |
| 3 | Cognito User Pool Console | `demo-03-cognito-pool.png` |
| 4 | Patient Dashboard | `demo-04-patient-dashboard.png` |
| 5 | Upload Lab Results UI | `demo-05-upload-ui.png` |
| 6 | S3 Bucket với files | `demo-06-s3-bucket.png` |
| 7 | AI Chatbot Interface | `demo-07-chatbot.png` |
| 8 | Bedrock Playground | `demo-08-bedrock-playground.png` |
| 9 | Payment Page | `demo-09-payment.png` |
| 10 | Admin Dashboard | `demo-10-admin-dashboard.png` |
