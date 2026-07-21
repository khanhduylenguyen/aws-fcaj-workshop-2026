---
title: "RDS PostgreSQL Database Configuration"
date: 2026-07-20
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Introduction to Amazon RDS PostgreSQL

**Amazon RDS (Relational Database Service)** is a fully managed relational database service by AWS. Medi Path Ease uses RDS PostgreSQL to store the main application data.

##### Why Choose RDS PostgreSQL?

| Advantage | Description |
|-----------|------------|
| **Fully Managed** | AWS handles backup, patching, failover automatically |
| **Multi-AZ** | High availability with replica in different AZ |
| **Scalability** | Easy to scale up/down instance |
| **Security** | Data encryption at-rest and in-transit |
| **Performance** | Read Replicas support for read-heavy workload |

#### RDS PostgreSQL Configuration

##### Step 1: Create RDS Instance

Access AWS Console → RDS → Create database

```bash
# Or use AWS CLI
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

##### Step 2: Configure Security Group

```bash
# Create Security Group
aws ec2 create-security-group \
    --group-name medi-path-ease-rds-sg \
    --description "Security group for Medi Path Ease RDS"

# Open PostgreSQL port (5432) for backend server
aws ec2 authorize-security-group-ingress \
    --group-name medi-path-ease-rds-sg \
    --protocol tcp \
    --port 5432 \
    --cidr 0.0.0.0/0
```

> **📸 [IMAGE 1]: RDS Instance Configuration on AWS Console**

##### Step 3: Enable Encryption

```bash
# Enable at-rest encryption (default when creating via console)
aws rds modify-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --storage-encryption --kms-key-id alias/aws/rds
```

#### Connecting from Backend

##### Environment Variables Configuration

```bash
# .env file
DATABASE_HOST=medi-path-ease-db.xxxxx.ap-southeast-1.rds.amazonaws.com
DATABASE_PORT=5432
DATABASE_NAME=medi_path_ease
DATABASE_USER=mediadmin
DATABASE_PASSWORD=YourSecurePassword123!
DATABASE_SSL=true
```

##### Install PostgreSQL Client

```bash
# Install pg (PostgreSQL client for Node.js)
npm install pg

# Or use Prisma ORM
npm install prisma @prisma/client
```

##### Connection using Prisma

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

##### Simple Connection with pg

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

#### Database Schema

> **📸 [IMAGE 2]: Database Schema Diagram**

##### Main Tables

| Table | Description |
|-------|------------|
| users | User information (patients, doctors, admin) |
| appointments | Appointment schedules |
| medical_records | Electronic medical records |
| prescriptions | Prescriptions |
| payments | Payment history |
| chat_messages | Telemedicine chat messages |

#### Backup & Recovery

##### Automatic Backup

```bash
# RDS auto backup based on configuration
# Backup retention: 7 days (configured in step 1)
# Backup window: Daily 03:00-04:00 UTC
```

##### Manual Snapshot

```bash
# Create manual snapshot
aws rds create-db-snapshot \
    --db-instance-identifier medi-path-ease-db \
    --db-snapshot-identifier medi-path-ease-manual-backup
```

##### Restore from Snapshot

```bash
# Restore from snapshot (creates new instance)
aws rds restore-db-instance-from-db-snapshot \
    --db-instance-identifier medi-path-ease-db-restored \
    --db-snapshot-identifier medi-path-ease-manual-backup
```

#### Monitoring & Logging

##### CloudWatch Metrics

```bash
# View metrics via AWS CLI
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
# Enable Enhanced Monitoring
aws rds modify-db-instance \
    --db-instance-identifier medi-path-ease-db \
    --monitoring-interval 60 \
    --monitoring-role-arn arn:aws:iam::123456789:role/rds-monitoring-role
```

#### Resource Summary

| Resource | Value | Description |
|---------|--------|------------|
| RDS Instance | `medi-path-ease-db` | PostgreSQL 15.4 |
| Instance Class | `db.t3.micro` | Suitable for dev/staging |
| Storage | 20 GB | GP3 |
| Multi-AZ | Enabled | High availability |
| Backup Retention | 7 days | Auto backup |
| Encryption | AES-256 | At-rest encryption |

> **📸 [IMAGE 3]: RDS Monitoring Dashboard**

#### References

* [Amazon RDS Documentation](https://docs.aws.amazon.com/rds/)
* [PostgreSQL Documentation](https://www.postgresql.org/docs/)
* [Prisma ORM](https://www.prisma.io/docs)
