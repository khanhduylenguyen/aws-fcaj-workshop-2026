---
title: "Sharing and Feedback"
date: 2026-07-06
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

> Here I candidly share my experience participating in the **First Cloud AI Journey (FCAJ) Workshop — 12-week cohort**. I hope my feedback helps the FCAJ team improve the program for future cohorts.

## Overall Evaluation

### 1. Working Environment & Format

The workshop ran in a **hybrid format (both online and offline)**: kickoff sessions, end-of-sprint demos and final defense took place offline at an AWS venue, while weekly labs and standup meetings were organized online via **Zoom + Discord**.

**Strengths:**

* **Discord workspace** was very well organized — channels like `#general`, `#bedrock`, `#lambda-help`, `#random`… meant I could ping the right person for help within minutes.
* **Offline facilities** (meeting room, large display, whiteboards) were well prepared, with coffee + snacks for offline sessions.
* **Quiet, focused workspace** without rigid schedule pressure — great for deep work.

**Weaknesses:**

* Some weekend offline sessions ran 6–8 hours; for members traveling from afar, this was tiring.
* **Offline room schedule** wasn't fixed in advance — sometimes the room changed right before the meeting.
* When only one mentor was physically present, groups had to wait their turn for help.

### 2. Mentor / Team Admin Support

This is **the thing I liked most** about the FCAJ workshop.

* Mentors shared real industry experience — they didn't just answer **"how"** but also explained **"why we should do it this way"**, especially around IAM least-privilege, AWS cost, and clean-up (in the spirit of "no resource left behind").
* The admin team responded quickly on Discord (avg **< 30 minutes** during business hours, **< 2 hours** outside).
* Mentors **encouraged me to debug first before asking** — this built my habit of reading CloudWatch logs, debugging IAM policies, and reading AWS documentation directly. These are crucial habits for real jobs.
* Admins provided **dedicated AWS accounts** for each team with **budget alerts** pre-configured — making cost control much easier.

### 3. Relevance of Work to Academic Background

The workshop followed a **7-chapter roadmap that is highly relevant to real enterprise work**:

1. Workshop Overview – introduction to RAG, Bedrock
2. Prerequisites – environment setup
3. **Knowledge Base** – building RAG pipeline with S3 + OpenSearch Serverless
4. **Frontend & API** – ReactJS + API Gateway + Lambda
5. **Bedrock Guardrails** – Responsible AI
6. Cleanup – resource cleanup (an important FinOps lesson)

For me (CS major), this workshop was **a perfect fit** because:

* My foundational knowledge (Java/Python, React, Docker, Git) was **reused end-to-end** throughout the workshop.
* It also expanded into areas school never touched: **RAG pattern, vector database, prompt engineering, Bedrock Guardrails, FinOps**.
* The clear roadmap gave me a **complete "cloud-native roadmap"** I can pursue after graduation.

### 4. Learning & Skill Development Opportunities

In 12 weeks, I **unlocked** a lot of new skills:

* **Technical skills**: setting up & operating a Bedrock Knowledge Base, writing a Lambda chat handler, configuring OpenSearch Serverless collections, setting up Guardrails to filter PII.
* **Soft skills**: writing bilingual technical reports (VI + EN), collaborating via Git Flow, presenting to mentors, writing 3 blog posts for the workshop site.
* **Cloud-native mindset**: understanding why to use serverless over traditional EC2, why we must clean up resources, why we need Guardrails when deploying LLMs.
* **Career path**: mentors shared personal experience about becoming a **Cloud Architect / AI Engineer**, plus a reasonable AWS certification roadmap (SAA → Specialty), which helped me plan my future more clearly.

### 5. Company Culture & Team Spirit

