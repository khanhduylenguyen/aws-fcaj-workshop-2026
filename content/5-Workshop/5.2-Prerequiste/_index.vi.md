---
title : "Các bước chuẩn bị"
date : 2026-07-20
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### 1. Tài khoản AWS & Region

Trong workshop này, chúng ta sẽ dùng region **Singapore (ap-southeast-1)** vì:
* Latency thấp từ Việt Nam
* Hỗ trợ đầy đủ các tính năng S3

Hãy đảm bảo bạn đã:
* Đăng nhập vào AWS Console với user có quyền IAM (hoặc user có quyền Administrator).
* Region được chọn là **Singapore** ở góc trên bên phải Console.

#### 2. Tạo IAM User cho Backend

Tạo một IAM user riêng cho backend với quyền hạn chế (least privilege):

1. Mở **IAM Console** → **Users** → **Create user**
2. Tên user: `medi-path-ease-s3`
3. Chọn **Access key - Programmatic access**
4. Click **Next: Permissions**
5. Chọn **Attach existing policies directly** → **Create policy**

**IAM Policy cho S3:**

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

6. Attach policy vào user
7. **Lưu lại Access Key ID và Secret Access Key** (chỉ hiển thị 1 lần duy nhất!)

![IAM User Creation](/images/5-Workshop/5.2-Prerequisite/create-iam-user.png)

#### 3. Cài đặt AWS CLI

```bash
# Kiểm tra đã cài chưa
aws --version

# Nếu chưa cài, tải và cài AWS CLI v2
# macOS/Linux:
curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "AWSCLIV2.pkg"
sudo installer -pkg AWSCLIV2.pkg -target /

# Windows: Tải từ https://aws.amazon.com/cli/
```

#### 4. Cấu hình AWS Credentials

```bash
# Cấu hình credentials cho IAM user vừa tạo
aws configure

# Nhập các thông tin:
# AWS Access Key ID [None]: AKIAIOSFODNN7EXAMPLE
# AWS Secret Access Key [None]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
# Default region name [None]: ap-southeast-1
# Default output format [None]: json
```

Verify credentials:

```bash
aws sts get-caller-identity
```

Kết quả mong đợi:
```json
{
    "UserId": "AIDAIOSFODNN7EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/medi-path-ease-s3"
}
```

#### 5. Kiểm tra Node.js và npm

```bash
# Kiểm tra version
node --version    # >= 18.x
npm --version     # >= 9.x
```

Nếu chưa cài, tải từ https://nodejs.org/

#### 6. Clone và cài đặt project

```bash
# Clone project
cd /path/to/your/projects
git clone <medi-path-ease-repo>

# Di chuyển vào thư mục
cd medi-path-ease-main/medi-path-ease-main

# Cài đặt dependencies
npm install

# Cài đặt AWS SDK cho Node.js (đã có trong package.json)
npm install @aws-sdk/client-s3 @aws-sdk/s3-request-presigner
```

#### 7. Cấu hình Environment Variables

Copy file `.env.example` thành `.env` và điền thông tin:

```bash
# Backend config
PORT=3001
NODE_ENV=development

# AWS S3 Configuration
AWS_REGION=ap-southeast-1
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE      # Thay bằng Access Key của bạn
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/...     # Thay bằng Secret Key của bạn
AWS_S3_BUCKET=medi-path-ease-uploads         # Bucket name

# JWT (để test authenticated requests)
JWT_SECRET=your-super-secret-jwt-key-minimum-32-chars
```

#### 8. Kiểm tra môi trường

Chạy script kiểm tra:

```bash
# Verify AWS credentials
aws s3 ls 2>&1 | head -5

# Verify Node.js
node -e "console.log('Node OK:', process.version)"

# Test S3 connection (sẽ thất bại nếu bucket chưa tồn tại - OK)
aws s3api head-bucket --bucket medi-path-ease-uploads 2>&1 || echo "Bucket chưa tồn tại - sẽ tạo ở bước tiếp theo"
```

Kết quả mong đợi:
```
An error occurred (404) when calling the HeadBucket operation: Not Found
Bucket chưa tồn tại - sẽ tạo ở bước tiếp theo
```

#### 9. Chuẩn bị Postman/curl để test API

Download Postman hoặc chuẩn bị curl commands để test API endpoints.

> ⚠️ **Lưu ý bảo mật:** Không commit file `.env` lên Git. Thêm vào `.gitignore` nếu chưa có.

#### Tài liệu tham khảo
* [Creating an IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)
* [AWS CLI Configuration](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html)
* [Installing AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
