---
title : "Networking"
date : 2026-01-01
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

### Goal

Build the networking infrastructure required to deploy the Second-Hand Marketplace application securely on AWS.

---

## 1. Overview

Networking is the foundation of the cloud infrastructure. In this chapter, you will create a Virtual Private Cloud (VPC) and configure the networking components required to support the application deployment.

The network architecture includes public and private subnets, an Internet Gateway, a NAT Gateway, route tables, and security groups. These components provide secure communication between the Application Load Balancer, Amazon ECS, AWS services, and external resources such as MongoDB Atlas.

---

---

## 2. Detailed Practice Content

Complete the following sections in order:

- **5.4.1 Create VPC**
- **5.4.2 Configure Network**

---

## 3. Expected Result

After completing this chapter, you will have:

- A Virtual Private Cloud (VPC) created.
- Public and private subnets configured.
- Internet Gateway and NAT Gateway configured.
- Route tables configured correctly.
- Security Groups configured for the Application Load Balancer and Amazon ECS.
- A networking environment ready for application deployment.