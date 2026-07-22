---
title: "Các bài blogs đã đăng"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---

Trong 15 tuần thực tập, mình đã đăng **3 bài blog kỹ thuật chuyên sâu** trên [AWS Study Group](https://www.facebook.com/groups/awsstudygroupfcj) — phân tích các case study thực tế trên AWS, bao gồm tính năng mới của services, tối ưu chi phí bằng IaC, và sử dụng AI services để giải quyết bài toán doanh nghiệp cụ thể.

| # | Đề tài | Phạm trù (theo yêu cầu FCAJ) | Ngày đăng |
|---|---|---|---|
| Blog 1 | **Tự động hóa "thay máu" AWS Storage Gateway từ AL2 sang AL2023 bằng IaC** | Tối ưu vận hành & cost với Terraform + Ansible | 12/04/2026 |
| Blog 2 | **Tự động hóa số hóa bệnh án với Amazon Bedrock Data Automation + AWS HealthLake** | Sử dụng AI services để giải bài toán ngành y tế | 03/05/2026 |
| Blog 3 | **Giải bài toán đọc hợp đồng tự động với Doczy.ai™ trên AWS** | Tính năng mới: Textract + Bedrock + Smart Chunking | 07/06/2026 |

---

### [3.1 Blog 1](3.1-Blog1/)

**Amazon Linux 2 (AL2)** sắp tới hạn End-of-Support (06/2026) — và AWS **không hỗ trợ in-place upgrade** cho Storage Gateway. Bài blog này giới thiệu pattern kết hợp **Terraform + Ansible** để di chuyển hàng trăm gateway từ AL2 sang AL2023 mà vẫn giữ nguyên dữ liệu cache, Gateway ID, và giảm downtime từ 1-3 ngày xuống chỉ **~1-2 giờ**. Đây là case study thực tế về **tự động hoá vận hành (IaC)** ở quy mô enterprise, giúp team DevOps tránh phải copy lại hàng TB dữ liệu từ S3.

---

### [3.2 Blog 2](3.2-Blog2/)

Hàng triệu hồ sơ bệnh án giấy trong các bệnh viện đang được nhập liệu thủ công với chi phí **hàng triệu USD/năm** và tỉ lệ sai sót 5-15%. Bài blog này phân tích kiến trúc **serverless + event-driven** kết hợp **Amazon Bedrock Data Automation** (trích xuất 50+ trường lâm sàng bằng AI, không cần training data) và **AWS HealthLake** (kho dữ liệu FHIR R4 HIPAA-compliant) để chuyển đổi PDF scan thành dữ liệu y tế chuẩn hóa trong **~30 phút/1000 bệnh án** thay vì 2-3 tuần, với chi phí **<$0.50/bệnh án** và tỉ lệ lỗi **<1%**.

---

### [3.3 Blog 3](3.3-Blog3/)

AArete đã xây dựng **Doczy.ai™** — hệ thống contract intelligence chạy trên AWS xử lý **2,5 triệu hợp đồng (≈50 triệu trang)** trong 22 tháng, đạt **độ chính xác 99%** (so với 55% của hệ thống rules-based cũ), gọi **137 triệu lần API tới Bedrock** và tiết kiệm **~330 triệu USD** cho khách hàng. Bài blog phân tích chi tiết kiến trúc **Textract + Bedrock + Smart Chunking** (sáng chế cốt lõi của AArete) và **dual clustering** (semantic + structural) — pattern mạnh cho tài liệu pháp lý mà bất kỳ team nào xây hệ thống RAG đều có thể học hỏi.