* My team had **5 members from 4 different universities**, and we coordinated seamlessly thanks to **Git Flow + Jira + Discord**.
* **No-judgment culture for "dumb" questions** — everyone felt comfortable asking basic questions without fear of being looked down on. This is **incredibly important** for newcomers.
* When our team got stuck on `OpenSearch Serverless network policy`, **other teams voluntarily jumped in to help** even though it wasn't their job — that's the true **"Build Together"** spirit of the FCAJ community.
* **Lightning talks at the end of each week** were presented by members themselves (not mentors), helping everyone **practice public speaking** without pressure.

### 6. Internship Policies / Benefits

* **Dedicated AWS account** with a **$200/month/team** budget — with budget alerts at 50%, 80%, 100%.
* **AWS Skill Builder subscription** free during the workshop.
* **AWS Certification voucher** (SAA-C03) for members who finish the workshop well.
* **Certificate of Completion** verified by AWS — a strong boost for the CV.
* However, there is **no cash allowance** — the only point I'd flag for improvement, especially if FCAJ wants to attract students from far-away provinces.

---

## Additional Questions

### What I was most satisfied with during the workshop?

**The FCAJ community and mentor support.**

This isn't a "safe" answer. Before joining the workshop, I had taken another online course about AWS + Bedrock, but when I got stuck I'd have to wait 3–5 days for an email reply. At FCAJ, when I'm stuck, I ping Discord → within minutes **another member** (not a mentor, just someone from another team) jumps in to help. That feeling of **"no one is left behind"** is what I appreciate most.

Additionally, the chance to **write technical blogs** forced me to revisit what I'd learned more deeply. Every time I wrote a blog, I had to read the original AWS docs, reference official blogs at `aws.amazon.com/blogs/`, and redraw the architecture diagram → I ended up understanding the topic **3x deeper** than if I'd just read the slide deck.

### What I think FCAJ should improve for future cohorts?

**1. AWS cost issue — need a clearer dashboard & FinOps guide for newcomers**

This is the point I want to raise most strongly, because it affects the actual experience of quite a few people in my team.

Specifically:

* At the beginning, each team's AWS account got a **$200/month** budget. Sounds like a lot, but in practice:

  * **OpenSearch Serverless** in `production` mode charges **OCU (OpenSearch Compute Unit)** — if you accidentally leave it on overnight, it can burn **$15-30** in one night.
  * **Bedrock Knowledge Base** keeps syncing with S3 — every sync re-embeds documents using Titan Embeddings, which can easily cost **$5-10/day** if you update docs frequently.
  * **API Gateway + Lambda** is cheap, but if you call Bedrock `InvokeModel` without caching, the **output tokens** of Claude 3.5 Sonnet burn through budget fast.

* My team **hit the budget alert twice** during weeks 6-7 just because we forgot to stop the OpenSearch collection when not using it.

* Suggestions for improvement:

  * Provide a **FinOps starter guide** in week 1, teaching how to use **AWS Cost Explorer + AWS Budgets + Cost Anomaly Detection**.
  * Require every service to be tagged with `WorkshopAutoCleanup = true` and **auto-stop after 8 hours of inactivity**.
  * Appoint a **"Cost Champion"** in each team — responsible for checking Cost Explorer every morning and pinging the team when spend exceeds $80.
  * Organize **one "FinOps clinic"** in week 4 — before teams build large Knowledge Bases.

**2. Vietnamese-language materials are still scarce**

Most official AWS docs are in English. My team has a member who's new to Bedrock; reading the original docs was slow. A **Vietnamese cheat-sheet** for the main services (Bedrock, Lambda, S3, IAM, CloudWatch) would be hugely helpful.

**3. Sprint pacing feels slightly rushed**

Weeks 5-8 (Frontend + API + Guardrails) are quite packed. Adding **1 buffer week** between sprints would let teams **clean up code + rewrite documentation** before moving on.

**4. No track for those who already have AWS experience**

Members who already have SAA or have worked with Bedrock found chapters 1-2 a bit "slow". We could add an **advanced track** (optional) for deeper work on CDK/Terraform, multi-region, and advanced cost optimization.

