---
title: "Event 1"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Bài thu hoạch: FCAJ Community Day MAY 2026

## 1. Thông tin chung

| | |
|---|---|
| **Tên sự kiện** | FCAJ Community Day MAY 2026 |
| **Thời gian** | 08:30, ngày 23/05/2026 |
| **Địa điểm** | Tầng 26, Bitexco Financial Tower, 02 Hải Triều, Quận 1, TP. Hồ Chí Minh |
| **Đơn vị tổ chức** | Cộng đồng First Cloud AI Journey (FCAJ) cùng các đối tác AWS |
| **Vai trò của tôi** | Người tham dự (Attendee) |

## 2. Mục tiêu của sự kiện

FCAJ Community Day là chuỗi sự kiện offline định kỳ do cộng đồng **First Cloud AI Journey (FCAJ)** tổ chức, hướng đến các mục tiêu:

- Cập nhật các xu hướng **Cloud & Generative AI** mới nhất trên nền tảng AWS.
- Trình diễn các dịch vụ AI/Data tiêu biểu như **Amazon Quick Suite** (phân tích dữ liệu & workflow no-code) và **Amazon CloudFront** (CDN & bảo mật ở Edge).
- Chia sẻ bài học thực chiến từ các cuộc thi Hackathon — đặc biệt là những sản phẩm GenAI xây dựng dưới áp lực thời gian cực cao.
- Phân tích chuyên sâu kiến trúc **Multi-Agent đạt chuẩn Enterprise-Grade**, với bài toán thực tế trong ngành tài chính – ngân hàng.

## 3. Danh sách diễn giả

| # | Diễn giả | Vai trò | Đơn vị |
|---|---|---|---|
| 1 | Tinh Truong | Platform Engineer | GoTymeX |
| 2 | Anh Pham | Cloud Consultant | G-ASIAPACIFIC Vietnam |
| 3 | Thinh Nguyen | DevOps Engineer | FCAJ |
| 4 | Thao Nguyen, Mai Nguyen, Uyen Le | GenAI Engineers | Team VIB |
| 5 | Duc Dao | Solutions Architect | Cloud Kinetics |
| 6 | Vy Lam | Senior Business Systems Analyst | VPBank |

## 4. Tổng hợp nội dung các phiên

### 4.1. Context Is Everything – Making AI Actually Work for You *(Tinh Truong – GoTymeX)*

- **Vì sao nhiều dự án AI thất bại?** Không phải vì model yếu, mà vì thiếu **Context (bối cảnh) phù hợp**. Cùng một prompt nhưng cung cấp context khác nhau sẽ cho output chênh lệch rất lớn.
- **Sự tiến hóa tư duy:** từ "viết prompt hay" sang **quản lý bộ nhớ cho AI** (khái niệm *Second AI Brain*). Retrieval layer, memory state, agent state giúp AI "nhớ" được bối cảnh dài hạn.
- **Bí kíp thực tiễn:** cách tổ chức context để kết quả AI ổn định hơn, giảm hiện tượng hallucination.

### 4.2. Friendly AI Assistant with Amazon Quick *(Anh Pham)*

- **Quick Chat Agent** – trợ lý AI phân tích dữ liệu và trích xuất insight từ bảng biểu, dashboard.
- **Quick Flows** – dựng workflow tự động bằng ngôn ngữ tự nhiên, không yêu cầu viết code.
- **Quick Spaces & Quick Sight** – không gian làm việc nhóm để chia sẻ insight và tạo dashboard từ dữ liệu thô.

### 4.3. From Edge To Origin – CloudFront as Your Foundation *(Duc Dao – Cloud Kinetics)*

- **CDN cho mọi workload:** web, API, media, streaming đều có thể đi qua CloudFront.
- **Tối ưu chi phí:** caching thông minh và tiered pricing giúp giảm đáng kể chi phí truyền tải.
- **Độ tin cậy & bảo mật:** chống DDoS tốt hơn, tích hợp WAF, bảo vệ hệ thống từ lớp Edge.

### 4.4. 36 Hours with LotusHacks – Building UTMorpho from Idea to Reality *(Thinh Nguyen + team LotusHacks)*

- Hành trình từ con số 0 đến định nghĩa bài toán.
- **Sprint lập trình 36 giờ liên tục** – quy trình làm việc thực chiến trong hackathon.
- Bài học rút ra: khó khăn, thất bại, các **turning points** và cách demo sản phẩm cuối thuyết phục ban giám khảo.

