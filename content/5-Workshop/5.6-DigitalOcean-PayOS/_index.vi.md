---
title: "Cấu hình DigitalOcean & Tích hợp PayOS"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Giới thiệu DigitalOcean App Platform

**DigitalOcean App Platform** là nền tảng PaaS cho phép deploy ứng dụng web một cách nhanh chóng mà không cần quản lý infrastructure phức tạp.

##### Tại sao chọn DigitalOcean?

| Ưu điểm | Mô tả |
|---------|--------|
| **Dễ sử dụng** | Deploy chỉ với vài click hoặc doctl CLI |
| **Auto-scaling** | Tự động scale theo lưu lượng |
| **Git deploy** | Tự động deploy khi push code |
| **Managed Database** | Có thể dùng kèm PostgreSQL managed |
| **Giá cả** | Chi phí hợp lý, free tier cho dev |

#### Deploy Backend lên DigitalOcean

##### Bước 1: Chuẩn bị Project

```bash
# Kiểm tra cấu trúc project
ls -la

# Project structure
# ├── server/
# │   ├── src/
# │   │   ├── routes/
# │   │   ├── services/
# │   │   └── index.js
# │   └── package.json
# └── package.json (root)
```

##### Bước 2: Tạo App Spec

Tạo file `app.yaml` trong thư mục server:

```yaml
name: medi-path-ease-api
region: sin
runtime: node

# Build settings
build_command: npm install

# Run settings  
run_command: npm start

# Environment variables
env:
  - key: NODE_ENV
    value: production
  - key: PORT
    value: 8080
  - key: DATABASE_URL
    value: ${db.DATABASE_URL}
  - key: JWT_SECRET
    value: ${jwt-secret}
  - key: AWS_REGION
    value: ap-southeast-1
  - key: AWS_ACCESS_KEY_ID
    value: ${aws-access-key}
  - key: AWS_SECRET_ACCESS_KEY
    value: ${aws-secret-key}
  - key: AWS_S3_BUCKET
    value: medi-path-ease-uploads
  - key: COGNITO_USER_POOL_ID
    value: ${cognito-pool-id}
  - key: COGNITO_CLIENT_ID
    value: ${cognito-client-id}

# HTTP routes
http_port: 8080

# Health check
health_check_path: /api/health

# Scaling
scaling:
  - match: ""
    min_instances: 1
    max_instances: 3
    target_cpu_percent: 75
```

> **📸 [HÌNH 1]: DigitalOcean App Platform Dashboard**

##### Bước 3: Deploy qua CLI

```bash
# Cài đặt doctl
# macOS
brew install doctl

# Windows (scoop)
scoop install doctl

# Authenticate
doctl auth init

# Tạo app
doctl apps create --spec app.yaml
```

##### Bước 4: Cấu hình Domain (Optional)

```bash
# Thêm domain tùy chỉnh
doctl apps update <app-id> --spec app.yaml

# Trỏ DNS về DigitalOcean
# CNAME: api.medipathease.com -> <app-endpoint>.ondigitalocean.app
```

#### Tích hợp PayOS

##### Giới thiệu PayOS

**PayOS** là cổng thanh toán trung gian hỗ trợ:
- Thanh toán qua thẻ ATM/Visa/MasterCard
- Thanh toán qua VNPay QR
- Thanh toán qua ví điện tử
- Tự động hoàn tiền

##### Bước 1: Đăng ký tài khoản PayOS

1. Truy cập https://payos.vn
2. Đăng ký merchant account
3. Lấy `Client ID` và `Api Key`

> **📸 [HÌNH 2]: PayOS Merchant Dashboard**

##### Bước 2: Cài đặt SDK

```bash
npm install payos-checkout
```

##### Bước 3: PayOS Service

```javascript
// services/payosService.js
import payos from "payos-checkout";

const payOSClient = new payos({
    clientId: process.env.PAYOS_CLIENT_ID,
    apiKey: process.env.PAYOS_API_KEY,
    checksumKey: process.env.PAYOS_CHECKSUM_KEY
});

/**
 * Tạo payment link
 */
export async function createPaymentLink({ amount, orderId, description, returnUrl, cancelUrl }) {
    const paymentData = {
        amount: amount,              // Số tiền (VND)
        orderId: String(orderId),    // Mã đơn hàng
        description: description,    // Mô tả
        returnUrl: returnUrl,       // URL callback khi thành công
        cancelUrl: cancelUrl        // URL callback khi hủy
    };

    const result = await payOSClient.createPaymentLink(paymentData);

    return {
        checkoutUrl: result.checkoutUrl,
        paymentId: result.paymentId,
        orderId: orderId
    };
}

/**
 * Xác minh webhook payment
 */
export async function verifyWebhook(data) {
    return payOSClient.verifyPaymentWebhook(data);
}

/**
 * Lấy thông tin payment
 */
export async function getPaymentInfo(paymentId) {
    return await payOSClient.getPaymentStatus(paymentId);
}

/**
 * Hoàn tiền
 */
export async function refundPayment(paymentId, amount, reason) {
    return await payOSClient.refundPayment(paymentId, amount, reason);
}
```

