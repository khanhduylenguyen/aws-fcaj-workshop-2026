---
title : "Create VPC"
date : 2026-01-01
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

## Create VPC

In this section, you will create a Virtual Private Cloud (VPC), which provides an isolated networking environment for deploying the application on AWS.

The VPC serves as the foundation of the entire infrastructure. All networking resources, including subnets, route tables, Internet Gateway, NAT Gateway, Application Load Balancer, and Amazon ECS, will be deployed inside this VPC.

---

## Create a Virtual Private Cloud

Navigate to:

**AWS Console → VPC → Your VPCs → Create VPC**

Select **VPC only**, then configure the following settings:

| Property | Value |
|----------|-------|
| Resources to create | VPC only |
| Name tag | production-vpc |
| IPv4 CIDR | 10.0.0.0/16 |
| IPv6 CIDR | None |
| Tenancy | Default |

Review the configuration and choose **Create VPC**.

![Create VPC](/images/5-Workshop/5.4-Networking/create-vpc.png)

---

## Verify the VPC

Navigate to:

**AWS Console → VPC → Your VPCs**

Select **production-vpc** and verify the following settings:

| Property | Expected Value |
|----------|----------------|
| State | Available |
| IPv4 CIDR | 10.0.0.0/16 |
| DNS Resolution | Enabled |
| DNS Hostnames | Enabled |

Confirm that the VPC has been created successfully before proceeding to the networking configuration.

![VPC Details](/images/5-Workshop/5.4-Networking/vpc-details.png)

---

## Expected Result

After completing this section, you will have:

- A Virtual Private Cloud named **production-vpc**.
- A private networking environment with the IPv4 CIDR block **10.0.0.0/16**.
- DNS Resolution and DNS Hostnames enabled.
- A VPC ready for configuring subnets and networking resources.