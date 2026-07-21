---
title: "Cleanup Resources"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

#### Introduction

After completing the workshop or when no longer using the resources, you should clean up AWS resources to avoid unnecessary charges.

> ⚠️ **Warning:** The steps below will PERMANENTLY DELETE data. Make sure you have backed up necessary data before proceeding.

#### 1. Clean up Amazon S3

##### Delete objects in bucket

```bash
# Delete all objects in bucket
aws s3 rm s3://medi-path-ease-uploads --recursive

# Verify deletion
aws s3 ls s3://medi-path-ease-uploads/
```

##### Delete bucket

```bash
# Delete bucket (only works when bucket is empty)
aws s3 rb s3://medi-path-ease-uploads --force
```

##### Delete logs bucket (if any)

```bash
# Delete logs bucket
aws s3 rm s3://medi-path-ease-logs-<id> --recursive
aws s3 rb s3://medi-path-ease-logs-<id> --force
```

> **📸 [IMAGE 1]: Delete S3 Bucket**

#### 2. Clean up CloudFront

##### List distributions

```bash
aws cloudfront list-distributions
```

##### Disable distribution

```bash
# Disable distribution before deleting
aws cloudfront update-distribution \
    --id <distribution-id> \
    --if-match <etag> \
    --distribution-config '{
        "Enabled": false
    }'
```

##### Delete distribution

```bash
# Delete distribution (must disable first)
aws cloudfront delete-distribution \
    --id <distribution-id> \
    --if-match <etag>
```

#### 3. Clean up Amazon RDS

##### Create final snapshot (recommended)

```bash
# Create snapshot before deleting
aws rds create-db-snapshot \
    --db-instance-identifier medi-path-ease-db \
    --db-snapshot-identifier medi-path-ease-final-backup
```

##### Delete RDS Instance

```bash
# Delete RDS instance (will delete all data)
aws rds delete-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --skip-final-snapshot
```

##### Delete all snapshots

```bash
# Delete automated backups
aws rds delete-db-instance \
    --db-instance-identifier medi-path-ease-db

# Delete manual snapshots (if not needed)
aws rds delete-db-snapshot \
    --db-snapshot-identifier medi-path-ease-final-backup
```

> **📸 [IMAGE 2]: Delete RDS Instance**

#### 4. Clean up Amazon Cognito

##### Delete User Pool

```bash
# Delete user pool (will delete all users)
aws cognito-idp delete-user-pool \
    --user-pool-id ap-southeast-1_xxxxxxx
```

##### Delete Identity Pool

```bash
# Delete identity pool
aws cognito-identity delete-identity-pool \
    --identity-pool-id ap-southeast-1:xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### 5. Clean up IAM

##### Delete IAM User

```bash
# Delete IAM user for backend
aws iam delete-user \
    --user-name medi-path-ease-backend
```

##### Delete IAM Role (if any)

```bash
# Delete IAM role for Lambda/Bedrock
aws iam delete-role \
    --role-name medi-path-ease-bedrock-role
```

##### Delete Policy

```bash
# Delete custom policy
aws iam delete-policy \
    --policy-arn arn:aws:iam::123456789:policy/medi-path-ease-policy
```

#### 6. Clean up DigitalOcean

##### Delete App

```bash
# Delete app via doctl
doctl apps delete <app-id> --force
```

##### Delete Database (if using managed)

```bash
# Delete managed database
doctl databases delete <database-id> --force
```

#### 7. Delete CloudWatch Logs

```bash
# Delete log groups
aws logs delete-log-group \
    --log-group-name /aws/lambda/medi-path-ease

aws logs delete-log-group \
    --log-group-name /aws/lambda/bedrock-chat
```

#### 8. Cleanup Checklist

| Resource | Command/AWS Service | Status |
|----------|---------------------|--------|
| S3 Bucket | `aws s3 rb` | ☐ |
| CloudFront | AWS Console | ☐ |
| RDS Database | `aws rds delete-db-instance` | ☐ |
| RDS Snapshots | `aws rds delete-db-snapshot` | ☐ |
| Cognito User Pool | `aws cognito-idp delete-user-pool` | ☐ |
| Cognito Identity Pool | `aws cognito-identity delete-identity-pool` | ☐ |
| IAM User | `aws iam delete-user` | ☐ |
| IAM Role | `aws iam delete-role` | ☐ |
| DigitalOcean App | `doctl apps delete` | ☐ |
| CloudWatch Logs | `aws logs delete-log-group` | ☐ |

#### 9. Verify No Charges

```bash
# Check AWS Billing Dashboard
# https://console.aws.amazon.com/billing/

# Check DigitalOcean
# https://www.digitalocean.com/account/overview
```

Check the following:
- [ ] No S3 buckets remaining
- [ ] No RDS instances running
- [ ] No Lambda functions
- [ ] No CloudFront distributions
- [ ] No Cognito User Pools

#### 10. Security Before Deletion

Before deleting, ensure:

1. **Backup important data:**
   - Export database schema and data
   - Download files from S3 if needed

2. **Notify users:**
   - Notify users before deleting Cognito
   - Allow time for data export

3. **Check dependent resources:**
   - Ensure no other resources depend on these

#### References

* [Deleting S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html)
* [Deleting a DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/DELETE.html)
* [Deleting CloudFront Distributions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToDeleteDistribution.html)
* [Cleaning up Cognito Resources](https://docs.aws.amazon.com/cognito/latest/developerguide/cleaning-up-cognito-resources.html)
