---
title : "Deploy Application to Amazon ECS"
date : 2026-01-01
weight : 2
chapter : false
pre : " <b> 5.7.2. </b> "
---

## Deploy Application to Amazon ECS

In this section, you will deploy the Second-Hand Marketplace application to Amazon ECS using AWS Fargate.

The deployment process includes creating a task definition, configuring an ECS service, and associating the service with the existing Application Load Balancer.

---

## Create a Task Definition

Navigate to:

**AWS Console → Amazon ECS → Task definitions → Create new task definition**

Configure the task definition using the following settings.

| Property | Value |
|----------|-------|
| Launch type | AWS Fargate |
| Task definition family | production-task |
| Operating system | Linux |
| CPU | 1 vCPU |
| Memory | 2 GB |

Choose **Next** to configure the container.

![Task Definition](/images/5-Workshop/5.7-Deploy-Application/task-definition.png)

---

## Configure the Container

Configure the container using the Docker image stored in Amazon ECR.

| Property | Value |
|----------|-------|
| Container name | wed-mbdc |
| Image URI | Amazon ECR Image |
| Container port | 3000 |

Configure the required environment variables and secrets, then create the task definition.

![Container Configuration](/images/5-Workshop/5.7-Deploy-Application/container-configuration.png)

---

## Create an ECS Service

Navigate to:

**Amazon ECS → Clusters → production-cluster → Create**

Configure the service using the following settings.

| Property | Value |
|----------|-------|
| Launch type | AWS Fargate |
| Task definition | production-task |
| Service name | production-service |
| Desired tasks | 1 |

Continue to the networking configuration.

![Create Service](/images/5-Workshop/5.7-Deploy-Application/create-service.png)

---

## Configure Networking

Configure the ECS service networking.

| Property | Value |
|----------|-------|
| VPC | production-vpc |
| Subnets | Private Subnets |
| Security Group | production-ecs-sg |
| Public IP | Disabled |

For the Load Balancer section:

- Select **Use existing load balancer**.
- Choose the Application Load Balancer created in the previous section.
- Select the existing Target Group.

Review the configuration and choose **Create**.

![Networking Configuration](/images/5-Workshop/5.7-Deploy-Application/networking.png)

---

## Verify the Deployment

Navigate to:

**Amazon ECS → Clusters → production-cluster → Services**

Verify that:

- Service status is **Active**.
- Running tasks equal the desired tasks.
- The task status is **Running**.

![Service Running](/images/5-Workshop/5.7-Deploy-Application/service-running.png)

---

## Expected Result

After completing this section, you will have:

- A Task Definition created.
- An ECS Service deployed successfully.
- The application running on AWS Fargate.
- Amazon ECS integrated with the existing Application Load Balancer.