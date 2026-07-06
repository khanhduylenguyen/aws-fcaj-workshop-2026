---
title: "Week 11 Worklog"
date: 2026-06-21
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
### Week 11 Objectives:
* Build & demo a Capstone project integrating everything learned in Cloud + AI/ML.
* Coordinate with mentors/team for review and revision of the product.
* Complete documentation for the final project (README, API doc, Architecture diagrams).

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                       | Start Date | Completion Date | Reference Material                        |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Review & finalize the Capstone idea with mentor <br> - Plan in detail: scope, timeline, architecture, tech stack <br> - Create the GitHub repo for the project, assign issues if working in a group <br> - Analyse target users & primary use cases <br> - Define success KPIs (latency < 2s, accuracy > 85%, cost < $X)                                                                                                                                  | 06/22/2026 | 06/22/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Build backend & infrastructure on AWS: <br>&emsp; + CloudFormation deploys VPC (2 AZ) + ECS Fargate + RDS + S3 + CloudWatch <br>&emsp; + API Gateway + Lambda for AI services <br>&emsp; + React frontend built & deployed via S3 + CloudFront <br>&emsp; + Cognito User Pool for authentication <br>&emsp; + Set up CI/CD with GitHub Actions                                                                                                                                                  | 06/23/2026 | 06/23/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Integrate Bedrock (RAG + Agent) into the product <br> - Set up CI/CD with GitHub Actions & CodePipeline (build → ECR → ECS / CloudFront) <br> - Add CloudWatch Alarms + SNS for critical services <br> - Build a system-wide monitoring dashboard <br> - Write README & architecture diagram                                                                                                                                                                                          | 06/24/2026 | 06/24/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **End-to-end practice:** <br>&emsp; + Test the full business flow: sign up → upload document → Q&A → export report <br>&emsp; + Write test cases for Lambdas & APIs (pytest, jest) <br>&emsp; + Test on multiple devices (mobile, tablet, desktop) <br>&emsp; + Security review: IAM, KMS, secret rotation, Bedrock Guardrails                                                                                                                                                             | 06/25/2026 | 06/26/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Internal demo for mentor & team <br> - Collect feedback, plan improvements for the final week <br> - Write the Capstone progress report on the workshop <br> - Update portfolio & CV with the Capstone project & AWS certifications earned                                                                                                                                                                                                                                          | 06/26/2026 | 06/26/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 11 Achievements:
* Finalised the Capstone topic and detailed plan:
  * Clear scope, achievable within the remaining time
  * Architecture showcasing all AWS services learned
  * Defined KPIs: latency, accuracy, cost

* Successfully deployed infrastructure & application:
  * CloudFormation auto-deploying the environment (VPC, subnets, NAT, ALB, ECS, RDS, S3)
  * ECS Fargate running the backend container; CloudFront + S3 hosting the React frontend
  * API Gateway + Lambda connecting to Bedrock (RAG + Agent)
  * Cognito User Pool for multi-user authentication
  * CloudWatch dashboard monitoring the whole system

* Integrated Generative AI:
  * Bedrock Knowledge Base querying business documents
  * Bedrock Agent handling multi-step tasks (invoking Lambda actions)
  * Guardrails protecting outputs (deny topics, PII redaction)
  * Embeddings via Titan Embeddings v2 for VI + EN

* Completed CI/CD:
  * GitHub Actions lint/test/build on every PR
  * CodePipeline auto-deploying to ECS / CloudFront
  * Automatic rollback on failed deploys (CloudWatch alarm trigger)

* Secured & operated:
  * IAM Roles instead of access keys for every service
  * Secrets stored in Secrets Manager, with auto rotation
  * KMS encrypting all data at rest
  * Guardrails for Bedrock, WAF for the ALB

* Tested:
  * Unit tests for Lambdas (pytest, coverage > 80%)
  * Integration tests for end-to-end API flows
  * Manual testing on Chrome, Firefox, Safari, iOS Safari
  * Security tests: XSS, SQL injection, prompt injection attempts

* Internal demo successful:
  * Presented to mentors and FCAJ peers
  * Received feedback on UX (upload flow), security (add MFA), and performance (cache response)

* Updated CV and portfolio with the Capstone project.
* New habits formed:
  * Write unit tests in parallel with features (TDD)
  * Every PR has at least one reviewer; nobody self-merges
  * Document every significant architectural decision in an ADR (Architecture Decision Record)
  * Runbook for every service in production
* ...