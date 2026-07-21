---
title: "Dọn dẹp tài nguyên"
date: 2026-07-20
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

#### Giới thiệu

Sau khi hoàn thành workshop hoặc khi không còn sử dụng, bạn nên dọn dẹp các tài nguyên AWS để tránh phí phát sinh không cần thiết.

> ⚠️ **Cảnh báo:** Các bước dưới đây sẽ xóa VĨNH VIỄN dữ liệu. Hãy chắc chắn bạn đã backup dữ liệu cần thiết trước khi tiếp tục.

#### 1. Dọn dẹp Amazon S3

##### Xóa objects trong bucket

```bash
# Xóa tất cả objects trong bucket
aws s3 rm s3://medi-path-ease-uploads --recursive

# Verify đã xóa
aws s3 ls s3://medi-path-ease-uploads/
```

##### Xóa bucket

```bash
# Xóa bucket (chỉ hoạt động khi bucket đã trống)
aws s3 rb s3://medi-path-ease-uploads --force
```

##### Xóa bucket logs (nếu có)

```bash
# Xóa logs bucket
aws s3 rm s3://medi-path-ease-logs-<id> --recursive
aws s3 rb s3://medi-path-ease-logs-<id> --force
```

> **📸 [HÌNH 1]: Xóa S3 Bucket**

#### 2. Dọn dẹp CloudFront

##### Xem danh sách distributions

```bash
aws cloudfront list-distributions
```

##### Disable distribution

```bash
# Disable distribution trước khi xóa
aws cloudfront update-distribution \
    --id <distribution-id> \
    --if-match <etag> \
    --distribution-config '{
        "Enabled": false
    }'
```

##### Xóa distribution

```bash
# Xóa distribution (cần disable trước)
aws cloudfront delete-distribution \
    --id <distribution-id> \
    --if-match <etag>
```

#### 3. Dọn dẹp Amazon RDS

##### Tạo final snapshot (khuyến nghị)

```bash
# Tạo snapshot trước khi xóa
aws rds create-db-snapshot \
    --db-instance-identifier medi-path-ease-db \
    --db-snapshot-identifier medi-path-ease-final-backup
```

##### Xóa RDS Instance

```bash
# Xóa RDS instance (sẽ xóa tất cả data)
aws rds delete-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --skip-final-snapshot
```

##### Xóa tất cả snapshots

```bash
# Xóa automated backups
aws rds delete-db-instance \
    --db-instance-identifier medi-path-ease-db

# Xóa manual snapshots (nếu không cần)
aws rds delete-db-snapshot \
    --db-snapshot-identifier medi-path-ease-final-backup
```

> **📸 [HÌNH 2]: Xóa RDS Instance**

#### 4. Dọn dẹp Amazon Cognito

##### Xóa User Pool

```bash
# Xóa user pool (sẽ xóa tất cả users)
aws cognito-idp delete-user-pool \
    --user-pool-id ap-southeast-1_xxxxxxx
```

##### Xóa Identity Pool

```bash
# Xóa identity pool
aws cognito-identity delete-identity-pool \
    --identity-pool-id ap-southeast-1:xxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

#### 5. Dọn dẹp IAM

##### Xóa IAM User

```bash
# Xóa IAM user cho backend
aws iam delete-user \
    --user-name medi-path-ease-backend
```

##### Xóa IAM Role (nếu có)

```bash
# Xóa IAM role cho Lambda/Bedrock
aws iam delete-role \
    --role-name medi-path-ease-bedrock-role
```

##### Xóa Policy

```bash
# Xóa custom policy
aws iam delete-policy \
    --policy-arn arn:aws:iam::123456789:policy/medi-path-ease-policy
```

#### 6. Dọn dẹp DigitalOcean

##### Xóa App

```bash
# Xóa app qua doctl
doctl apps delete <app-id> --force
```

##### Xóa Database (nếu dùng managed)

```bash
# Xóa managed database
doctl databases delete <database-id> --force
```

#### 7. Xóa CloudWatch Logs

```bash
# Xóa log groups
aws logs delete-log-group \
    --log-group-name /aws/lambda/medi-path-ease

aws logs delete-log-group \
    --log-group-name /aws/lambda/bedrock-chat
```

#### 8. Checklist dọn dẹp

| Tài nguyên | Lệnh/AWS Service | Trạng thái |
|------------|------------------|-------------|
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

#### 9. Xác minh không còn phí

```bash
# Kiểm tra AWS Billing Dashboard
# https://console.aws.amazon.com/billing/

# Kiểm tra DigitalOcean
# https://www.digitalocean.com/account/overview
```

Kiểm tra các mục sau:
- [ ] Không còn S3 buckets
- [ ] Không còn RDS instances đang chạy
- [ ] Không còn Lambda functions
- [ ] Không còn CloudFront distributions
- [ ] Không còn Cognito User Pools

#### 10. Bảo mật trước khi xóa

Trước khi xóa, hãy đảm bảo:

1. **Backup dữ liệu quan trọng:**
   - Export database schema và data
   - Download files từ S3 nếu cần

2. **Thông báo cho users:**
   - Notify users trước khi xóa Cognito
   - Cho thời gian export data

3. **Kiểm tra dependent resources:**
   - Đảm bảo không có resources khác phụ thuộc

#### Tài liệu tham khảo

* [Deleting S3 Buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/delete-bucket.html)
* [Deleting a DB Instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/DELETE.html)
* [Deleting CloudFront Distributions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToDeleteDistribution.html)
* [Cleaning up Cognito Resources](https://docs.aws.amazon.com/cognito/latest/developerguide/cleaning-up-cognito-resources.html)
