---
title: "Xác thực người dùng với Amazon Cognito"
date: 2026-07-20
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Giới thiệu Amazon Cognito

**Amazon Cognito** cung cấp giải pháp xác thực, ủy quyền và quản lý người dùng cho ứng dụng web và mobile. Cognito hỗ trợ đăng nhập qua email/password, OAuth (Google, Facebook), và SAML.

##### Tại sao chọn Cognito?

| Tính năng | Cognito | Giải pháp custom |
|-----------|---------|-------------------|
| User pools | ✅ | Cần tự xây dựng |
| Social login | ✅ OAuth/SAML | Cần tích hợp thủ công |
| MFA | ✅ TOTP/SMS | Cần tự implement |
| JWT tokens | ✅ Tự động | Cần thư viện bên thứ 3 |
| Password recovery | ✅ Built-in | Cần tự xây dựng |
| Scaling | ✅ Tự động | Cần infrastructure |

#### Cấu hình Cognito User Pool

##### Bước 1: Tạo User Pool

Truy cập AWS Console → Cognito → Create user pool

```bash
# Hoặc sử dụng AWS CLI
aws cognito-idp create-user-pool \
    --pool-name medi-path-ease-users \
    --auto-verified-attributes email \
    --username-attributes email \
    --password-policy "MinimumLength=8,RequireUppercase=true,RequireLowercase=true,RequireNumbers=true,RequireSymbols=true" \
    --region ap-southeast-1
```

> **📸 [HÌNH 1]: Cognito User Pool Configuration**

##### Bước 2: Cấu hình App Client

```bash
# Tạo App Client
aws cognito-idp create-user-pool-client \
    --user-pool-id <your-pool-id> \
    --client-name medi-path-ease-web \
    --no-generate-secret \
    --allowed-o-auth-flows authorization_code \
    --allowed-o-auth-scopes openid email profile \
    --allowed-o-auth-flows-user-pool-client \
    --callback-urls "http://localhost:5173/callback,https://mediathease.com/callback" \
    --logout-urls "http://localhost:5173/logout,https://mediathease.com/logout" \
    --region ap-southeast-1
```

> **📸 [HÌNH 2]: Cognito App Client Configuration**

##### Bước 3: Cấu hình Identity Pool (Optional)

```bash
# Tạo Identity Pool để truy cập AWS services
aws cognito-identity create-identity-pool \
    --identity-pool-name medi-path-ease-identities \
    --allow-unauthenticated-identities \
    --region ap-southeast-1
```

#### Tích hợp Backend

##### Cài đặt thư viện

```bash
npm install amazon-cognito-identity-js
# hoặc
npm install @aws-sdk/client-cognito-identity @aws-sdk/client-cognito-identity-provider
```

##### Cognito Service

