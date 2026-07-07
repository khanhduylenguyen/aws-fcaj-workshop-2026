---
title: "Blogs Posted"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

During the 12-week internship I published **3 in-depth technical blogs** on [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) — analyzing real-world case studies on AWS, covering new service features, cost optimization with IaC, and using AI services to solve concrete enterprise problems.

| # | Topic | Category (per FCAJ requirement) | Date |
|---|---|---|---|
| Blog 1 | **Automate AWS Storage Gateway AL2 → AL2023 migration with IaC** | Operations & cost optimization with Terraform + Ansible | 12/04/2026 |
| Blog 2 | **Automate medical record digitization with Amazon Bedrock Data Automation + AWS HealthLake** | Using AI services to solve a healthcare industry problem | 03/05/2026 |
| Blog 3 | **Solving automated contract intelligence with Doczy.ai™ on AWS** | New features: Textract + Bedrock + Smart Chunking | 07/06/2026 |

---

### [Blog 1 - Automate AWS Storage Gateway AL2 to AL2023 migration with IaC](3.1-Blog1/)

**Amazon Linux 2 (AL2)** is reaching End-of-Support (06/2026) — and AWS does **not support in-place upgrade** for Storage Gateway. This blog introduces a pattern combining **Terraform + Ansible** to migrate hundreds of gateways from AL2 to AL2023 while keeping cache data, Gateway ID intact, and reducing downtime from 1-3 days down to only **~1-2 hours**. It's a real-world case study on **Infrastructure-as-Code automation** at enterprise scale, helping DevOps teams avoid re-copying terabytes of data from S3.

---

### [Blog 2 - Automate medical record digitization with Amazon Bedrock Data Automation and AWS HealthLake](3.2-Blog2/)

Millions of paper medical records in hospitals are still being keyed in manually at a cost of **millions of USD/year** and an error rate of 5-15%. This blog analyzes a **serverless + event-driven** architecture combining **Amazon Bedrock Data Automation** (extracts 50+ clinical fields with AI, no training data needed) and **AWS HealthLake** (a HIPAA-eligible FHIR R4 data store) to convert scanned PDFs into standardized medical data in **~30 minutes per 1,000 records** instead of 2-3 weeks, at a cost of **<$0.50 per record** with an error rate of **<1%**.

---

### [Blog 3 - Lessons from the trenches: Solving automated contract intelligence with Doczy.ai™ on AWS](3.3-Blog3/)

AArete built **Doczy.ai™** — a contract-intelligence system running on AWS that processed **2.5 million contracts (~50 million pages)** in 22 months, reaching **99% accuracy** (vs. 55% for older rules-based systems), making **137 million Bedrock API calls** and saving customers **~$330M**. This blog walks through the **Textract + Bedrock + Smart Chunking** architecture (AArete's core patent) and **dual clustering** (semantic + structural) — a powerful pattern for legal documents that any team building a RAG system can learn from.