---
title : "Configure Network"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

## Configure Network

After creating the Virtual Private Cloud (VPC), the next step is to configure the networking components required for the application.

In this section, you will create public and private subnets, configure an Internet Gateway, a NAT Gateway, route tables, and security groups. These components provide secure communication between the internet, the Application Load Balancer, Amazon ECS, and other AWS services.

---

## Create Public and Private Subnets

Navigate to:

**AWS Console → VPC → Subnets → Create subnet**

Create four subnets using the following configuration:

| Name | Availability Zone | IPv4 CIDR |
|------|-------------------|------------|
| public-subnet-a | ap-southeast-1a | 10.0.1.0/24 |
| public-subnet-b | ap-southeast-1b | 10.0.2.0/24 |
| private-subnet-a | ap-southeast-1a | 10.0.3.0/24 |
| private-subnet-b | ap-southeast-1b | 10.0.4.0/24 |

Enable **Auto-assign public IPv4 address** for both public subnets.

After creating the subnets, verify that all four subnets are available.

![Subnets](/images/5-Workshop/5.4-Networking/subnets.png)

---

## Configure Internet Gateway

Navigate to:

**AWS Console → VPC → Internet Gateways → Create internet gateway**

Configure the Internet Gateway using the following settings:

| Property | Value |
|----------|-------|
| Name | production-igw |

After creating the Internet Gateway:

- Select **Attach to VPC**
- Choose **production-vpc**

Verify that the Internet Gateway status is **Attached**.

![Internet Gateway](/images/5-Workshop/5.4-Networking/internet-gateway.png)

---

## Configure NAT Gateway

Navigate to:

**AWS Console → VPC → NAT Gateways → Create NAT gateway**

Use the following configuration:

| Property | Value |
|----------|-------|
| Name | production-nat |
| Subnet | public-subnet-a |
| Connectivity type | Public |
| Elastic IP | Allocate Elastic IP |

Wait until the NAT Gateway status changes to **Available** before proceeding.

![NAT Gateway](/images/5-Workshop/5.4-Networking/nat-gateway.png)

---

## Configure Route Tables

Navigate to:

**AWS Console → VPC → Route Tables**

Create two route tables:

| Route Table | Associated Subnets | Default Route |
|-------------|--------------------|---------------|
| public-rt | public-subnet-a, public-subnet-b | Internet Gateway |
| private-rt | private-subnet-a, private-subnet-b | NAT Gateway |

Configure the routes as follows.

### Public Route Table

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | Internet Gateway |

### Private Route Table

| Destination | Target |
|-------------|--------|
| 0.0.0.0/0 | NAT Gateway |

Associate each route table with the corresponding subnets.

![Route Tables](/images/5-Workshop/5.4-Networking/route-tables.png)

---

## Configure Security Groups

Navigate to:

**AWS Console → EC2 → Security Groups**

Create two security groups.

### Application Load Balancer Security Group

| Property | Value |
|----------|-------|
| Name | production-alb-sg |
| VPC | production-vpc |

Configure the inbound rules:

| Type | Port | Source |
|------|------|---------|
| HTTP | 80 | 0.0.0.0/0 |
| HTTPS | 443 | 0.0.0.0/0 |

Configure the outbound rules:

| Type | Destination |
|------|-------------|
| All Traffic | 0.0.0.0/0 |

---

### Amazon ECS Security Group

| Property | Value |
|----------|-------|
| Name | production-ecs-sg |
| VPC | production-vpc |

Configure the inbound rules:

| Type | Port | Source |
|------|------|---------|
| Custom TCP | 3000 | production-alb-sg |

Configure the outbound rules:

| Type | Destination |
|------|-------------|
| All Traffic | 0.0.0.0/0 |

After completing the configuration, verify that both security groups have been created successfully.

![Security Groups](/images/5-Workshop/5.4-Networking/security-groups.png)

---

## Expected Result

After completing this section, you will have:

- Two public subnets and two private subnets created.
- An Internet Gateway attached to the VPC.
- A NAT Gateway in the **Available** state.
- Public and private route tables configured correctly.
- Security Groups configured for the Application Load Balancer and Amazon ECS.
- A networking environment ready for deploying the application in the following chapters.