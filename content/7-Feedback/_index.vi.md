---
title: "Chia sẻ, đóng góp ý kiến"
date: 2026-07-06
weight: 7
chapter: false
pre: " <b> 7. </b> "
---
> Tại đây mình chia sẻ thẳng thắn về trải nghiệm tham gia chương trình **First Cloud AI Journey (FCAJ) Workshop 12 tuần**, kỳ vọng giúp team FCAJ cải thiện những điểm còn thiếu sót dựa trên các hạng mục sau:

## Đánh giá chung

### 1. Môi trường làm việc & hình thức tổ chức

Workshop được tổ chức theo mô hình **hybrid (vừa online vừa offline)** khá linh hoạt: các buổi kickoff, demo cuối sprint và bảo vệ báo cáo diễn ra offline tại địa điểm của AWS, các buổi lab hàng tuần và standup meeting tổ chức online qua **Zoom + Discord**.

**Điểm tích cực:**

* **Discord workspace** được tổ chức rất tốt — chia theo kênh `#general`, `#bedrock`, `#lambda-help`, `#random`… nên khi gặp vấn đề kỹ thuật có thể ping đúng người hỗ trợ chỉ trong vài phút.
* **Cơ sở vật chất offline** (phòng họp, màn hình lớn, bảng trắng) được chuẩn bị chu đáo, có cà phê + snack cho buổi offline.
* **Không gian làm việc yên tĩnh**, không bị áp lực giờ giấc cứng nhắc, tạo điều kiện tập trung cao.

**Hạn chế:**

* Một số buổi offline cuối tuần khá dài (6–8 tiếng), nếu thành viên ở xa thì di chuyển mệt mỏi.
* Chưa có lịch cố định cho **phòng họp offline** trước — đôi khi phải đổi phòng sát giờ.
* Khi mentor trực tiếp chỉ có 1 người, các nhóm phải xếp hàng hỏi khá lâu.

### 2. Sự hỗ trợ của mentor / team admin

Đây là **điểm mình hài lòng nhất** của FCAJ workshop.

* Mentor giàu kinh nghiệm thực tế, không chỉ trả lời **"làm thế nào"** mà còn giải thích **"tại sao nên làm thế này"** — đặc biệt là các best-practice về bảo mật IAM least-privilege, chi phí AWS, và clean-up tài nguyên (đúng tinh thần "no resource left behind").
* Team admin phản hồi nhanh trên Discord (trung bình **< 30 phút** trong giờ hành chính, **< 2 tiếng** ngoài giờ).
* Mentor **khuyến khích mình tự debug trước khi hỏi** — điều này giúp mình hình thành thói quen đọc log CloudWatch, debug IAM policy, đọc AWS documentation gốc. Đây là kỹ năng cực kỳ quan trọng khi đi làm thật.
* Admin hỗ trợ **AWS account riêng** cho từng nhóm với **budget alert** cài sẵn — giúp nhóm kiểm soát chi phí dễ dàng.

### 3. Sự phù hợp giữa công việc và chuyên ngành học

Workshop được thiết kế theo lộ trình **7 chương rất sát với thực tế doanh nghiệp**:

1. Workshop Overview – giới thiệu RAG, Bedrock
2. Prerequisites – chuẩn bị môi trường
3. **Knowledge Base** – xây dựng pipeline RAG với S3 + OpenSearch Serverless
4. **Frontend & API** – ReactJS + API Gateway + Lambda
5. **Bedrock Guardrails** – Responsible AI
6. Cleanup – dọn dẹp tài nguyên (bài học quan trọng về FinOps)

Đối với mình (chuyên ngành CNTT), workshop này **rất phù hợp** vì:

* Kiến thức nền tảng (Java/Python, React, Docker, Git) **được dùng lại nguyên bộ** trong workshop.
* Mở rộng thêm các mảng hoàn toàn mới ở trường chưa dạy: **RAG pattern, vector database, prompt engineering, Bedrock Guardrails, FinOps**.
* Cách trình bày lộ trình rõ ràng giúp mình thấy được **một "cloud-native roadmap" hoàn chỉnh** mà mình có thể theo đuổi sau khi tốt nghiệp.