```javascript
// services/cognitoService.js
import {
    CognitoIdentityProviderClient,
    SignUpCommand,
    ConfirmSignUpCommand,
    InitiateAuthCommand,
    GetUserCommand,
    ForgotPasswordCommand,
    ConfirmForgotPasswordCommand
} from "@aws-sdk/client-cognito-identity-provider";

const client = new CognitoIdentityProviderClient({
    region: process.env.AWS_REGION || "ap-southeast-1"
});

const USER_POOL_ID = process.env.COGNITO_USER_POOL_ID;
const CLIENT_ID = process.env.COGNITO_CLIENT_ID;

/**
 * Đăng ký user mới
 */
export async function signUp({ email, password, name }) {
    const command = new SignUpCommand({
        ClientId: CLIENT_ID,
        Username: email,
        Password: password,
        UserAttributes: [
            { Name: "email", Value: email },
            { Name: "name", Value: name },
            { Name: "custom:role", Value: "PATIENT" }
        ]
    });
    
    const response = await client.send(command);
    return {
        userId: response.UserSub,
        confirmed: false
    };
}

/**
 * Xác nhận email
 */
export async function confirmSignUp({ email, code }) {
    const command = new ConfirmSignUpCommand({
        ClientId: CLIENT_ID,
        Username: email,
        ConfirmationCode: code
    });
    
    await client.send(command);
    return { confirmed: true };
}

/**
 * Đăng nhập - lấy tokens
 */
export async function signIn({ email, password }) {
    const command = new InitiateAuthCommand({
        AuthFlow: "USER_PASSWORD_AUTH",
        ClientId: CLIENT_ID,
        AuthParameters: {
            USERNAME: email,
            PASSWORD: password
        }
    });
    
    const response = await client.send(command);
    
    return {
        accessToken: response.AuthenticationResult.AccessToken,
        idToken: response.AuthenticationResult.IdToken,
        refreshToken: response.AuthenticationResult.RefreshToken,
        expiresIn: response.AuthenticationResult.ExpiresIn
    };
}

/**
 * Lấy thông tin user từ token
 */
export async function getUser(accessToken) {
    const command = new GetUserCommand({
        AccessToken: accessToken
    });
    
    const response = await client.send(command);
    
    const userAttributes = {};
    response.UserAttributes.forEach(attr => {
        userAttributes[attr.Name] = attr.Value;
    });
    
    return {
        email: userAttributes.email,
        name: userAttributes.name,
        role: userAttributes["custom:role"],
        emailVerified: userAttributes.email_verified === "true"
    };
}

/**
 * Quên mật khẩu
 */
export async function forgotPassword({ email }) {
    const command = new ForgotPasswordCommand({
        ClientId: CLIENT_ID,
        Username: email
    });
    
    await client.send(command);
    return { sent: true };
}

/**
 * Reset mật khẩu
 */
export async function confirmForgotPassword({ email, code, newPassword }) {
    const command = new ConfirmForgotPasswordCommand({
        ClientId: CLIENT_ID,
        Username: email,
        ConfirmationCode: code,
        Password: newPassword
    });
    
    await client.send(command);
    return { reset: true };
}
```

##### Auth Middleware

```javascript
// middleware/auth.js
import jwt from "jsonwebtoken";
import jwkToPem from "jwk-to-pem";
import { getUser } from "../services/cognitoService.js";

let pems = null;

/**
 * Lấy public keys từ Cognito
 */
async function getPublicKeys() {
    if (pems) return pems;
    
    const response = await fetch(
        `https://cognito-idp.${process.env.AWS_REGION}.amazonaws.com/${process.env.COGNITO_USER_POOL_ID}/.well-known/jwks.json`
    );
    const data = await response.json();
    
    pems = {};
    data.keys.forEach(key => {
        pems[key.kid] = jwkToPem(key);
    });
    
    return pems;
}

/**
 * Middleware xác thực JWT
 */
export async function authMiddleware(req, res, next) {
    try {
        const authHeader = req.headers.authorization;
        
        if (!authHeader || !authHeader.startsWith("Bearer ")) {
            return res.status(401).json({ error: "No token provided" });
        }
        
        const token = authHeader.split(" ")[1];
        const decoded = jwt.decode(token, { complete: true });
        
        if (!decoded) {
            return res.status(401).json({ error: "Invalid token" });
        }
        
        const pems = await getPublicKeys();
        const pem = pems[decoded.header.kid];
        
        if (!pem) {
            return res.status(401).json({ error: "Invalid token signature" });
        }
        
        jwt.verify(token, pem, { algorithms: ["RS256"] }, (err, payload) => {
            if (err) {
                return res.status(401).json({ error: "Token expired or invalid" });
            }
            
            req.user = {
                sub: payload.sub,
                email: payload.email,
                role: payload["custom:role"]
            };
            next();
        });
    } catch (error) {
        return res.status(401).json({ error: "Authentication failed" });
    }
}

/**
 * Middleware kiểm tra role
 */
export function requireRole(allowedRoles) {
    return (req, res, next) => {
        if (!req.user) {
            return res.status(401).json({ error: "Not authenticated" });
        }
        
        if (!allowedRoles.includes(req.user.role)) {
            return res.status(403).json({ error: "Permission denied" });
        }
        
        next();
    };
}
```

##### Auth Routes

```javascript
// routes/auth.js
import express from "express";
import * as cognitoService from "../services/cognitoService.js";

