---
title: "Worklog Tuần 9"
date: 2026-06-07
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Mục tiêu tuần 9:
* Tìm hiểu Amazon Bedrock và các Foundation Model (Anthropic Claude, Meta Llama, Amazon Titan, Mistral, Cohere).
* Làm quen với các kỹ thuật Prompt Engineering, RAG, Fine-tuning và Bedrock Agents.
* Xây dựng ứng dụng Generative AI hoàn chỉết (chatbot RAG + Agent + Guardrails) và viết blog chia sẻ.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                                                                                                                                                                                                                                                            | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tổng quan Generative AI & LLM: <br>&emsp; + Khái niệm Foundation Model <br>&emsp; + Cách LLM hoạt động (tokenization, attention, transformer) <br>&emsp; + So sánh các họ LLM (Claude, Llama, Titan, Mistral) <br> - Tìm hiểu Amazon Bedrock: <br>&emsp; + Model access & Marketplace <br>&emsp; + Playgrounds (text, chat, image) <br>&emsp; + Converse API & InvokeModel <br>&emsp; + Pricing theo token in/out                                                                                                                                      | 08/06/2026   | 08/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Tìm hiểu Prompt Engineering: <br>&emsp; + Zero-shot, Few-shot prompting <br>&emsp; + Chain-of-thought (CoT) & ReAct <br>&emsp; + System prompt, role, format <br>&emsp; + Negative prompting, temperature, top_p <br>&emsp; + Output schema (JSON mode) <br> - **Thực hành:** Viết prompt để sinh bài blog, tóm tắt tài liệu, trích xuất JSON có cấu trúc, viết code Python                                                                                                                                                          | 09/06/2026   | 09/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Tìm hiểu Knowledge Base & RAG: <br>&emsp; + Khái niệm Retrieval Augmented Generation <br>&emsp; + Embedding model (Titan Embeddings v2) <br>&emsp; + Vector store: OpenSearch Serverless, Pinecone, S3 Vectors <br>&emsp; + Bedrock Knowledge Base tự động pipeline <br> - **Thực hành:** Upload tài liệu workshop vào S3, tạo Knowledge Base, chatbot hỏi đáp với Bedrock + Claude, đánh giá độ chính xác                                                                                                                                                       | 10/06/2026   | 10/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Tìm hiểu Bedrock Agents: <br>&emsp; + Action Groups (Lambda function calling) <br>&emsp; + Multi-step orchestration & planning <br>&emsp; + Session attributes <br>&emsp; + Knowledge Base association <br> - **Thực hành:** Tạo Agent đặt lịch họp, tự động gọi Lambda để check lịch rảnh và tạo event trong Calendar (Google Calendar API)                                                                                                                                                          | 11/06/2026   | 12/06/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Tìm hiểu Bedrock Guardrails & Responsible AI: <br>&emsp; + Denied topics <br>&emsp; + Content filters (hate, sexual, violence, prompt injection) <br>&emsp; + Sensitive information filters (PII redaction) <br>&emsp; + Word filters <br> - **Lab tổng hợp tuần 9:** <br>&emsp; + Chatbot hỏi đáp nội bộ dùng Bedrock + RAG + Guardrails <br>&emsp; + Frontend React + API Gateway + Lambda <br>&emsp; + Viết bài blog chia sẻ về Bedrock                                                                              | 12/06/2026   | 12/06/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Kết quả đạt được tuần 9:
* Nắm vững Generative AI trên AWS:
  * Hiểu Foundation Model, khả năng & giới hạn của từng họ model
  * Lựa chọn model phù hợp:
    * **Claude 3.5 Sonnet/Haiku:** reasoning mạnh, code tốt
    * **Amazon Titan Embeddings v2:** embedding văn bản, hỗ trợ 25+ ngôn ngữ
    * **Llama 3:** open-source, có thể tự host
    * **Cohere Command:** RAG chuyên dụng
    * **Stable Diffusion / Titan Image Generator:** tạo ảnh từ text

* Sử dụng thành thạo Amazon Bedrock:
  * Bedrock Playground test nhanh nhiều model, so sánh response
  * Gọi model qua AWS SDK (boto3) & CLI
  * Hiểu cơ chế token (input/output), latency, pricing
  * Converse API giúp switch model dễ dàng không cần đổi code
  * Provisioned Throughput cho workload production cần latency ổn định

* Thành thạo Prompt Engineering:
  * Áp dụng Zero/Few-shot, Chain-of-thought tuỳ bài toán
  * System prompt để định hình persona, format output
  * Ép model trả về JSON có cấu trúc để dễ tích hợp
  * Điều chỉnh `temperature` (0-1), `top_p`, `max_tokens` để cân chỉnh giữa sáng tạo và chính xác
  * Tối ưu prompt để giảm token và tăng chất lượng

* Xây dựng hệ thống RAG:
  * Upload tài liệu lên S3 (PDF, Markdown, HTML)
  * Tạo Bedrock Knowledge Base với OpenSearch Serverless
  * Document chunking & embedding tự động
  * Truy vấn có ngữ cảnh, giảm hallucination
  * Citation: trả về nguồn trích dẫn cho user

* Khám phá Bedrock Agents:
  * Action Groups gọi Lambda function
  * Agent tự lập kế hoạch & gọi tool để hoàn thành task
  * Session attributes lưu trữ context giữa các turn
  * Có thể kết hợp Knowledge Base + Action để Agent có cả tri thức & hành động

* Áp dụng Guardrails:
  * Cấu hình deny topics & content filter (Hate, Sexual, Violence, Misconduct)
  * PII redaction tuỳ chỉnh (ẩn email, số điện thoại)
  * Prompt attack detection chống injection
  * Đảm bảo ứng dụng an toàn & tuân thủ Responsible AI

* Hoàn thành chatbot RAG đầu tiên với Bedrock:
  * React frontend giao diện chat
  * API Gateway + Lambda gọi Bedrock KB
  * Guardrails lọc trước khi trả về user
  * CloudWatch theo dõi input/output tokens, latency

* Viết bài blog chia sẻ về Bedrock để giúp cộng đồng tiếp cận GenAI trên AWS.
* Thói quen mới:
  * Luôn test Bedrock response trên Playground trước khi tích hợp vào code
  * Caching response trong ElastiCache với key = hash(prompt+model) để giảm chi phí
  * Đánh giá chất lượng prompt với bộ test case cố định (accuracy, latency, cost)
  * Có guardrail mặc định cho mọi ứng dụng AI public
* ...