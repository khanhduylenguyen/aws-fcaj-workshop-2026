---
title: "Week 5 Worklog"
date: 2026-05-10
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
### Week 5 Objectives:
* Get familiar with Infrastructure as Code (IaC) through AWS CloudFormation and AWS CDK.
* Set up CI/CD pipelines with GitHub Actions & AWS CodePipeline/CodeBuild.
* Automate infrastructure and application deployment from Git to Production.
* Understand best practices for versioning, rollback, and separating dev/staging/prod environments.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                  | Start Date | Completion Date | Reference Material                        |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Learn Infrastructure as Code (IaC): <br>&emsp; + Concepts & benefits (reuse, versioning, audit) <br>&emsp; + Declarative vs Imperative <br>&emsp; + Compare CloudFormation, Terraform, Pulumi <br> - Learn AWS CloudFormation: <br>&emsp; + Template (YAML/JSON) <br>&emsp; + Stack & Nested Stack <br>&emsp; + Change Set & Drift Detection <br>&emsp; + Outputs, Parameters, Mappings <br>&emsp; + Custom Resource <br>&emsp; + StackSets (multi-account, multi-region)                                                                                                       | 05/11/2026 | 05/11/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - **Practice:** Write a CloudFormation template deploying VPC + 2 subnets + EC2 + Security Group + IAM Role <br> - Use Change Sets to preview changes <br> - Learn AWS CDK (Cloud Development Kit): <br>&emsp; + Writing IaC in Python/TypeScript/Java <br>&emsp; + Structure: App → Stack → Construct <br>&emsp; + Commands `cdk init / synth / deploy / diff / destroy`                                                                                                                                            | 05/12/2026 | 05/12/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn GitHub Actions: <br>&emsp; + Workflow, Job, Step, Action <br>&emsp; + Triggers: push, pull_request, schedule, workflow_dispatch <br>&emsp; + Secrets & Environment (production) <br>&emsp; + Self-hosted runner <br>&emsp; + Matrix build <br> - Learn AWS CodePipeline & CodeBuild: <br>&emsp; + Stage & Action <br>&emsp; + Source (CodeCommit/S3/Bitbucket) <br>&emsp; + Build artifact (S3) <br>&emsp; + Deploy (CloudFormation, ECS, Lambda)                                                                                                                                       | 05/13/2026 | 05/13/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - **Practice:** Build a complete CI/CD pipeline: <br>&emsp; + GitHub Actions to test & build a Node.js/React app <br>&emsp; + Push Docker image to Amazon ECR <br>&emsp; + Deploy container to ECS Fargate or EC2 by updating the task definition <br>&emsp; + Secure secrets via GitHub Secrets & AWS Secrets Manager <br>&emsp; + Automatically run `cdk deploy` / `sam deploy` on merge to main                                                                                                                                         | 05/14/2026 | 05/15/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 5 capstone lab:** <br>&emsp; + CloudFormation deploys the full stack (VPC + ALB + ASG + RDS) <br>&emsp; + GitHub Actions auto runs lint/test on push PR <br>&emsp; + CodePipeline auto deploys on new release tag <br>&emsp; + Auto rollback on failed deploy (CloudWatch alarm → CodeDeploy rollback) <br>&emsp; + Update worklog & workshop, write a blog "IaC & CI/CD on AWS"                                                                                                                                                | 05/15/2026 | 05/15/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 5 Achievements:
* Mastered the IaC mindset:
  * Managing infrastructure with files (YAML/JSON) instead of clicks
  * Versioning & reviewing infrastructure config in Git
  * Reproducible environments across regions and accounts
  * The whole team shares the same infra code, easier onboarding

* Mastered AWS CloudFormation:
  * Wrote a template to deploy VPC + 2 subnets + EC2 + SG + IAM Role
  * Used Parameters & Mappings to customise per environment (dev/prod)
  * Managed Stacks with Change Sets previewing before apply
  * Drift Detection to find resources changed outside CloudFormation
  * StackSets deploying the same stack across many accounts/regions

* Explored AWS CDK:
  * Initialised a CDK project with `cdk init app --language python`
  * Used L2 Construct (e.g. `aws_ec2.Vpc`) instead of writing raw CFN
  * `cdk synth` generated a CloudFormation template
  * `cdk diff` compared code with current infrastructure
  * `cdk deploy` automatically applied the Change Set

* Built a complete CI/CD pipeline:
  * GitHub Actions workflow:
    * Trigger: `on: push: branches: [main]` and `pull_request`
    * `test` job: ESLint, unit tests with Jest, coverage report
    * `build` job: docker build & push to ECR tagged by commit SHA
    * `deploy` job: update ECS task definition, trigger deploy
  * CodePipeline: GitHub → CodeBuild (test + build) → CodeDeploy (deploy ECS)
  * Automatically pushed Docker images to Amazon ECR
  * Auto deploy to ECS on new release tag (`v*.*.*`)
  * Secured secrets via GitHub Secrets and AWS Secrets Manager

* Integrated CloudFormation + CodePipeline:
  * Pipeline auto runs `cloudformation deploy` on template changes
  * Tracked stack status from the Pipeline console
  * Automatic rollback on deploy failure: CloudWatch alarm → CodeDeploy rollback

* Understood best practices:
  * Stack separation: Network Stack (VPC) → Application Stack (ALB, ASG) → Database Stack (RDS)
  * Manage parameters via SSM Parameter Store or Secrets Manager
  * Never commit credentials to Git; use OIDC roles so GitHub Actions access AWS without access keys

* Completed the capstone lab and committed the full IaC + workflow source to GitHub.
* Wrote a sharing blog post on IaC & CI/CD on AWS.
* New habits formed:
  * Every infra change goes through a Pull Request reviewed by at least one other person
  * Stack naming convention: `service-environment-region`, e.g. `workshop-prod-ap-southeast-1`
  * Use Git semantic-version tags (v1.2.3) to trigger deploys
* ...