const router = express.Router();

/**
 * POST /api/auth/register
 */
router.post("/register", async (req, res) => {
    try {
        const { email, password, name } = req.body;
        
        const result = await cognitoService.signUp({ email, password, name });
        res.json({ success: true, ...result });
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

/**
 * POST /api/auth/verify
 */
router.post("/verify", async (req, res) => {
    try {
        const { email, code } = req.body;
        
        const result = await cognitoService.confirmSignUp({ email, code });
        res.json({ success: true, ...result });
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

/**
 * POST /api/auth/login
 */
router.post("/login", async (req, res) => {
    try {
        const { email, password } = req.body;
        
        const tokens = await cognitoService.signIn({ email, password });
        
        res.json({
            success: true,
            accessToken: tokens.accessToken,
            idToken: tokens.idToken,
            refreshToken: tokens.refreshToken,
            expiresIn: tokens.expiresIn
        });
    } catch (error) {
        res.status(401).json({ error: "Invalid credentials" });
    }
});

/**
 * POST /api/auth/forgot-password
 */
router.post("/forgot-password", async (req, res) => {
    try {
        const { email } = req.body;
        await cognitoService.forgotPassword({ email });
        res.json({ success: true, message: "Code sent to email" });
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

/**
 * POST /api/auth/reset-password
 */
router.post("/reset-password", async (req, res) => {
    try {
        const { email, code, newPassword } = req.body;
        await cognitoService.confirmForgotPassword({ email, code, newPassword });
        res.json({ success: true, message: "Password reset successful" });
    } catch (error) {
        res.status(400).json({ error: error.message });
    }
});

/**
 * GET /api/auth/me
 */
router.get("/me", authMiddleware, async (req, res) => {
    try {
        const user = await cognitoService.getUser(req.headers.authorization.split(" ")[1]);
        res.json({ success: true, user });
    } catch (error) {
        res.status(401).json({ error: "Failed to get user" });
    }
});

export default router;
```

#### Environment Variables

```bash
# .env
AWS_REGION=ap-southeast-1
COGNITO_USER_POOL_ID=ap-southeast-1_xxxxxxx
COGNITO_CLIENT_ID=xxxxxxxxxxxxxxxxxxxxxxxxxx
COGNITO_SECRET_HASH=<secret-hash-if-needed>
```

#### Flow đăng nhập

```
┌─────────┐         ┌──────────────┐         ┌─────────────┐
│ Frontend │ ──────► │ Backend API  │ ──────► │   Cognito   │
│         │         │              │         │  User Pool  │
└─────────┘         └──────────────┘         └─────────────┘
     │                    │                       │
     │  1. POST /login   │                       │
     │  {email, pass}    │                       │
     │ ─────────────────►│                       │
     │                   │  2. InitiateAuth      │
     │                   │ ─────────────────────►│
     │                   │                       │
     │                   │  3. Tokens (JWT)     │
     │                   │ ◄─────────────────────│
     │                   │                       │
     │  4. Response      │                       │
     │  {accessToken,   │                       │
     │   idToken, ...}   │                       │
     │ ◄──────────────── │                       │
```

> **📸 [HÌNH 3]: Authentication Flow Diagram**

#### Resource Summary

| Resource | Giá trị | Mô tả |
|---------|---------|--------|
| User Pool | `medi-path-ease-users` | Pool cho patients/doctors |
| App Client | `medi-path-ease-web` | Web application client |
| Identity Pool | `medi-path-ease-identities` | AWS credentials pool |
| Auth Flow | USER_PASSWORD_AUTH | Email/password login |

> **📸 [HÌNH 4]: Cognito User Management Dashboard**

#### Tài liệu tham khảo

* [Amazon Cognito Documentation](https://docs.aws.amazon.com/cognito/)
* [AWS SDK Cognito Identity Provider](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/cognito-identity-provider/)
* [JWT Verification](https://docs.aws.amazon.com/cognito/latest/developerguide/amazon-cognito-user-pools-using-tokens-verifying-a-jwt.html)
