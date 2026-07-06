---
title: "Week 4 Worklog"
date: 2026-05-03
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
### Week 4 Objectives:
* Master the AWS Serverless model with Lambda, API Gateway, Step Functions.
* Build REST/HTTP API applications without managing servers.
* Connect serverless to storage (S3, DynamoDB) and message queue (SQS, SNS, EventBridge).
* Complete the first serverless application (API + Lambda + DynamoDB + Frontend).

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn AWS Lambda: <br>&emsp; + Function handler & event payload <br>&emsp; + Runtime: Python 3.12, Node.js 20, Java 21, Go, .NET <br>&emsp; + Cold start vs warm start <br>&emsp; + Concurrency limit & reserved concurrency <br>&emsp; + Timeout & memory size (128 MB - 10240 MB) <br>&emsp; + Lambda Layers to share libraries <br>&emsp; + Container image support <br> - **Practice:** Build a Lambda function that resizes an image from an S3 trigger and writes the result to another bucket | 05/04/2026 | 05/04/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn Amazon API Gateway: <br>&emsp; + REST API (resources & methods) vs HTTP API (lighter, cheaper) <br>&emsp; + Stages & Deployment <br>&emsp; + Mapping template (request/response) <br>&emsp; + CORS, Throttling, Usage Plan & API Key <br>&emsp; + Custom Domain Name <br>&emsp; + Lambda Authorizer & Cognito Authorizer <br> - **Practice:** Build a CRUD API (GET, POST, PUT, DELETE) connected to Lambda + DynamoDB and test via Postman                                                                                                                                              | 05/05/2026 | 05/05/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Amazon SQS & SNS: <br>&emsp; + SQS Standard vs FIFO Queue <br>&emsp; + Visibility Timeout & Long Polling <br>&emsp; + Dead Letter Queue <br>&emsp; + SNS Topic & Subscriptions (email, SMS, Lambda, SQS) <br>&emsp; + Fan-out pattern <br> - Learn Amazon EventBridge: <br>&emsp; + Event bus, rule, target <br>&emsp; + Scheduled job (cron) <br> - **Practice:** Build an order-processing workflow through SQS + SNS notification on new orders                                                                                                                              | 05/06/2026 | 05/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn AWS Step Functions: <br>&emsp; + State machine (Amazon States Language - ASL) <br>&emsp; + State types: Task, Choice, Parallel, Map, Wait, Pass, Fail, Succeed, Catch/Retry <br>&emsp; + Standard vs Express Workflows <br> - **Practice:** Design an order pipeline: receive order → validate → pay (simulated) → save DB → send confirmation email via SNS; with retry handling on failure                                                                                                                                                                          | 05/07/2026 | 05/08/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 4 capstone lab:** <br>&emsp; + REST API (API Gateway) → Lambda → DynamoDB <br>&emsp; + React SPA frontend calling the API, deployed via S3 + CloudFront <br>&emsp; + CloudWatch monitoring Lambda logs & metrics <br>&emsp; + Cognito Authorizer protecting the API <br>&emsp; + Wrap up & update the workshop <br> - Write a blog post: "Serverless 101 - Building a REST API on AWS"                                                                                                                                                  | 05/08/2026 | 05/08/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 4 Achievements:
* Mastered AWS Lambda:
  * Built functions reacting to S3 (ObjectCreated), API Gateway (proxy), and EventBridge (scheduled) events
  * Optimised the package by removing unused libraries → ~40% cold-start reduction
  * Used Lambda Layers to share common libraries (e.g. boto3, pandas) across functions
  * Configured reserved concurrency per function to protect downstream services
  * Knew when to use a Container Image (Lambda Image) instead of a ZIP package (e.g. 500 MB, custom ML model)

* Built a complete REST API:
  * Implemented CRUD via API Gateway (REST API) + Lambda
  * Configured CORS so frontends on a different domain can call it
  * Throttling (100 RPS), usage plans for Free vs Pro tiers
  * Connected API Gateway to DynamoDB via the Lambda's IAM Role
  * Knew when to choose HTTP API (70% cheaper, sufficient for simple use cases) vs REST API (richer features)
  * Configured a Custom Domain Name with an ACM certificate

* Understood and used messaging services:
  * Distinguished SQS (queue, pull) from SNS (pub/sub, push)
  * SQS FIFO Queue to guarantee processing order for strictly-ordered workloads
  * Dead Letter Queue for Lambda consumers when a message fails after N retries
  * Used EventBridge to schedule jobs (e.g. cron `0 2 * * ? *` runs at 2 AM daily)
  * Fan-out pattern: one SNS topic → many SQS subscribers for parallel processing

* Designed complex workflows with Step Functions:
  * State machines describing entire business pipelines as JSON ASL
  * Parallel State for concurrent tasks (e.g. send email + update inventory)
  * Map State for iterating over items in an array
  * Retry & Catch for automatic error handling, avoiding infinite retry loops
  * Visualised executions in the console, easy debugging with the execution history

* Completed the first serverless application:
  * React frontend on S3, delivered via CloudFront
  * Backend as a REST API (API Gateway + Lambda) calling DynamoDB
  * Cognito User Pool issuing JWT tokens to the frontend; Authorizer validates each request
  * CloudWatch dashboard tracking invocations, errors, throttles, and duration

* Wrote and shared a "Serverless 101" blog post on the workshop.
* New habits formed:
  * Every Lambda function has a clear retry policy (maxRetryAttempts: 3, maxEventAgeInSeconds: 3600)
  * Using X-Ray to trace requests across API Gateway → Lambda → DynamoDB
  * Naming convention for functions: `service-stage-function`, e.g. `order-prod-createOrder`
* ...