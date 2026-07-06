---
title: "Week 6 Worklog"
date: 2026-05-17
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
### Week 6 Objectives:
* Master Containers with Docker: image, container, Dockerfile, Docker Compose.
* Master Amazon ECR, ECS (Fargate & EC2) and explore basic Kubernetes with EKS.
* Deploy a microservice on ECS Fargate behind an ALB with Auto Scaling and Blue/Green deployment.
* Write a blog post sharing container experience on AWS.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                  | Start Date | Completion Date | Reference Material                        |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn basic Docker: <br>&emsp; + Concepts: Image vs Container <br>&emsp; + Dockerfile & commands (FROM, RUN, COPY, CMD, ENTRYPOINT, EXPOSE) <br>&emsp; + Multi-stage build <br>&emsp; + Layer & caching <br>&emsp; + Docker Compose (multi-container) <br>&emsp; + Volume, Network, Bind Mount <br> - **Practice:** Containerise Node.js (Express) and Python (FastAPI) apps and push to Docker Hub | 05/18/2026 | 05/18/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn Amazon ECR: <br>&emsp; + Push & pull images via Docker CLI <br>&emsp; + Lifecycle policy (delete old images) <br>&emsp; + Image scanning (CVE) <br>&emsp; + Cross-region replication <br> - Learn Amazon ECS: <br>&emsp; + Cluster, Service, Task Definition <br>&emsp; + Capacity provider: Fargate vs EC2 <br>&emsp; + Service Auto Scaling <br>&emsp; + Task IAM Role <br>&emsp; + Container Insights (CloudWatch) | 05/19/2026 | 05/19/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - **Practice:** Deploy a microservice on ECS Fargate: <br>&emsp; + Create Task Definition for Node.js web (CPU 256, Memory 512) <br>&emsp; + Create Service connected to ALB <br>&emsp; + Configure Auto Scaling by CPU <br>&emsp; + Update image via CodeDeploy (Blue/Green) <br>&emsp; + Enable Container Insights for monitoring                                                                                                                                                       | 05/20/2026 | 05/20/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic Kubernetes: <br>&emsp; + Architecture: Control Plane, Worker Node, kubelet <br>&emsp; + Pod, Deployment, Service, ReplicaSet <br>&emsp; + Namespace, ConfigMap, Secret <br>&emsp; + Basic kubectl: get, describe, logs, exec <br>&emsp; + YAML manifests <br> - Learn Amazon EKS: <br>&emsp; + Managed Node Group <br>&emsp; + Fargate profile <br>&emsp; + EKS Add-ons (VPC CNI, kube-proxy, ...)                                                                  | 05/21/2026 | 05/22/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 6 capstone lab:** <br>&emsp; + Containerise web app (React frontend) + database (Postgres) <br>&emsp; + Push images to ECR <br>&emsp; + Deploy web on ECS Fargate with ALB <br>&emsp; + Database runs on RDS in private subnet <br>&emsp; + Update workshop & write blog "Container & ECS Fargate 101"                                                                                                                                                                | 05/22/2026 | 05/22/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 6 Achievements:
* Mastered Docker:
  * Wrote multi-stage Dockerfile reducing final image size by ~70%
  * Used `docker-compose.yml` for dev environments (web + db + redis)
  * Understood layers, cache, and image registries
  * Debugged containers with `docker logs`, `docker exec -it <container> sh`
  * Pushed images to Docker Hub and ECR

* Mastered Amazon ECS:
  * Designed proper Task Definitions: container definitions, CPU, memory, env, port mapping
  * Deployed Services on Fargate (serverless containers, no EC2 to manage)
  * Configured ALB + Auto Scaling for the Service (target tracking CPU 70%)
  * Blue/Green deployment via CodeDeploy: shift 50% traffic to new version, then full cutover
  * Container Insights on CloudWatch monitoring CPU, memory, and network per container

* Explored Kubernetes & EKS:
  * Understood Pod, Deployment, Service, Namespace
  * Wrote YAML manifests to deploy apps (`kubectl apply -f deployment.yaml`)
  * Created an EKS Cluster with a Managed Node Group and Fargate Profile
  * Used `eksctl` and the AWS Console to manage the cluster
  * Brief overview of Helm charts for packaging manifests

* Completed the container + ECS Fargate + ALB capstone lab:
  * Docker Compose locally for development
  * Push images to ECR from GitHub Actions (combining with Week 5)
  * Deployed to ECS Fargate via CodePipeline
  * RDS lives in a private subnet, only the ECS Service can access it via Security Group

* Compared and chose between deployment options:
  * **ECS Fargate:** simple, deeply integrated with AWS, no cluster management
  * **ECS EC2:** cheaper for steady workloads but you manage the EC2s
  * **EKS:** needed when you want the rich Kubernetes ecosystem or multi-cloud, but more complex

* Published a sharing blog post on Docker + ECS experience on the workshop.
* New habits formed:
  * Always have a `.dockerignore` to avoid copying `node_modules`, `.git`, `.env` into the image
  * Scan images with `trivy` or ECR scan before deploy
  * Use a shared base image to save layer storage
  * Tag images by commit SHA; never use `latest` in production
* ...