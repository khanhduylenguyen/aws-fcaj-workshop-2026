---
title: "Demo - Real Application Interface"
date: 2026-07-20
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

#### Demo Overview

In this section, we will walk through the main interfaces of Medi Path Ease and how the AWS components integrate together.

> **📸 [IMAGE 1]: Medi Path Ease Homepage**

#### 1. Login / Register Page

**Function:** User authentication with Amazon Cognito

```
┌─────────────────────────────────────────┐
│           Medi Path Ease                │
│                                         │
│    [🔐 Logo]                           │
│                                         │
│    Welcome to Medi Path Ease            │
│    Connect doctors and patients         │
│                                         │
│    ┌─────────────────────────────┐     │
│    │ Email                        │     │
│    └─────────────────────────────┘     │
│    ┌─────────────────────────────┐     │
│    │ Password              👁️   │     │
│    └─────────────────────────────┘     │
│                                         │
│    [        Sign In        ]            │
│                                         │
│    Forgot password?                     │
│    ─────────────────────────────        │
│    Don't have an account?               │
│    [       Sign Up        ]            │
│                                         │
│    Sign in with:                        │
│    [G] [F] [Apple]                     │
└─────────────────────────────────────────┘
```

> **📸 [IMAGE 2]: Login Page with Cognito Authentication**

**AWS Services used:**
- **Amazon Cognito** - User Pool for authentication
- **Cognito JWT Tokens** - Access Token, ID Token, Refresh Token

> **📸 [IMAGE 3]: Cognito User Pool on AWS Console**

#### 2. Patient Dashboard

**Function:** Patient information overview

```
┌────────────────────────────────────────────────────────────────┐
│  [👤] Medi Path Ease           [🔔] [⚙️] [🚪]                │
├──────────┬─────────────────────────────────────────────────────┤
│          │                                                     │
│ Dashboard│  Good morning, John Doe                              │
│ ────────│                                                     │
│ 👤 Profile│  ┌──────────┐ ┌──────────┐ ┌──────────┐           │
│ 📅 Appts│  │Appoint-  │ │ Messages │ │Payments  │             │
│ 💬 Chat │  │ments 3   │ │    5     │ │ Pending  │             │
│ 📋 Labs │  └──────────┘ └──────────┘ └──────────┘             │
│ 💊 Rx   │                                                     │
│ 🤖 AI   │  Upcoming Appointments                               │
│ 💳 Pay  │  ┌─────────────────────────────────────────┐        │
│ ────────│  │ 📅 07/20/2026 - 09:00                   │        │
│ ⚙️ Settings│ Dr. Smith - General Medicine              │        │
│          │  └─────────────────────────────────────────┘        │
│          │  ┌─────────────────────────────────────────┐        │
│          │  │ 📅 07/22/2026 - 14:00                   │        │
│          │  │ Dr. Johnson - Dermatology               │        │
│          │  └─────────────────────────────────────────┘        │
│          │                                                     │
│          │  Recent Lab Results                                  │
│          │  ┌─────────────────────────────────────────┐        │
│          │  │ 📄 Blood Test - 07/18/2026             │        │
│          │  │    [View Details]                      │        │
│          │  └─────────────────────────────────────────┘        │
│          │                                                     │
└──────────┴─────────────────────────────────────────────────────┘
```

> **📸 [IMAGE 4]: Patient Dashboard**

**AWS Services used:**
- **Amazon RDS PostgreSQL** - Fetch appointments, records data
- **Amazon S3** - Display profile images

#### 3. Upload Lab Results

**Function:** Upload medical documents to S3