### 4. Cơ hội học hỏi & phát triển kỹ năng

Trong 12 tuần, mình đã **"mở khóa"** được rất nhiều kỹ năng:

* **Kỹ thuật chuyên môn**: cài đặt & vận hành Knowledge Base trên Bedrock, viết Lambda function xử lý chat request, config OpenSearch Serverless collection, thiết lập Guardrails filter PII.
* **Kỹ năng mềm**: viết báo cáo kỹ thuật song ngữ (VI + EN), làm việc nhóm qua Git Flow, demo trước mentor, viết 3 bài blog chia sẻ trên workshop site.
* **Tư duy Cloud-native**: hiểu được vì sao nên dùng serverless thay vì EC2 truyền thống, vì sao phải cleanup tài nguyên, vì sao phải có Guardrails khi triển khai LLM.
* **Career path**: mentor đã chia sẻ kinh nghiệm cá nhân về con đường trở thành **Cloud Architect / AI Engineer**, lộ trình chứng chỉ AWS hợp lý (SAA → Specialty), giúp mình định hướng rõ hơn.

### 5. Văn hóa & tinh thần đồng đội

* Nhóm mình có **5 thành viên** đến từ 4 trường đại học khác nhau, dù vậy vẫn phối hợp rất ăn ý nhờ **Git Flow + Jira + Discord**.
* Văn hóa **không phán xét khi hỏi "ngớ ngẩn"** — ai cũng thoải mái đặt câu hỏi cơ bản mà không sợ bị chê. Điều này **cực kỳ quan trọng** với người mới.
* Khi nhóm mình bị stuck trong phần `OpenSearch Serverless network policy`, các nhóm khác **tự nguyện vào hỗ trợ** dù không phải việc của họ — đúng tinh thần **"Build Together"** của cộng đồng FCAJ.
* Các buổi **lightning talk cuối tuần** do chính thành viên trình bày (không phải mentor) giúp mọi người **rèn kỹ năng public speaking** mà không áp lực.

### 6. Chính sách / phúc lợi cho thực tập sinh

* **AWS account riêng** với budget $200/tháng cho mỗi nhóm — kèm budget alert khi đạt 50%, 80%, 100%.
* **AWS Skill Builder subscription** miễn phí trong thời gian workshop.
* **Voucher thi AWS Certification** (SAA-C03) cho các thành viên hoàn thành tốt workshop.
* **Certificate of Completion** có xác nhận từ AWS — giúp CV mình nổi bật hơn khi xin việc.
* Tuy nhiên, **không có phụ cấp tiền mặt** — đây là điểm duy nhất mình thấy có thể cải thiện để thu hút thêm sinh viên từ các tỉnh xa.

---

## Một số câu hỏi khác

### Điều mình hài lòng nhất trong thời gian tham gia workshop?

**Cộng đồng FCAJ và sự hỗ trợ của mentor.** 

Đây không phải câu trả lời "an toàn". Trước khi vào workshop, mình từng tham gia một khóa học online khác với nội dung cũng về AWS + Bedrock, nhưng khi stuck thì chỉ có thể đợi email phản hồi 3–5 ngày. Ở FCAJ, mình stuck → ping Discord → trong vài phút có người (không phải mentor mà là thành viên nhóm khác) vào giúp. Cảm giác **"không ai bị bỏ lại phía sau"** là điều mình trân trọng nhất.

Ngoài ra, cơ hội **viết blog kỹ thuật** giúp mình quay lại kiến thức đã học theo cách sâu hơn. Mỗi lần viết blog, mình phải đọc docs gốc từ AWS, tham khảo blog chính thức từ `aws.amazon.com/blogs/`, và tự vẽ lại sơ đồ kiến trúc → hiểu bài **gấp 3 lần** so với chỉ đọc slide.

### Điều mình nghĩ FCAJ cần cải thiện cho các kỳ sau?

**1. Vấn đề chi phí AWS — cần có dashboard & hướng dẫn FinOps rõ ràng hơn cho người mới**

Đây là điểm mình muốn **lên tiếng mạnh nhất**, vì nó ảnh hưởng đến trải nghiệm thực tế của khá nhiều bạn trong nhóm mình.