### If I recommend to a friend, would I suggest they join FCAJ?

**Yes, absolutely yes.** Here's why:

1. **It's free**, but you get to work with real AWS services — this is rare in Vietnam.
2. **High-quality community** — mentors from AWS, fellow learners who all have clear learning goals. Learning with a strong group pulls you up.
3. **You build a real product**, not "hello world". After 12 weeks I had a working RAG chatbot on AWS — an **incredibly valuable asset** for my CV and personal portfolio.
4. **Certificate + AWS Certification voucher** is a worthy reward.
5. The "no one left behind" culture is real, not a slogan — that's hard to find in online courses.

**However**, I'd warn my friends that:

* The workshop demands **15-20 hours per week** — this isn't a casual walk.
* You should have a foundation in **Java/Python + Git + Linux basics** before joining, or you'll struggle with the labs.
* Be ready to **read a lot of English-language material** — the workshop is in Vietnamese, but 99% of the docs are in English.

---

## Suggestions & Expectations

### Suggestions for improving the internship experience

1. **FinOps starter pack**: Add a 2-hour **AWS cost management** module in week 1, teaching **AWS Budgets, Cost Explorer, Cost Anomaly Detection**, **resource tagging convention**, and **automated cleanup with scheduled Lambda**. This is a skill missing in many freshers when they start their first job.
2. Provide **Vietnamese cheat-sheets** for the main services (Bedrock, Lambda, S3, IAM, CloudWatch, OpenSearch Serverless).
3. Add an **advanced track** for those with existing foundations — deeper work on CDK/Terraform, multi-region, advanced cost optimization.
4. **Extend the Frontend + API phase by 1-2 weeks**, or add **a polish + docs week** before the Guardrails sprint.
5. **Expand mentor coverage**: currently there's only one Bedrock mentor, so groups asking about CDK/Terraform get slower support.
6. **Organize mock presentations** before the final defense — help members **rehearse and get feedback** on slide decks and delivery.
7. **Public blog system**: let members **republish their blogs on personal Medium / dev.to** (with FCAJ credit) — an effective way to **build personal brand**.
8. **Maintain post-workshop connection**: create an **FCAJ Alumni Discord channel** so alumni can support new cohorts and share AWS-related job openings.

### Would I like to continue the program in the future?

**Yes, very much.** Specifically:

* Join the **FCAJ Alumni network** to stay connected with the community and receive **AWS-related job openings**.
* Sign up as a **co-mentor** for future cohorts — share my real-world experience, especially around **FinOps & cost optimization** (because I've "burned budget" so I'm quite good at this 😂).
* Continue learning **AWS Specialty certifications** (aiming for **Machine Learning Specialty** and **Security Specialty**) and share the knowledge back through the workshop.
* Keep **writing technical blog posts** on the workshop site — topics I'd like to cover next: **multi-region Bedrock deployment**, **cost optimization patterns**, and **Bedrock Agent with Action Groups deep-dive**.

### Other Comments (Free Sharing)

* The workshop gave me a clear answer to **"what should an IT student learn after graduation?"** — namely, **mastering Cloud + AI/ML on AWS**. Thank you, FCAJ, for helping me find my direction.
* One of the biggest lessons I'll carry with me: **"no resource is truly free on the cloud, even on a trial account."** FCAJ taught me that lesson **very early on** by requiring resource cleanup at the end of every learning session.
* I consolidated all the knowledge in **3 technical blog posts** published on the workshop site (Blog 1: Textract + unstructured PDF, Blog 2: Bedrock Data Automation + HealthLake, Blog 3: Doczy.ai™ on AWS). Anyone who wants to dive deeper into AWS document processing should read those three for a comprehensive overview.
* Final note: **FCAJ has changed my learning mindset** — from "study for exams" to "study to **build real products**". That's the core value a student workshop should have. **Keep that DNA, FCAJ team!** 🚀