##### Bước 4: Payment Routes

```javascript
// routes/payment.js
import express from "express";
import * as payosService from "../services/payosService.js";

const router = express.Router();

/**
 * POST /api/payment/create
 * Tạo payment link
 */
router.post("/create", authMiddleware, async (req, res) => {
    try {
        const { amount, description } = req.body;
        const userId = req.user.sub;

        // Tạo order ID unique
        const orderId = `ORD-${Date.now()}-${userId}`;

        const result = await payosService.createPaymentLink({
            amount,
            orderId,
            description: description || "Thanh toán Medi Path Ease",
            returnUrl: `${process.env.FRONTEND_URL}/payment/success?orderId=${orderId}`,
            cancelUrl: `${process.env.FRONTEND_URL}/payment/cancel?orderId=${orderId}`
        });

        // Lưu order vào database
        await saveOrder({
            orderId,
            userId,
            amount,
            status: "PENDING",
            checkoutUrl: result.checkoutUrl
        });

        res.json({
            success: true,
            checkoutUrl: result.checkoutUrl,
            orderId
        });
    } catch (error) {
        console.error("Payment error:", error);
        res.status(500).json({ error: "Failed to create payment" });
    }
});

/**
 * POST /api/payment/webhook
 * Webhook nhận thông tin thanh toán từ PayOS
 */
router.post("/webhook", async (req, res) => {
    try {
        const isValid = payosService.verifyWebhook(req.body);

        if (isValid) {
            const { orderId, paymentId, status } = req.body;

            // Update order status
            await updateOrderStatus(orderId, {
                paymentId,
                status: status === "PAID" ? "COMPLETED" : "FAILED"
            });

            res.json({ success: true });
        } else {
            res.status(400).json({ error: "Invalid webhook" });
        }
    } catch (error) {
        console.error("Webhook error:", error);
        res.status(500).json({ error: "Webhook processing failed" });
    }
});

/**
 * GET /api/payment/:orderId
 * Lấy thông tin payment
 */
router.get("/:orderId", authMiddleware, async (req, res) => {
    try {
        const order = await getOrder(req.params.orderId);
        
        if (!order) {
            return res.status(404).json({ error: "Order not found" });
        }

        res.json({
            success: true,
            order
        });
    } catch (error) {
        res.status(500).json({ error: "Failed to get order" });
    }
});

export default router;
```

##### Bước 5: Frontend Payment Flow

```jsx
// components/PaymentButton.jsx
import { api } from "../../services/api";

export function PaymentButton({ amount, description, onSuccess, onError }) {
    const handlePayment = async () => {
        try {
            const response = await api.post("/payment/create", {
                amount,
                description
            });

            if (response.checkoutUrl) {
                // Redirect đến trang thanh toán PayOS
                window.location.href = response.checkoutUrl;
            }
        } catch (error) {
            onError?.(error.message);
        }
    };

    return (
        <button onClick={handlePayment} className="payment-btn">
            Thanh toán {amount.toLocaleString("vi-VN")}đ
        </button>
    );
}
```

#### Environment Variables

```bash
# .env
# DigitalOcean
DIGITALOCEAN_APP_URL=https://medi-path-ease-api.ondigitalocean.app

# PayOS
PAYO_S_CLIENT_ID=your-client-id
PAYO_S_API_KEY=your-api-key
PAYO_S_CHECKSUM_KEY=your-checksum-key

# Frontend
FRONTEND_URL=http://localhost:5173
```

#### Resource Summary

| Resource | Giá trị | Mô tả |
|---------|---------|--------|
| App | `medi-path-ease-api` | Backend API |
| Region | Singapore (sin) | Gần Việt Nam |
| Scaling | 1-3 instances | Auto-scale |
| PayOS | Sandbox/Production | Payment gateway |

> **📸 [HÌNH 3]: DigitalOcean App Deployment Status**

> **📸 [HÌNH 4]: PayOS Payment Flow**

#### Tài liệu tham khảo

* [DigitalOcean App Platform](https://www.digitalocean.com/products/app-platform)
* [DigitalOcean doctl](https://docs.digitalocean.com/reference/doctl/)
* [PayOS Documentation](https://docs.payos.vn/)
* [PayOS Node.js SDK](https://github.com/payos-vn/payos-checkout)
