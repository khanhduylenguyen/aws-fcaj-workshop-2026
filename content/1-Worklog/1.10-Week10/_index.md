---
title: "Week 10 Worklog"
date: 2026-06-14
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
### Week 10 Objectives:
* Strengthen Security & Identity knowledge in the AWS ecosystem.
* Explore cost optimization strategies with AWS Budgets, Cost Explorer, and Savings Plans.
* Audit & secure the personal workshop hosted on AWS, write an actionable checklist.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                  | Start Date | Completion Date | Reference Material                        |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Review AWS Shared Responsibility Model <br> - Learn AWS Organizations & AWS Control Tower: <br>&emsp; + Multi-account architecture (dev, staging, prod) <br>&emsp; + Service Control Policies (SCPs) blocking regions/services <br>&emsp; + Organizational Units (OU) per team/project <br>&emsp; + AWS RAM sharing resources across accounts                                                                                                                                          | 06/15/2026 | 06/15/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn AWS IAM Identity Center (SSO): <br>&emsp; + Permission Sets <br>&emsp; + Account assignment <br>&emsp; + Integration with Azure AD / Okta <br> - Learn AWS Cognito: <br>&emsp; + User Pool (authentication) <br>&emsp; + Identity Pool (authorization) <br>&emsp; + JWT token & refresh token <br>&emsp; + Hosted UI <br> - **Practice:** Protect API Gateway with Cognito Authorizer; user pool for React web app                                                                                                                          | 06/16/2026 | 06/16/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn AWS KMS & Secrets Manager: <br>&emsp; + Customer Managed Key (CMK) <br>&emsp; + AWS Managed Key <br>&emsp; + Encryption at-rest & in-transit <br>&emsp; + Key Rotation <br>&emsp; + Secrets Manager auto secret rotation (RDS password) <br>&emsp; + Cross-account access <br> - **Practice:** Encrypt EBS, RDS, S3 with KMS, store DB password in Secrets Manager, retrieve in Lambda                                                                                                                  | 06/17/2026 | 06/17/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn AWS Cost Explorer & Budgets: <br>&emsp; + Cost & Usage Report (CUR) <br>&emsp; + Reserved Instances & Savings Plans <br>&emsp; + Cost anomaly detection <br>&emsp; + Tagging policy <br>&emsp; + Spot Instances for flexible workloads <br> - **Practice:** Set budget alerts (e.g. $20/month), analyse cost by service/tag via Cost Explorer, enable anomaly detection                                                                                                                              | 06/18/2026 | 06/19/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Personal workshop audit:** <br>&emsp; + Remove unused resources (EC2, EBS, Elastic IP) <br>&emsp; + Use tags to track costs (Environment, Project) <br>&emsp; + Stop EC2 outside working hours (Lambda + EventBridge) <br>&emsp; + Audit and delete unused old IAM access keys <br>&emsp; + Enable CloudTrail in used regions <br> - Write security & cost checklist on the workshop                                                                                                                                                              | 06/19/2026 | 06/19/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 10 Achievements:
* Mastered the AWS security model:
  * Clearly understood AWS's "security OF the cloud" responsibility (hardware, hypervisor, region)
  * Customer's "security IN the cloud" responsibility (OS, app, data, IAM)
  * Applied them to the services in use

* Managed multi-account with Organizations:
  * Created separate accounts (dev / staging / prod) to isolate environments
  * Attached SCPs to block unused regions (only allow `ap-southeast-1`, `us-east-1`)
  * Organised OUs by team/project, easy to apply policies at scale
  * AWS RAM sharing VPC subnets and Transit Gateway across accounts

* Configured SSO & access management:
  * IAM Identity Center for single sign-on across many accounts
  * Permission Sets assigning roles (Admin, Developer, Read-only)
  * Cognito User Pool provides authentication for web/mobile apps
  * Cognito Identity Pool issues temporary AWS credentials to users
  * API Gateway + Cognito Authorizer protecting REST APIs

* Secured data:
  * Encrypted EBS, RDS, S3 with KMS CMKs
  * S3 default encryption (SSE-S3 or SSE-KMS)
  * RDS encryption at rest + in transit (Force SSL)
  * Secrets Manager auto-rotating passwords (Lambda rotate function)
  * Retrieved secrets from Lambda without hard-coding
  * Key policy: only the necessary services can use the key

* Optimised costs:
  * Detected idle resources (EC2, EBS, Elastic IP) via Cost Explorer
  * Set Budget alarms when exceeding $20/month
  * Enabled Cost Anomaly Detection to get alerts on cost spikes
  * Analysed cost by service, tag, and region
  * Applied Savings Plans for steady workloads (EC2, Lambda)
  * Used Spot Instances for flexible workloads (dev, batch)

* Applied best practices:
  * Stop EC2 when not in use via Lambda + EventBridge (schedule)
  * Auto delete EBS volumes when EC2 is terminated (`DeleteOnTermination: true`)
  * Release Elastic IPs not attached to any instance
  * Remove access keys older than 90 days
  * Enable MFA for every user with admin permissions

* Completed the personal workshop audit and published the security & cost optimization checklist on GitHub.
* New habits formed:
  * Every resource is tagged with a standard set: Project, Environment, Owner, CostCenter
  * Weekly 30-minute review of Cost Explorer
  * Never share access keys via Slack/email; use roles + OIDC instead
  * Default Block Public Access is enabled on every new S3 bucket
* ...