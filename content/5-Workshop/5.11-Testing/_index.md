---
title : "Testing"
date : 2024-01-01
weight : 11
chapter : false
pre : " <b> 5.11. </b> "
---

# 5.11. Testing

This section demonstrates the complete deployment of the TechMarket E-Commerce platform on AWS. It showcases the main user interfaces of the application and explains how each feature interacts with the deployed cloud infrastructure, including Amazon ECS Fargate, Amazon S3, MongoDB Atlas, Amazon Route 53, AWS Certificate Manager (ACM), Application Load Balancer, Amazon CloudWatch, Amazon ECR, and AWS CodeBuild.

---

# 1. Customer Website

## A. Homepage

The homepage is the primary interface where customers browse products available on the TechMarket platform.

### Infrastructure Integration

The Node.js Express application is deployed on Amazon ECS Fargate and exposed through an Application Load Balancer. Route 53 resolves the custom domain while ACM provides HTTPS encryption. Product information is retrieved from MongoDB Atlas and product images are loaded from Amazon S3.

### Data Flow

Customer visits **https://techmarketstore.store**

↓

Route 53 resolves the domain.

↓

Application Load Balancer forwards the request.

↓

Amazon ECS processes the request.

↓

MongoDB Atlas returns product information.

↓

Amazon S3 returns product images.

↓

Homepage is displayed.

<br>

![Homepage](/images/5-Workshop/5.11-Testing/homepage.png)

---

## B. User Registration

Customers can register a new account before accessing shopping features.

### Infrastructure Integration

Registration requests are processed by the Express application running on Amazon ECS. User information is validated before being stored in MongoDB Atlas. Passwords are encrypted before persistence.

![Register Type](/images/5-Workshop/5.11-Testing/register-type.jpg)

![Customer Register](/images/5-Workshop/5.11-Testing/customer-register.jpg)

---

## C. Shop Registration

Customers can register their own online store and become sellers on the platform.

### Infrastructure Integration

Shop registration requests are submitted to the Node.js backend running on Amazon ECS. Store information is saved in MongoDB Atlas and awaits administrator approval.

![Shop Register](/images/5-Workshop/5.11-Testing/shop-register.jpg)

---

## D. Login

Users authenticate using their email and password before accessing protected resources.

### Infrastructure Integration

Authentication is handled entirely by the Express application deployed on Amazon ECS. User credentials are verified against MongoDB Atlas, and authenticated sessions are managed using Express Session.

![Login](/images/5-Workshop/5.11-Testing/login.jpg)

---

## E. User Profile

The profile page allows customers to update personal information and upload an avatar.

### Infrastructure Integration

Profile information is retrieved from MongoDB Atlas. Avatar images are uploaded to Amazon S3, while image URLs are stored inside MongoDB Atlas.

![Profile](/images/5-Workshop/5.11-Testing/profile.jpg)

---

## F. Product Detail

The product detail page displays product specifications, descriptions, pricing, inventory status, and purchasing options.

### Infrastructure Integration

Product information is retrieved from MongoDB Atlas while product images are served directly from Amazon S3. All business logic is processed by the Node.js application running on Amazon ECS.

![Product Detail](/images/5-Workshop/5.11-Testing/product-detail.jpg)

---

## G. Payment Page

The payment page allows customers to review order information, customer information, payment method, and confirm the order.

### Infrastructure Integration

The payment request is handled by the Node.js Express application running on Amazon ECS. Order details are retrieved from MongoDB Atlas before the payment confirmation is processed.

![Payment Page](/images/5-Workshop/5.11-Testing/payment-page.jpg)

---

## H. Order Success

After payment confirmation, customers receive a successful order notification.

### Infrastructure Integration

The application records the completed order in MongoDB Atlas before returning the confirmation page.

![Order Success](/images/5-Workshop/5.11-Testing/order-success.jpg)

---
# 2. Shop Management

## A. Commission Statistics

Shop owners can monitor revenue and commission generated from completed orders.

### Infrastructure Integration

Revenue reports are calculated dynamically by the backend using data stored in MongoDB Atlas. The results are presented in a statistical dashboard.

![Commission Report](/images/5-Workshop/5.11-Testing/commission-report.jpg)

---

# 3. Administrator Dashboard

## A. Admin Dashboard

Administrators can monitor users, stores, and overall platform statistics.

### Infrastructure Integration

The administrator dashboard aggregates information directly from MongoDB Atlas through the Express application running on Amazon ECS.

Displayed information includes:

- Total Users
- Total Shops
- Shop Status
- User Information

![Admin Dashboard](/images/5-Workshop/5.11-Testing/admin-dashboard.jpg)

---

## B. Admin Order Management

Administrators manage customer orders, monitor order status, and update processing information.

### Infrastructure Integration

Order records are retrieved from MongoDB Atlas through the Node.js backend deployed on Amazon ECS. All order status updates are synchronized with the database immediately after execution.

![Admin Order Management](/images/5-Workshop/5.11-Testing/admin-order-management.jpg)

---

## C. Commission Statistics

Administrators can review platform-wide commission, monthly revenue, completed orders, and payment status of commission records.

### Infrastructure Integration

The Node.js backend aggregates commission data from MongoDB Atlas and returns statistical summaries to the administrator dashboard. The calculations are generated dynamically based on completed orders.

![Admin Commission Report](/images/5-Workshop/5.11-Testing/commission-report.jpg)

---

# AWS Services Demonstrated

During application testing, the deployed platform integrates the following AWS services:

| AWS Service | Purpose |
|-------------|---------|
| Amazon ECS Fargate | Hosts the Node.js Express application |
| Amazon ECR | Stores Docker container images |
| Amazon S3 | Stores product images and user avatars |
| MongoDB Atlas | Stores application data |
| Amazon Route 53 | Provides domain name resolution |
| AWS Certificate Manager | Enables HTTPS encryption |
| Application Load Balancer | Distributes incoming traffic |
| Amazon CloudWatch | Monitors application health |
| AWS CodeBuild | Automates build and deployment |

The successful execution of all interfaces demonstrates that the TechMarket application has been fully deployed and is operating correctly on AWS Cloud infrastructure.