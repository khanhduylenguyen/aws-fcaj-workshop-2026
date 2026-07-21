---
title: "Cấu hình Cơ sở dữ liệu RDS PostgreSQL"
date: 2026-07-20
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Giới thiệu Amazon RDS PostgreSQL

**Amazon RDS (Relational Database Service)** là dịch vụ cơ sở dữ liệu quan hệ được quản lý hoàn toàn bởi AWS. Medi Path Ease sử dụng RDS PostgreSQL để lưu trữ dữ liệu chính của ứng dụng.

##### Tại sao chọn RDS PostgreSQL?

| Ưu điểm | Mô tả |
|---------|--------|
| **Quản lý tự động** | AWS tự động xử lý backup, patching, failover |
| **Multi-AZ** | Đảm bảo high availability với replica ở AZ khác |
| **Scalability** | Dễ dàng scale up/down instance |
| **Security** | Mã hóa dữ liệu at-rest và in-transit |
| **Performance** | Hỗ trợ Read Replicas cho read-heavy workload |

#### Cấu hình RDS PostgreSQL

##### Bước 1: Tạo RDS Instance

Truy cập AWS Console → RDS → Create database

```bash
# Hoặc sử dụng AWS CLI
aws rds create-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --db-instance-class db.t3.micro \
    --engine postgres \
    --engine-version 15.4 \
    --allocated-storage 20 \
    --master-username mediadmin \
    --master-user-password "YourSecurePassword123!" \
    --backup-retention-period 7 \
    --multi-az \
    --region ap-southeast-1
```

##### Bước 2: Cấu hình Security Group

```bash
# Tạo Security Group
aws ec2 create-security-group \
    --group-name medi-path-ease-rds-sg \
    --description "Security group for Medi Path Ease RDS"

# Mở port PostgreSQL (5432) cho backend server
aws ec2 authorize-security-group-ingress \
    --group-name medi-path-ease-rds-sg \
    --protocol tcp \
    --port 5432 \
    --cidr 0.0.0.0/0
```

> **📸 [HÌNH 1]: RDS Instance Configuration trên AWS Console**

##### Bước 3: Enable Encryption

```bash
# Bật mã hóa at-rest (mặc định khi tạo qua console)
aws rds modify-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --storage-encryption --kms-key-id alias/aws/rds
```

#### Kết nối từ Backend

##### Cấu hình Environment Variables

```bash
# File .env
DATABASE_HOST=medi-path-ease-db.xxxxx.ap-southeast-1.rds.amazonaws.com
DATABASE_PORT=5432
DATABASE_NAME=medi_path_ease
DATABASE_USER=mediadmin
DATABASE_PASSWORD=YourSecurePassword123!
DATABASE_SSL=true
```

##### Cài đặt PostgreSQL Client

```bash
# Cài đặt pg (PostgreSQL client cho Node.js)
npm install pg

# Hoặc sử dụng Prisma ORM
npm install prisma @prisma/client
```

##### Kết nối sử dụng Prisma

```javascript
// prisma/schema.prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

model User {
  id            String   @id @default(uuid())
  email         String   @unique
  password      String
  name          String
  role          Role     @default(PATIENT)
  phone         String?
  dateOfBirth   DateTime?
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt

  appointments  Appointment[]
  medicalRecords MedicalRecord[]
  prescriptions  Prescription[]
}

model Appointment {
  id            String   @id @default(uuid())
  patientId     String
  doctorId      String
  scheduledAt   DateTime
  status        AppointmentStatus @default(SCHEDULED)
  notes         String?
  createdAt     DateTime @default(now())

  patient       User     @relation(fields: [patientId], references: [id])
  doctor        User     @relation(fields: [doctorId], references: [id])
}

model MedicalRecord {
  id            String   @id @default(uuid())
  patientId     String
  doctorId      String
  type          RecordType
  title         String
  content       String
  fileUrl       String?
  createdAt     DateTime @default(now())

  patient       User     @relation(fields: [patientId], references: [id])
  doctor        User     @relation(fields: [doctorId], references: [id])
}

enum Role {
  PATIENT
  DOCTOR
  ADMIN
}

enum AppointmentStatus {
  SCHEDULED
  COMPLETED
  CANCELLED
}

enum RecordType {
  LAB_RESULT
  PRESCRIPTION
  DIAGNOSIS
  NOTE
}
```

##### Kết nối đơn giản với pg

```javascript
// lib/db.js
import { Pool } from 'pg';

const pool = new Pool({
  host: process.env.DATABASE_HOST,
  port: process.env.DATABASE_PORT,
  database: process.env.DATABASE_NAME,
  user: process.env.DATABASE_USER,
  password: process.env.DATABASE_PASSWORD,
  ssl: {
    rejectUnauthorized: false
  },
  max: 20,
  idleTimeoutMillis: 30000,
  connectionTimeoutMillis: 2000,
});

export const query = (text, params) => pool.query(text, params);
export default pool;
```

#### Schema Database

> **📸 [HÌNH 2]: Database Schema Diagram**

##### Bảng chính

| Bảng | Mô tả |
|------|--------|
| users | Thông tin người dùng (bệnh nhân, bác sĩ, admin) |
| appointments | Lịch hẹn khám |
| medical_records | Hồ sơ bệnh án điện tử |
| prescriptions | Đơn thuốc |
| payments | Lịch sử thanh toán |
| chat_messages | Tin nhắn chat telemedicine |

#### Backup & Recovery

##### Tự động Backup

```bash
# RDS tự động backup theo cấu hình
# Backup retention: 7 ngày (config ở bước 1)
# Backup window: Daily 03:00-04:00 UTC
```

##### Manual Snapshot

```bash
# Tạo snapshot thủ công
aws rds create-db-snapshot \
    --db-instance-identifier medi-path-ease-db \
    --db-snapshot-identifier medi-path-ease-manual-backup
```

##### Restore từ Snapshot

```bash
# Khôi phục từ snapshot (tạo instance mới)
aws rds restore-db-instance-from-db-snapshot \
    --db-instance-identifier medi-path-ease-db-restored \
    --db-snapshot-identifier medi-path-ease-manual-backup
```

#### Monitoring & Logging

##### CloudWatch Metrics

```bash
# Xem metrics qua AWS CLI
aws cloudwatch get-metric-statistics \
    --namespace AWS/RDS \
    --metric-name DatabaseConnections \
    --dimensions Name=DBInstanceIdentifier,Value=medi-path-ease-db \
    --start-time 2026-07-01T00:00:00Z \
    --end-time 2026-07-20T00:00:00Z \
    --period 3600 \
    --statistics Average
```

##### Enhanced Monitoring

```bash
# Bật Enhanced Monitoring
aws rds modify-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --monitoring-interval 60 \
    --monitoring-role-arn arn:aws:iam::123456789:role/rds-monitoring-role
```

#### Resource Summary

| Resource | Giá trị | Mô tả |
|---------|---------|--------|
| RDS Instance | `medi-path-ease-db` | PostgreSQL 15.4 |
| Instance Class | `db.t3.micro` | Phù hợp cho dev/staging |
| Storage | 20 GB | GP3 |
| Multi-AZ | Enabled | High availability |
| Backup Retention | 7 days | Auto backup |
| Encryption | AES-256 | At-rest encryption |

> **📸 [HÌNH 3]: RDS Monitoring Dashboard**

#### Tài liệu tham khảo

* [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/)
* [PostgreSQL Documentation](https://www.postgresql.org/docs/)
* [Prisma ORM](https://www.prisma.io/docs)
