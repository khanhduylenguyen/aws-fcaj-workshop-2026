---
title: "Week 12 Worklog"
date: 2026-06-28
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---
### Week 12 Objectives:
* Polish the Capstone based on last week's feedback.
* Hold the final demo for mentors and the FCAJ community.
* Wrap up the whole 12-week internship, complete the report, and update the portfolio.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                       | Start Date | Completion Date | Reference Material                        |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Address last week's feedback: <br>&emsp; + Improve UX (upload flow, loading state, error message) <br>&emsp; + Enable MFA for the Cognito User Pool <br>&emsp; + Cache Bedrock responses in ElastiCache (Redis) <br> - Optimise performance: <br>&emsp; + Provisioned Concurrency for hot-path Lambdas <br>&emsp; + CloudFront caching for static assets & API responses <br>&emsp; + Tighter Athena queries (partition + Parquet)                                           | 06/29/2026 | 06/29/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Complete the personal workshop on GitHub: <br>&emsp; + Add illustrations (architecture diagrams, console screenshots) for each week <br>&emsp; + Fix broken links and standardise Markdown formatting <br>&emsp; + Ensure Hugo builds successfully and deploy to GitHub Pages <br>&emsp; + Add a "References" section to each week                                                                                                                                       | 06/30/2026 | 06/30/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Hold the final demo: <br>&emsp; + Schedule with mentor & the FCAJ community <br>&emsp; + Prepare slides (problem, solution, architecture, demo, results, lessons learned) <br>&emsp; + 5-10 minute demo script + backup video <br>&emsp; + Anticipated Q&A and prepared answers                                                                                                                                                                                          | 07/01/2026 | 07/02/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Aggregate numbers & write the final report: <br>&emsp; + Summarise the 12 weeks of knowledge (skill matrix) <br>&emsp; + Self-evaluation (strengths, weaknesses) <br>&emsp; + Development roadmap for the next steps (AWS certifications, career path) <br>&emsp; + Acknowledgements to mentors and peers                                                                                                                                                                          | 07/02/2026 | 07/03/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Submit the report and collect mentor feedback <br> - Join the FCAJ program closing session <br> - Share experience with the next cohort <br> - Update CV & portfolio with the Capstone project & AWS certifications earned <br> - Archive source code and docs on GitHub for future reference                                                                                                                                                                                            | 07/03/2026 | 07/03/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 12 Achievements:
* Finalised the Capstone project:
  * Applied feedback to improve UX (multi-step upload, skeleton loading, toast notifications)
  * Enabled MFA for Cognito User Pool, strengthening security
  * Cached Bedrock responses in ElastiCache (cut ~40% token cost)
  * Provisioned Concurrency for hot-path Lambdas — cold start down from 3s to 200ms
  * Optimised Athena queries with partitioning + Parquet — query cost down 80%

* Completed the personal workshop on GitHub:
  * Hugo built successfully and deployed to GitHub Pages
  * All 12 weeks have illustrations (architecture diagrams, console screenshots)
  * Both VI/EN languages in sync
  * Each week has a "References" section with official AWS links
  * A top-level README makes it easy for newcomers to follow the journey

* Final demo successful:
  * Presented to mentors and the FCAJ community (30+ attendees)
  * Product ran stably with no major bugs
  * Backup video on hand for network failures
  * Answered audience questions on architecture, cost, and security confidently
  * Received positive feedback on feasibility and real-world applicability

* Completed the internship report:
  * **Summary of 12 weeks of knowledge:**
    * Weeks 1-3: Cloud fundamentals (EC2, S3, IAM, VPC, RDS, CloudWatch)
    * Weeks 4-6: Serverless & Containers (Lambda, API Gateway, ECS, CI/CD)
    * Weeks 7-9: AI/ML (Rekognition, Comprehend, SageMaker, Bedrock)
    * Week 10: Security & Cost Optimization
    * Weeks 11-12: Capstone & Demo
  * **Self-evaluation:**
    * Strengths: independent learner, mastered the AWS ecosystem, good at debugging
    * Areas to improve: advanced prompt engineering, MLOps, technical English
  * **Development roadmap:**
    * Sit the AWS Solutions Architect Associate exam
    * Deepen MLOps & LLM fine-tuning knowledge
    * Contribute to the AWS Vietnam community (blogging, mentoring future cohorts)
  * **Acknowledgements:** mentors, teammates, the FCAJ program team, AWS Vietnam

* Joined the program closing session and shared experience with the next cohort.
* Updated CV & portfolio, ready for the next career opportunity.

* **Lessons learned across 12 weeks:**
  1. **Learn by doing:** labs stick better than theory
  2. **Read original AWS docs:** always up-to-date and accurate
  3. **Ask when stuck:** mentors & the FCAJ community are always ready
  4. **Build your own project:** the personal workshop consolidates knowledge
  5. **Manage cost from day one:** always set budgets and audit resources weekly
  6. **Security by default:** it's not a feature you bolt on later
  7. **IaC & CI/CD from the start:** avoids technical debt later
  8. **AI is a tool, not a replacement for thinking:** understand the use case first

* Habits carried forward after the internship:
  * Start every new project with an architecture diagram & cost estimate
  * Write a blog post about every new piece of knowledge to consolidate it
  * Update the CV quarterly with new achievements
  * Aim for at least one AWS certification per year
  * Join the AWS User Group community in HCM City
* ...