Cụ thể:

* Khi mới bắt đầu, AWS account được cấp budget **$200/tháng/nhóm**. Nghe có vẻ nhiều, nhưng thực tế:

  * **OpenSearch Serverless** ở chế độ `production` tính phí **OCU (OpenSearch Compute Unit)** — nếu vô tình bật rồi quên tắt, sau 1 đêm có thể "bay" **$15-30**.
  * **Bedrock Knowledge Base** sync liên tục với S3 — mỗi lần sync embed lại tốn token Titan Embeddings, có thể lên **$5-10/ngày** nếu cập nhật docs thường xuyên.
  * **API Gateway + Lambda** lại rẻ, nhưng nếu gọi Bedrock `InvokeModel` không có caching thì **token đầu ra** của Claude 3.5 Sonnet dễ "đốt" budget nhanh.

* Mình và nhóm đã từng bị **budget alert kích hoạt 2 lần** trong tuần 6-7 chỉ vì quên tắt OpenSearch collection khi không dùng.

* Đề xuất cải thiện:

  * Cung cấp **FinOps starter guide** ngay từ tuần 1, dạy cách dùng **AWS Cost Explorer** + **AWS Budgets** + **Cost Anomaly Detection**.
  * Yêu cầu mọi service **phải có tag `WorkshopAutoCleanup = true`** và tự động stop sau 8 tiếng không hoạt động.
  * Có **"cost champion" trong mỗi nhóm** — người chịu trách nhiệm check Cost Explorer mỗi sáng và ping nhóm khi chi phí vượt $80.
  * Tổ chức **1 buổi "FinOps clinic"** vào tuần 4 — trước khi nhóm build Knowledge Base lớn.

**2. Tài liệu tiếng Việt còn khá ít**

Phần lớn docs chính thức của AWS bằng tiếng Anh. Nhóm mình có bạn mới tiếp cận Bedrock lần đầu, đọc tài liệu gốc khá chậm. Một **cheat-sheet tiếng Việt** cho các dịch vụ chính (Bedrock, Lambda, S3, IAM, CloudWatch) sẽ giúp ích rất nhiều.

**3. Thời gian giữa các sprint hơi ngắn**

Tuần 5-8 (Frontend + API + Guardrails) khá dồn dồn. Nếu thêm **1 tuần buffer** giữa sprint thì nhóm sẽ có thời gian **cleanup code + viết lại documentation** trước khi sang sprint mới.

**4. Thiếu track cho người đã có kinh nghiệm AWS**

Những bạn đã có SAA hoặc đã làm với Bedrock thì thấy phần 1-2 hơi "slow". Có thể thêm **advanced track** (tuỳ chọn) để những bạn này làm sâu hơn.

### Nếu giới thiệu cho bạn bè, mình có khuyên họ tham gia FCAJ không?

**Có, chắc chắn có.** Lý do:

1. **Không cần trả tiền** mà được học trực tiếp các dịch vụ AWS thật — điều này hiếm có ở Việt Nam.
2. **Cộng đồng chất lượng cao** — mentor từ AWS, các bạn đồng môn đều có động lực học tập rõ ràng. Học cùng nhóm giỏi kéo mình lên theo.
3. **Có sản phẩm chạy thật** chứ không phải "hello world". Mình có một chatbot RAG thật trên AWS sau 12 tuần — đây là **asset cực kỳ giá trị** cho CV và portfolio cá nhân.
4. **Certificate + Voucher thi AWS** là phần thưởng xứng đáng.
5. Văn hoá "không ai bị bỏ lại" thật sự, không phải slogan — đây là điều khó tìm ở các khoá học online.

**Tuy nhiên**, mình sẽ lưu ý với bạn bè rằng:

* Cần **dành 15-20 tiếng/tuần** cho workshop, không phải dạo chơi.
* Nên có nền tảng **Java/Python + Git + Linux cơ bản** trước khi vào, nếu không sẽ hơi struggle với phần lab.
* Chuẩn bị tinh thần **học rất nhiều tài liệu tiếng Anh** — workshop bằng tiếng Việt nhưng docs thì 99% tiếng Anh.

