---
title : "Prerequisites"
date : 2026-07-20
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### 1. AWS Account & Region

In this workshop, we'll use the **Singapore (ap-southeast-1)** region because:
* Low latency from Vietnam
* Full support for all S3 features

Make sure you:
* Logged into AWS Console with a user with IAM permissions (or Administrator).
* Region is set to **Singapore** in the top-right corner of the Console.

#### 2. Create IAM User for Backend

Create a separate IAM user for the backend with restricted permissions (least privilege):

1. Open **IAM Console** → **Users** → **Create user**
2. User name: `medi-path-ease-s3`
3. Select **Access key - Programmatic access**
4. Click **Next: Permissions**
5. Select **Attach existing policies directly** → **Create policy**

**S3 IAM Policy:**

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowSpecificBucketOnly",
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:HeadObject"
      ],
      "Resource": "arn:aws:s3:::medi-path-ease-uploads/*"
    },
    {
      "Sid": "AllowListBucket",
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket"
      ],
      "Resource": "arn:aws:s3:::medi-path-ease-uploads",
      "Condition": {
        "StringLike": {
          "s3:prefix": "uploads/*"
        }
      }
    },
    {
      "Sid": "DenyDeleteBucket",
      "Effect": "Deny",
      "Action": [
        "s3:DeleteBucket",
        "s3:DeleteBucketPolicy",
        "s3:PutBucketPolicy"
      ],
      "Resource": "arn:aws:s3:::medi-path-ease-uploads"
    }
  ]
}
```

6. Attach the policy to the user
7. **Save Access Key ID and Secret Access Key** (only shown once!)

#### 3. Install AWS CLI

```bash
# Check if installed
aws --version

# If not installed, download and install AWS CLI v2
# macOS/Linux:
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /

# Windows: Download from https://aws.amazon.com/cli/
```

#### 4. Configure AWS Credentials

```bash
# Configure credentials for the IAM user
aws configure

# Enter the following:
# AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
# AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
# Default region name [None]: ap-southeast-1
# Default output format [None]: json
```

Verify credentials:

```bash
aws sts get-caller-identity
```

Expected output:
```json
{
    "UserId": "AIDAIOSFODNN7EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/medi-path-ease-s3"
}
```

#### 5. Check Node.js and npm

```bash
# Check versions
node --version    # >= 18.x
npm --version     # >= 9.x
```

If not installed, download from https://nodejs.org/

#### 6. Clone and Setup Project

```bash
# Clone project
cd /path/to/your/projects
git clone <medi-path-ease-repo>

# Navigate to project directory
cd medi-path-ease-main/medi-path-ease-main

# Install dependencies
npm install

# Install AWS SDK for Node.js (already in package.json)
npm install @aws-sdk/client-s3 @aws-sdk/s3-request-presigner
```

#### 7. Configure Environment Variables

Copy `.env.example` to `.env` and fill in the information:

```bash
# Backend config
PORT=3001
NODE_ENV=development

# AWS S3 Configuration
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE      # Replace with your Access Key
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/...     # Replace with your Secret Key
AWS_S3_BUCKET=medi-path-ease-uploads         # Bucket name

# JWT (for testing authenticated requests)
JWT_SECRET=your-super-secret-jwt-key-minimum-32-chars
```

#### 8. Verify Environment

Run verification script:

```bash
# Verify AWS credentials
aws s3 ls 2>&1 | head -5

# Verify Node.js
node -e "console.log('Node OK:', process.version)"

# Test S3 connection (will fail if bucket doesn't exist - OK)
aws s3api head-bucket --bucket medi-path-ease-uploads 2>&1 || echo "Bucket doesn't exist - will create in next step"
```

Expected output:
```
An error occurred (404) when calling the HeadBucket operation: Not Found
Bucket doesn't exist - will create in next step
```

#### 9. Prepare Postman/curl for API Testing

Download Postman or prepare curl commands to test API endpoints.

> ⚠️ **Security Note:** Do not commit `.env` file to Git. Add to `.gitignore` if not already present.

#### References
* [Creating an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)
* [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
* [Installing AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
