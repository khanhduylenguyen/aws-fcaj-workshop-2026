---
title: "Week 1 Worklog"
date: 2026-04-12
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
### Week 1 Objectives:
* Get acquainted with the First Cloud AI Journey 2026 team and understand the rules and working procedures at the internship unit.
* Understand the AWS Cloud overview: history, deployment models (IaaS/PaaS/SaaS) and core service categories.
* Set up a local working environment with the AWS Console and AWS CLI on a personal computer.
* Kick off the Cloud + AI journey with an AWS Free Tier account and deploy the first EC2 instance.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                                       | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ----------------------------------------- |
| Sun | - Attend the FCAJ 2026 opening ceremony <br> - Listen to mentor and member sharing <br> - Receive the internal account and communication channels (Slack/Discord) <br> - Briefly introduce myself to the group                                                                                                                                                                                                                              | 04/12/2026 | 04/12/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 2   | - Get acquainted with FCAJ members <br> - Read and take note of internship unit rules and regulations <br> - Explore GitHub and how to manage the personal workshop via Hugo <br> - Install Visual Studio Code, Git and useful extensions                                                                                                                                                                                                       | 04/13/2026 | 04/13/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn what AWS Cloud is, its history and market positioning <br> - Categorise AWS services: <br>&emsp; + **Compute:** EC2, Lambda, ECS, EKS <br>&emsp; + **Storage:** S3, EBS, EFS, Glacier <br>&emsp; + **Networking:** VPC, CloudFront, Route 53 <br>&emsp; + **Database:** RDS, DynamoDB, Aurora <br>&emsp; + **Security:** IAM, KMS, WAF, Shield <br>&emsp; + **AI/ML:** SageMaker, Bedrock, Rekognition, Comprehend                                                                              | 04/14/2026 | 04/14/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Create an AWS Free Tier account (requires an international card) <br> - Explore the AWS Console: how to find & access services by region <br> - Learn AWS CLI: <br>&emsp; + Install AWS CLI v2 on Windows/macOS/Linux <br>&emsp; + Configure Access Key, Secret Key, default Region <br> - **Practice:** <br>&emsp; + Create an AWS account <br>&emsp; + Install AWS CLI & run `aws configure` <br>&emsp; + Use AWS CLI: `aws sts get-caller-identity`, `aws ec2 describe-regions` | 04/15/2026 | 04/15/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn basic EC2: <br>&emsp; + Instance types (t3.micro, t3.small, m5.large, ...) <br>&emsp; + AMI (Amazon Machine Image) <br>&emsp; + EBS (Elastic Block Store) & volume types <br>&emsp; + Security Group vs NACL <br>&emsp; + Key Pair <br> - SSH connection methods to EC2 (SSH client, EC2 Instance Connect) <br> - Learn Elastic IP and how it differs from Public IP                                                                                                                                       | 04/16/2026 | 04/17/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 1 capstone practice:** <br>&emsp; + Launch an EC2 instance (Amazon Linux 2023, t3.micro) <br>&emsp; + Connect via SSH from local terminal <br>&emsp; + Install Nginx/Apache and open ports 80/443 in the Security Group <br>&emsp; + Create an extra EBS volume and attach it to the instance <br>&emsp; + Attach an Elastic IP, restart & verify persistent IP <br> - Update the Week 1 worklog & workshop on GitHub                                                                                                                                  | 04/17/2026 | 04/17/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 1 Achievements:
* Attended the opening ceremony and connected with mentors and members in FCAJ 2026; can now communicate via the internal channels.
* Gained a holistic view of the AWS Cloud:
  * Milestones in the history of AWS from 2006 to today
  * The Shared Responsibility Model
  * The distinction between IaaS / PaaS / SaaS and corresponding AWS services
  * Free Tier scope (12 months, always free, trials)

* Understood what AWS is and mastered the core service categories:
  * **Compute:** EC2, Lambda, ECS, EKS
  * **Storage:** S3, EBS, EFS, Glacier
  * **Networking:** VPC, CloudFront, Route 53
  * **Database:** RDS, DynamoDB, Aurora
  * **Security:** IAM, KMS, WAF, Shield
  * **AI/ML:** SageMaker, Bedrock, Rekognition, Comprehend

* Successfully created and configured an AWS Free Tier account; enabled MFA for the root account and IAM user.
* Became familiar with the AWS Management Console:
  * Know how to switch Regions and find services via the search bar
  * Read CPU, RAM, and network metrics on the EC2 dashboard
  * Used AWS Budgets to receive cost-threshold alerts

* Installed and configured AWS CLI on the computer, including:
  * Access Key & Secret Key (never committed to Git)
  * Default Region (e.g. ap-southeast-1 - Singapore)
  * Default output format (json)
  * Named profiles to quickly switch between accounts

* Used AWS CLI to perform basic operations such as:
  * `aws sts get-caller-identity` - check account & configuration information
  * `aws ec2 describe-regions` - retrieve the list of regions
  * `aws ec2 describe-instances` - view EC2 instances
  * `aws ec2 create-key-pair` / `delete-key-pair` - create and manage key pairs
  * `aws ec2 describe-volumes` - check attached EBS volumes
  * `aws ec2 associate-address` - attach an Elastic IP

* Launched and configured the first EC2 instance:
  * Selected Amazon Linux 2023 AMI (optimised for EC2)
  * Chose instance type t3.micro (Free Tier eligible)
  * Configured a Security Group opening ports 22 (SSH) & 80 (HTTP)
  * Attached a default 8 GB gp3 EBS volume

* Connected via SSH successfully:
  * Used the terminal: `ssh -i key.pem ec2-user@<public-ip>`
  * Used EC2 Instance Connect from the browser (no key required)

* Attached an additional EBS volume to the instance:
  * Created a 20 GB gp3 EBS volume in the same AZ as the instance
  * Attached the volume at `/dev/sdf`
  * Formatted with `mkfs.xfs` and mounted it

* Understood the Elastic IP concept and how to assign a static IP to an instance; understood the difference with a Public IP (changes when you stop/start).
* Able to switch between web console and CLI to manage AWS resources in parallel.
* Learned to tag resources (Name, Environment, Owner, Project) for easier tracking and cost allocation.
* Committed the Week 1 worklog to GitHub and learned to open Pull Requests and review code with teammates.
* New habits formed:
  * Spent 30 minutes each day reading AWS Skill Builder and AWS Whitepapers
  * Wrote newly learned concepts into a notebook for long-term retention
  * Asked questions in the FCAJ group whenever I got stuck
* ...