---

## Đề xuất & mong muốn

### Đề xuất cải thiện trải nghiệm thực tập

1. **FinOps starter pack**: Thêm 1 module ~2 tiếng về **AWS cost management** ngay từ tuần 1, dạy cách dùng **AWS Budgets, Cost Explorer, Cost Anomaly Detection**, **resource tagging convention**, và **tự động cleanup với Lambda scheduled**. Đây là kỹ năng thiếu hụt ở rất nhiều fresher khi đi làm.
2. **Cung cấp cheat-sheet tiếng Việt** cho các service chính (Bedrock, Lambda, S3, IAM, CloudWatch, OpenSearch Serverless).
3. **Thêm "advanced track"** cho người đã có nền tảng — có thể làm sâu về CDK/Terraform, multi-region, cost optimization nâng cao.
4. **Kéo dài thêm 1-2 tuần** ở phần Frontend + API, hoặc thêm **1 tuần polish + docs** trước khi sang sprint Guardrails.
5. **Mở rộng phạm vi mentor**: hiện tại mentor Bedrock chỉ có 1 người, nếu nhóm nào hỏi nhiều về CDK/Terraform sẽ khó nhận support.
6. **Tổ chức mock presentation** trước khi bảo vệ cuối kỳ — giúp thành viên **rehearse và nhận feedback** về slide deck + cách trình bày.
7. **Public blog system**: cho phép thành viên **chia sẻ bài blog lên Medium / dev.to cá nhân** (có ghi credit FCAJ) — đây là cách giúp thành viên **xây dựng personal brand** rất hiệu quả.
8. **Sau workshop vẫn giữ kết nối**: tạo **FCAJ Alumni Discord channel** để các kỳ trước hỗ trợ các kỳ sau, chia sẻ job opening AWS-related.

### Mong muốn tiếp tục chương trình trong tương lai?

**Có, mình rất muốn tiếp tục.** Cụ thể:

* **Tham gia FCAJ Alumni network** để duy trì kết nối với cộng đồng và nhận thông tin **job opening AWS-related**.
* **Đăng ký làm "co-mentor"** cho các kỳ sau — chia sẻ lại kinh nghiệm thực chiến, đặc biệt về **FinOps & cost optimization** (vì mình "đã cháy budget" nên rất thành thạo 😂).
* **Tiếp tục học các AWS Specialty** (đang nhắm **Machine Learning Specialty** và **Security Specialty**) và chia sẻ lại kiến thức qua workshop.
* **Viết tiếp blog kỹ thuật** trên workshop site — đề tài mình muốn viết tiếp gồm: **multi-region Bedrock deployment**, **cost optimization patterns**, và **phân tích Bedrock Agent với Action Groups**.

### Góp ý khác (tự do chia sẻ)

* Workshop đã cho mình một câu trả lời rõ ràng cho câu hỏi **"sinh viên IT nên học gì sau tốt nghiệp?"** — đó là **làm chủ Cloud + AI/ML trên AWS**. Cảm ơn FCAJ đã giúp mình định hướng.
* Một trong những bài học lớn nhất mà mình mang đi: **"không có tài nguyên nào là miễn phí trên cloud, kể cả account trial".** Đây là bài học FCAJ dạy mình **rất sớm** qua việc yêu cầu cleanup tài nguyên cuối mỗi buổi học.
* Mình đã **tổng hợp lại toàn bộ kiến thức** trong **3 bài blog kỹ thuật** được đăng trên workshop site (Blog 1: Textract + unstructured PDF, Blog 2: Bedrock Data Automation + HealthLake, Blog 3: Doczy.ai™ on AWS). Nếu bạn nào muốn tìm hiểu sâu hơn về AWS document processing pipeline thì đọc 3 bài đó là đủ để có overview toàn diện.
* Góp ý cuối: **FCAJ đã thay đổi mindset học tập của mình** — từ "học để thi" sang "học để **build sản phẩm thật**". Đây là giá trị cốt lõi mà một workshop sinh viên nên có. **Giữ vững DNA đó nhé team FCAJ!** 🚀