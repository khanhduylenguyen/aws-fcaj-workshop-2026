---
title: "DigitalOcean Configuration & PayOS Integration"
date: 2026-07-20
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Introduction to DigitalOcean App Platform

**DigitalOcean App Platform** is a PaaS platform that allows quick web application deployment without complex infrastructure management.

##### Why Choose DigitalOcean?

| Advantage | Description |
|-----------|-------------|
| **Easy to use** | Deploy with just a few clicks or doctl CLI |
| **Auto-scaling** | Automatically scale with traffic |
| **Git deploy** | Auto-deploy when pushing code |
| **Managed Database** | Can use PostgreSQL managed |
| **Pricing** | Affordable, free tier for dev |

#### Deploy Backend to DigitalOcean

##### Step 1: Prepare Project

```bash
# Check project structure
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

##### Step 2: Create App Spec

Create `app.yaml` in the server folder:

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

> **📸 [IMAGE 1]: DigitalOcean App Platform Dashboard**

##### Step 3: Deploy via CLI

```bash
# Install doctl
# macOS
brew install doctl

# Windows (scoop)
scoop install doctl

# Authenticate
doctl auth init

# Create app
doctl apps create --spec app.yaml
```

##### Step 4: Configure Domain (Optional)

```bash
# Add custom domain
doctl apps update <app-id> --spec app.yaml

# Point DNS to DigitalOcean
# CNAME: api.medipathease.com -> <app-endpoint>.ondigitalocean.app
```

#### PayOS Integration

##### Introduction to PayOS

**PayOS** is a payment gateway supporting:
- ATM/Visa/MasterCard payments
- VNPay QR payments
- E-wallet payments
- Auto refunds

##### Step 1: Register PayOS Account

1. Access https://payos.vn
2. Register merchant account
3. Get `Client ID` and `Api Key`

> **📸 [IMAGE 2]: PayOS Merchant Dashboard**

##### Step 2: Install SDK

```bash
npm install payos-checkout
```

##### Step 3: PayOS Service

```javascript
// services/payosService.js
import payos from "payos-checkout";

const payOSClient = new payos({
    clientId: process.env.PAYOS_CLIENT_ID,
    apiKey: process.env.PAYOS_API_KEY,
    checksumKey: process.env.PAYOS_CHECKSUM_KEY
});

/**
 * Create payment link
 */
export async function createPaymentLink({ amount, orderId, description, returnUrl, cancelUrl }) {
    const paymentData = {
        amount: amount,
        orderId: String(orderId),
        description: description,
        returnUrl: returnUrl,
        cancelUrl: cancelUrl
    };

    const result = await payOSClient.createPaymentLink(paymentData);

    return {
        checkoutUrl: result.checkoutUrl,
        paymentId: result.paymentId,
        orderId: orderId
    };
}

/**
 * Verify webhook payment
 */
export async function verifyWebhook(data) {
    return payOSClient.verifyPaymentWebhook(data);
}

/**
 * Get payment info
 */
export async function getPaymentInfo(paymentId) {
    return await payOSClient.getPaymentStatus(paymentId);
}

/**
 * Refund
 */
export async function refundPayment(paymentId, amount, reason) {
    return await payOSClient.refundPayment(paymentId, amount, reason);
}
```

##### Step 4: Payment Routes

```javascript
// routes/payment.js
import express from "express";
import * as payosService from "../services/payosService.js";

const router = express.Router();

/**
 * POST /api/payment/create
 * Create payment link
 */
router.post("/create", authMiddleware, async (req, res) => {
    try {
        const { amount, description } = req.body;
        const userId = req.user.sub;

        // Create unique order ID
        const orderId = `ORD-${Date.now()}-${userId}`;

        const result = await payosService.createPaymentLink({
            amount,
            orderId,
            description: description || "Medi Path Ease Payment",
            returnUrl: `${process.env.FRONTEND_URL}/payment/success?orderId=${orderId}`,
            cancelUrl: `${process.env.FRONTEND_URL}/payment/cancel?orderId=${orderId}`
        });

        // Save order to database
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
 * Webhook receives payment info from PayOS
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
 * Get payment info
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

##### Step 5: Frontend Payment Flow

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
                // Redirect to PayOS payment page
                window.location.href = response.checkoutUrl;
            }
        } catch (error) {
            onError?.(error.message);
        }
    };

    return (
        <button onClick={handlePayment} className="payment-btn">
            Pay {amount.toLocaleString("vi-VN")}đ
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

| Resource | Value | Description |
|---------|--------|-------------|
| App | `medi-path-ease-api` | Backend API |
| Region | Singapore (sin) | Near Vietnam |
| Scaling | 1-3 instances | Auto-scale |
| PayOS | Sandbox/Production | Payment gateway |

> **📸 [IMAGE 3]: DigitalOcean App Deployment Status**

> **📸 [IMAGE 4]: PayOS Payment Flow**

#### References

* [DigitalOcean App Platform](https://www.digitalocean.com/products/app-platform)
* [DigitalOcean doctl](https://docs.digitalocean.com/reference/doctl/)
* [PayOS Documentation](https://docs.payos.vn/)
* [PayOS Node.js SDK](https://github.com/payos-vn/payos-checkout)
