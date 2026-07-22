---
title : "Configure Route 53 and AWS Certificate Manager"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.8.1. </b> "
---

## Configure Route 53 and AWS Certificate Manager

In this section, you will configure Amazon Route 53 and AWS Certificate Manager (ACM) to enable secure HTTPS access for the Second-Hand Marketplace application.

AWS Certificate Manager issues the SSL/TLS certificate, while Amazon Route 53 manages the domain name and routes user requests to the Application Load Balancer.

---

## Request an SSL/TLS Certificate

Navigate to:

**AWS Console → AWS Certificate Manager → Request**

Choose **Request a public certificate**.

Configure the certificate using the following settings.

| Property | Value |
|----------|-------|
| Domain name | your-domain.com |
| Validation method | DNS validation |
| Key algorithm | RSA 2048 |

Choose **Request**.

![Request Certificate](/images/5-Workshop/5.8-Domain-and-HTTPS/request-certificate.png)

---

## Validate the Certificate

After requesting the certificate, ACM provides a DNS validation record.

If your domain is hosted in Amazon Route 53, choose **Create records in Route 53**.

Wait until the certificate status changes to **Issued**.

![Certificate Issued](/images/5-Workshop/5.8-Domain-and-HTTPS/certificate-issued.png)

---

## Configure the Domain in Amazon Route 53

Navigate to:

**AWS Console → Route 53 → Hosted Zones**

Open the hosted zone for your domain and create an **A – Alias** record.

Configure the record as follows.

| Property | Value |
|----------|-------|
| Record type | A |
| Alias | Yes |
| Route traffic to | Application Load Balancer |

Save the record.

![Hosted Zone](/images/5-Workshop/5.8-Domain-and-HTTPS/hosted-zone.png)

---

## Configure HTTPS on the Application Load Balancer

Navigate to:

**AWS Console → EC2 → Load Balancers**

Open the Application Load Balancer and add an HTTPS listener.

Configure the listener using:

| Property | Value |
|----------|-------|
| Protocol | HTTPS |
| Port | 443 |
| Certificate | ACM Certificate |
| Default action | Forward to Target Group |

Save the configuration.

![HTTPS Listener](/images/5-Workshop/5.8-Domain-and-HTTPS/https-listener.png)

---

## Verify HTTPS Access

Open a web browser and access the application using the configured domain.

Verify that:

- The application is accessible.
- HTTPS is enabled.
- The browser displays a secure connection.

![HTTPS Website](/images/5-Workshop/5.8-Domain-and-HTTPS/https-website.png)

---

## Expected Result

After completing this section, you will have:

- An SSL/TLS certificate issued by AWS Certificate Manager.
- A custom domain configured in Amazon Route 53.
- HTTPS enabled on the Application Load Balancer.
- Secure access to the deployed application.