```
┌────────────────────────────────────────────────────────────────┐
│  Upload Lab Results                                            │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │                                                          │   │
│  │              📄                                          │   │
│  │                                                          │   │
│  │        Drag & drop file here                             │   │
│  │        or click to select                               │   │
│  │                                                          │   │
│  │        Formats: PDF, JPG, PNG                            │   │
│  │        Max size: 10MB                                    │   │
│  │                                                          │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  Selected file:                                                 │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ 📄 blood_test.pdf                               [✕]   │   │
│  │ ████████████████████░░░░░░░░░░░░░  65%               │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  Description (optional):                                       │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ Monthly blood test - July                              │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  [                    Upload                      ]            │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

> **📸 [IMAGE 5]: Upload Lab Results UI**

**AWS Services used:**
- **Amazon S3** - Store files
- **S3 Presigned URL** - Direct upload
- **Amazon CloudFront** - CDN for static files

> **📸 [IMAGE 6]: S3 Bucket with uploaded files**

#### 4. AI Chatbot Assistant

**Function:** Chat with medical AI assistant

```
┌────────────────────────────────────────────────────────────────┐
│  AI Medical Assistant                                    [─][×] │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ [🤖] Hello! I'm Medi Path Ease medical assistant.      │   │
│  │      How can I help you today?                        │   │
│  │      10:30 AM                                         │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │                                              [👤]     │   │
│  │ I have frequent headaches, what should I do? 10:31  │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ [🤖] Hello! Headaches can have many causes such as:    │   │
│  │      lack of sleep, stress, dehydration...            │   │
│  │                                                          │   │
│  │      💡 You should:                                    │   │
│  │      1. Sleep 7-8 hours each night                    │   │
│  │      2. Drink 2-3 liters of water/day                 │   │
│  │      3. Reduce stress, get enough rest                 │   │
│  │                                                          │   │
│  │      ⚠️ If headache persists > 3 days, I recommend   │   │
│  │         booking an appointment with a doctor!          │   │
│  │      10:31 AM                                         │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
├────────────────────────────────────────────────────────────────┤
│  [💬 Type your question...                        ] [Send]    │
│                                                                │
│  💡 Suggestions:                                               │
│  • Common COVID-19 symptoms                                    │
│  • Summer health care tips                                     │
│  • When to get vaccinated?                                     │
└────────────────────────────────────────────────────────────────┘
```

> **📸 [IMAGE 7]: AI Chatbot Interface**

**AWS Services used:**
- **Amazon Bedrock** - Claude 3.5 Sonnet
- **Bedrock Guardrails** - Content filtering

> **📸 [IMAGE 8]: Amazon Bedrock Playground - Test Claude**

#### 5. Payment

**Function:** Pay via PayOS

```
┌────────────────────────────────────────────────────────────────┐
│  Payment                                                       │
├────────────────────────────────────────────────────────────────┤
│                                                                │
│  Payment Details                                               │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ 🏥 Consultation Fee - Dr. Smith                       │   │
│  │ 📅 07/20/2026 - 09:00                                 │   │
│  ├────────────────────────────────────────────────────────┤   │
│  │                                                        │   │
│  │ Subtotal:                          $8.00 USD          │   │
│  │ Service Fee:                           $0.50 USD      │   │
│  │ ─────────────────────────────────────────────────     │   │
│  │ Total:                               $8.50 USD        │   │
│  │                                                        │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  Payment Method                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ ○ [💳] ATM/Visa/MasterCard                             │   │
│  │ ○ [📱] VNPay QR Code                                   │   │
│  │ ○ [📱] E-wallet (Momo, ZaloPay...)                    │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  ┌────────────────────────────────────────────────────────┐   │
│  │ [           Pay Now - $8.50 USD                ]       │   │
│  └────────────────────────────────────────────────────────┘   │
│                                                                │
│  🔒 Secure payment via PayOS                                   │
│                                                                │
└────────────────────────────────────────────────────────────────┘
```

> **📸 [IMAGE 9]: Payment Page with PayOS Integration**

#### 6. Admin Dashboard

**Function:** System management

> **📸 [IMAGE 10]: Admin Dashboard**

**AWS Services used:**
- **Amazon RDS PostgreSQL** - User data
- **Amazon CloudWatch** - Monitoring

#### Required Images Summary

| No. | Image Description | Filename |
|-----|-------------------|----------|
| 1 | Medi Path Ease Homepage | `demo-01-homepage.png` |
| 2 | Login Page with Cognito | `demo-02-login.png` |
| 3 | Cognito User Pool Console | `demo-03-cognito-pool.png` |
| 4 | Patient Dashboard | `demo-04-patient-dashboard.png` |
| 5 | Upload Lab Results UI | `demo-05-upload-ui.png` |
| 6 | S3 Bucket with files | `demo-06-s3-bucket.png` |
| 7 | AI Chatbot Interface | `demo-07-chatbot.png` |
| 8 | Bedrock Playground | `demo-08-bedrock-playground.png` |
| 9 | Payment Page | `demo-09-payment.png` |
| 10 | Admin Dashboard | `demo-10-admin-dashboard.png` |
