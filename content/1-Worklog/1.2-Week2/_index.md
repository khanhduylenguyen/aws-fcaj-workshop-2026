---
title: "Week 2 Worklog"
date: 2026-04-19
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
### Week 2 Objectives:
* Master Amazon S3: bucket, object, storage class, versioning, static website hosting.
* Master AWS IAM concepts: User, Group, Role, Policy and how to apply the least-privilege principle.
* Understand the basic VPC model: CIDR, Subnet, Route Table, Internet Gateway, NAT Gateway.
* Complete a combined lab of EC2 + S3 + IAM and host a static website on S3.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Start Date | Completion Date | Reference Material                        |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn Amazon S3: <br>&emsp; + Concepts: Bucket (global unique name) & Object (key + value + metadata) <br>&emsp; + Storage class: S3 Standard, IA, One Zone-IA, Glacier, Glacier Deep Archive <br>&emsp; + Versioning <br>&emsp; + Lifecycle rule <br>&emsp; + Cross-Region Replication <br>&emsp; + Object Lock & WORM <br> - **Practice:** Create the first bucket, upload & download files via Console, upload via AWS CLI (`aws s3 cp / sync`) <br> - Configure Static Website Hosting for the bucket | 04/20/2026 | 04/20/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn AWS IAM: <br>&emsp; + Concepts: User, Group, Role, Policy <br>&emsp; + AWS Managed Policy vs Customer Managed Policy vs Inline Policy <br>&emsp; + JSON Policy structure (Effect, Action, Resource, Condition) <br>&emsp; + Principle of least privilege <br>&emsp; + IAM Access Analyzer <br> - **Practice:** Create new IAM users for developer & admin, attach policies, enable MFA, create secure access keys for CI/CD                                                                                                                                            | 04/21/2026 | 04/21/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn basic Amazon VPC: <br>&emsp; + VPC & CIDR block (10.0.0.0/16) <br>&emsp; + Public / Private subnets <br>&emsp; + Route Table & Route propagation <br>&emsp; + Internet Gateway (IGW) <br>&emsp; + NAT Gateway & NAT Instance <br>&emsp; + DHCP Option Set <br> - **Practice:** Plan IP ranges for a VPC with 2 AZs, design CIDR ranges for each subnet                                                                                                                                                              | 04/22/2026 | 04/22/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn Security Group & NACL: <br>&emsp; + Stateful vs Stateless <br>&emsp; + Rule evaluation order <br> - **Practice:** Create a custom VPC with 2 subnets (public/private) across 2 AZs, launch EC2 in the private subnet, configure a Bastion Host in the public subnet and a NAT Gateway for egress <br> - Test SSH from Bastion → private EC2                                                                                                                                                                       | 04/23/2026 | 04/24/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 2 capstone lab:** <br>&emsp; + Create S3 buckets for the static site <br>&emsp; + Configure IAM Role for EC2 to access S3 (no hard-coded keys) <br>&emsp; + Upload images from EC2 in the private subnet to S3 via AWS CLI <br>&emsp; + Configure S3 static website hosting with public read <br>&emsp; + Add CloudFront in front of S3 for caching & HTTPS <br> - Update worklog progress & workshop on GitHub                                                                                                                                       | 04/24/2026 | 04/24/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 2 Achievements:
* Mastered Amazon S3 and key concepts:
  * **Bucket & Object:** bucket names are globally unique; object keys may contain `/` to mimic folders
  * **Storage class:** choose by access frequency and cost (Standard for hot, IA for warm, Glacier for archive)
  * **Versioning:** keep multiple versions of an object for easy rollback on accidental deletion
  * **Lifecycle rule:** automatically transition objects to cheaper classes or delete them after N days
  * **Static website hosting:** host static HTML/CSS/JS, integrated with CloudFront for global caching
  * **Object Lock / WORM:** immutable storage for compliance data
  * **Event Notifications:** connect S3 → SQS / SNS / Lambda to react to events (upload, delete)

* Managed IAM effectively:
  * Distinguished IAM User (person), Group (set of users), Role (for services)
  * Understood JSON Policy structure: `Effect`, `Action`, `Resource`, `Condition`
  * Wrote policies following the least-privilege principle (granting only what's needed)
  * Enabled MFA for administrative users and created dedicated IAM Access Keys for CI/CD
  * Used IAM Roles instead of hard-coded Access Keys when EC2 needs to access S3
  * Used IAM Access Analyzer to detect overly broad policies

* Understood and built the basic VPC model:
  * **CIDR block & Subnet planning:** carve a /16 VPC into /24 subnets giving ~256 IPs each
  * **Distinguished Public subnets** (route 0.0.0.0/0 → IGW) from **Private subnets** (route via NAT Gateway)
  * **Route Table & propagation:** associated subnets with the appropriate route tables
  * **Internet Gateway:** allows resources in public subnets to reach the Internet bidirectionally
  * **NAT Gateway:** allows resources in private subnets to egress to the Internet (download only)
  * **Bastion Host:** an EC2 in a public subnet used to SSH-jump into EC2 in private subnets

* Differentiated:
  * **Security Group** (stateful, applied at the ENI; if inbound allowed, outbound is automatic)
  * **NACL** (stateless, applied at the subnet; rules needed both inbound and outbound; processed in order)

* Completed the combined EC2 + S3 + IAM lab and deployed a simple static website:
  * Created 2 buckets: one for images, one for HTML hosting
  * EC2 in a private subnet still uploaded images to S3 via IAM Role
  * Public users accessed the static site through the S3 endpoint or CloudFront
  * Enabled Block Public Access on the image bucket; only the EC2 Role could read

* Regularly updated the personal workshop on GitHub; meaningful commits with a README for each week.
* New habits formed:
  * Every IAM policy includes comments explaining the reason for granted permissions
  * Never hard-code Access Keys in source — use env vars or Secrets Manager (to be covered next week)
  * Design the VPC before launching instances to avoid rebuilding later
* ...