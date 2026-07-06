---
title: "Week 3 Worklog"
date: 2026-04-26
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
### Week 3 Objectives:
* Master AWS Database services: RDS (relational) and DynamoDB (NoSQL).
* Set up monitoring & logging with Amazon CloudWatch and CloudTrail.
* Build highly available infrastructure (HA) with ALB + Auto Scaling Group across multiple AZs.
* Complete a full 3-tier lab (Web + ALB + ASG + RDS).

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                    | Start Date | Completion Date | Reference Material                        |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn Relational Database Service (RDS): <br>&emsp; + Supports MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Aurora <br>&emsp; + DB Instance class & Storage type <br>&emsp; + Multi-AZ Deployment (synchronous replication) <br>&emsp; + Read Replica (asynchronous replication) <br>&emsp; + Parameter Group & Option Group <br>&emsp; + Automated Backup & Point-in-Time Recovery <br>&emsp; + RDS Proxy <br> - **Practice:** Create an RDS MySQL instance, connect from EC2, create a sample schema & table | 04/27/2026 | 04/27/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn Amazon DynamoDB: <br>&emsp; + NoSQL key-value & document <br>&emsp; + Partition Key & Sort Key <br>&emsp; + Secondary Index (GSI / LSI) <br>&emsp; + Capacity mode (On-demand / Provisioned) <br>&emsp; + Auto Scaling & TTL <br>&emsp; + Global Tables (multi-region) <br>&emsp; + DAX (in-memory cache) <br> - **Practice:** Create a DynamoDB table and perform CRUD via AWS CLI & Python SDK (boto3)                                                                                                                                  | 04/28/2026 | 04/28/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Amazon CloudWatch: <br>&emsp; + Metrics & Namespaces (CPU, NetworkIn, StatusCheckFailed, ...) <br>&emsp; + Alarms & Actions (Auto Scaling, SNS) <br>&emsp; + Logs & Log Groups (CloudWatch Agent) <br>&emsp; + Dashboards <br>&emsp; + CloudWatch Logs Insights (log queries) <br> - Learn AWS CloudTrail for API call auditing <br> - **Practice:** Install CloudWatch Agent on EC2, ship logs to CloudWatch Logs, create a CPU > 70% alarm that notifies via SNS | 04/29/2026 | 04/29/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn High Availability & Load Balancing: <br>&emsp; + ELB: ALB (Layer 7) vs NLB (Layer 4) vs GWLB (Gateway) <br>&emsp; + Target Group & Health Check <br>&emsp; + Auto Scaling Group (min/desired/max) <br>&emsp; + Launch Template vs Launch Configuration <br>&emsp; + Scaling policies: target tracking, step, simple <br> - **Practice:** Deploy an ASG + ALB for a web app across multiple AZs and observe the ASG recreating lost instances                                                                                                                               | 04/30/2026 | 05/01/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 3 capstone lab:** <br>&emsp; + Web app on EC2 (Auto Scaling) behind ALB <br>&emsp; + Backend stores data in RDS MySQL inside private subnets <br>&emsp; + CloudWatch Alarm when CPU > 70% triggers scale out + SNS notification <br>&emsp; + Wrap up & commit code to GitHub <br>&emsp; + Write a PDF report for the lab with architecture diagram                                                                                                                                                                          | 05/01/2026 | 05/01/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 3 Achievements:
* Deployed and operated Amazon RDS:
  * Created Multi-AZ RDS MySQL; the standby replica in another AZ automatically fails over on primary failure
  * Configured a Security Group permitting EC2 access on port 3306 only
  * Created a Read Replica and tested read load with sysbench
  * Enabled Automated Backup with a 7-day retention, enabling Point-in-Time Recovery
  * Tuned `max_connections` and `slow_query_log` via a Parameter Group

* Mastered DynamoDB for NoSQL workloads:
  * Designed proper Partition Key / Sort Key (e.g. `PK = USER#123`, `SK = ORDER#2026-04-28`)
  * Performed CRUD via AWS CLI and boto3
  * Understood the difference between On-demand (pay per request) and Provisioned (fixed RCU/WCU)
  * Enabled TTL to auto-delete sessions after N days
  * Used GSIs to query by non-key attributes
  * Understood pricing units: Read Capacity Unit (1 strongly consistent 4 KB read), Write Capacity Unit (1 KB write)

* Configured CloudWatch:
  * Created custom EC2 metrics (RAM, disk) via CloudWatch Agent
  * Set up Alarms triggering actions (scale out / SNS email)
  * Built dashboards to monitor CPU, network, and 5xx errors
  * Enabled CloudTrail in all regions to audit every change in the account
  * Used Logs Insights to query: `fields @timestamp, @message | filter @message like /ERROR/`

* Built highly available infrastructure:
  * Auto Scaling Group scaling on CPU (target tracking 60%)
  * Application Load Balancer distributing traffic across AZs using round-robin
  * Health checks every 30s removing unhealthy instances from rotation
  * A Launch Template reused for instance configuration (AMI, security group, user data)
  * A 60s warm-up time to avoid pounding freshly launched instances

* Completed the 3-tier lab (Web + ALB + ASG + RDS) and wrote a clear architecture-diagram report on the workshop.
* Comfortable designing a "defense in depth" architecture:
  * Multi-AZ for database and application
  * Separate Security Groups per tier (web, app, db)
  * CloudWatch monitoring for every layer
* New habits formed:
  * Every infra change is tagged `Environment: dev/staging/prod` for cost tracking
  * Every RDS is encrypted at rest with KMS and has deletion protection enabled
  * Each CloudWatch alarm has a runbook: who receives it, how to handle it
* ...