### 4.5. Non-Determinism of "Deterministic" LLM Settings *(Duc Dao)*

- **Cơ chế LLM chọn token:** mô hình sinh ra token tiếp theo dựa trên xác suất, không phải rule cứng.
- **Sự thật về `Temperature = 0`:** không đảm bảo kết quả đồng nhất tuyệt đối — các inference optimization như KV-cache, batching, sampling tweaks vẫn ảnh hưởng output.
- **Chiến lược giảm thiểu:** dùng fixed seed, retries có kiểm soát, hoặc self-consistency decoding khi cần kết quả lặp lại.

### 4.6. Enterprise-Grade Multi-Agent System – Startup Credit Scoring *(Vy Lam – VPBank cùng team VIB)*

- **Bài toán ngân hàng:** dữ liệu tài chính của startup không khớp cấu trúc core-banking sẵn có.
- **Single Agent vs Multi-Agent:** khi nào dùng single agent (task đơn giản) và khi nào tách thành multi-agent (chia nhỏ phạm vi, kiểm soát hallucination tốt hơn).
- **Blueprint "Ủy ban tín dụng ảo":** multi-agent có **Guardrails chặt chẽ, Compliance rõ ràng và đo lường được Operational ROI**.

## 5. Ba trụ cột của một hệ AI cấp Enterprise-Grade

Tổng hợp từ các phiên hội thảo, mình rút ra **3 nguyên tắc cốt lõi** cho hệ AI trong doanh nghiệp:

| Trụ cột | Ý nghĩa | Ví dụ thực tế trong AWS |
|---|---|---|
| **Securely** | Bảo mật tuyệt đối, lọc PII & prompt injection | Bedrock Guardrails, WAF + CloudFront |
| **Reliably** | Ổn định, chịu lỗi tốt, luôn sẵn sàng | CloudFront CDN ở Edge, fallback cho LLM |
| **Scalably** | Mở rộng theo tải mà không phá vỡ kiến trúc | Tăng OCU OpenSearch, multi-region |

## 6. Ứng dụng vào đồ án thực tập FCAJ Internal Knowledge Assistant

- **CloudFront cho frontend:** đưa Amazon CloudFront vào kiến trúc triển khai React SPA, đạt chuẩn **Securely + Reliably**.
- **Guardrails hai đầu:** khi phát triển chatbot RAG trên **Bedrock + Knowledge Base**, lồng ghép lớp guardrails cho **cả input lẫn output** thay vì chỉ filter một phía.
- **Tinh thần "36 giờ sprint"** từ team LotusHacks: áp dụng cường độ tập trung cao cho các milestone 2 tuần của dự án.
- **Nghiên cứu thêm Amazon Quick Suite** để tự động hoá các tác vụ phân tích log và sinh dashboard vận hành.

## 7. Cảm nhận cá nhân

Một số điểm ấn tượng nhất với mình tại sự kiện:

- **Góc nhìn đa chiều** từ 6 diễn giả thuộc 6 mảng khác nhau (Platform, Cloud Consultant, DevOps, GenAI Engineer cho đến Solutions Architect và BSA ngân hàng). Mình cuối cùng cũng thấy được vòng đời của một sản phẩm AI đi từ hạ tầng tầng thấp lên đến bài toán doanh nghiệp tầng cao.
- **Bài chia sẻ của Vy Lam (VPBank)** để lại dấu ấn sâu nhất. Bài toán "Ủy ban tín dụng ảo" rất thực tế và thách thức: vừa phải thoả mãn quy trình tài chính khắt khe, vừa tận dụng được sức mạnh multi-agent.
- **Mindset hoàn toàn mới:** sản phẩm (như UTMorpho của LotusHacks) không chỉ là code, mà bắt đầu từ **đúng bài toán**, **đủ context**, và một **kiến trúc đủ mạnh để nâng đỡ ý tưởng**.
- **Phần quà và kết nối:** ngoài phần quà lưu niệm từ ban tổ chức, mình kết nối được với nhiều kỹ sư Cloud/AI trong cộng đồng FCAJ — rất hữu ích cho đồ án thực tập và cả công việc sau này.

## 8. Một số hình ảnh tại sự kiện

*Thêm ảnh của bạn tại đây*

> Nhìn chung, FCAJ Community Day MAY 2026 là một trải nghiệm mở mang tầm mắt — những kiến thức Cloud/AI mình học trong workshop trực tuyến được hiện thực hoá qua cách các kỹ sư hàng đầu triển khai hệ thống trong thực tế.
