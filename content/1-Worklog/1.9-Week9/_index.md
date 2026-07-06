---
title: "Week 9 Worklog"
date: 2026-06-07
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
### Week 9 Objectives:
* Explore Amazon Bedrock and leading Foundation Models (Anthropic Claude, Meta Llama, Amazon Titan, Mistral, Cohere).
* Learn Prompt Engineering, RAG, Fine-tuning and Bedrock Agents.
* Build a complete Generative AI app (RAG chatbot + Agents + Guardrails) and write a sharing blog.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                            | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Generative AI & LLM overview: <br>&emsp; + Foundation Model concepts <br>&emsp; + How LLMs work (tokenization, attention, transformer) <br>&emsp; + Compare LLM families (Claude, Llama, Titan, Mistral) <br> - Learn Amazon Bedrock: <br>&emsp; + Model access & Marketplace <br>&emsp; + Playgrounds (text, chat, image) <br>&emsp; + Converse API & InvokeModel <br>&emsp; + Pricing per input/output token                                                        | 06/08/2026 | 06/08/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn Prompt Engineering: <br>&emsp; + Zero-shot, Few-shot prompting <br>&emsp; + Chain-of-thought (CoT) & ReAct <br>&emsp; + System prompt, role, format <br>&emsp; + Negative prompting, temperature, top_p <br>&emsp; + Output schema (JSON mode) <br> - **Practice:** Write prompts for blog writing, document summarisation, structured JSON extraction, Python code generation                                                                                                                                                              | 06/09/2026 | 06/09/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Knowledge Base & RAG: <br>&emsp; + RAG concept <br>&emsp; + Embedding model (Titan Embeddings v2) <br>&emsp; + Vector stores: OpenSearch Serverless, Pinecone, S3 Vectors <br>&emsp; + Bedrock Knowledge Base auto pipeline <br> - **Practice:** Upload workshop docs to S3, create a Knowledge Base, build a Q&A chatbot with Bedrock + Claude, evaluate accuracy                                                                                                                                                       | 06/10/2026 | 06/10/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn Bedrock Agents: <br>&emsp; + Action Groups (Lambda function calling) <br>&emsp; + Multi-step orchestration & planning <br>&emsp; + Session attributes <br>&emsp; + Knowledge Base association <br> - **Practice:** Create an Agent that schedules meetings, calls Lambda to check free time and creates events in Calendar (Google Calendar API)                                                                                                                                                                  | 06/11/2026 | 06/12/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - Learn Bedrock Guardrails & Responsible AI: <br>&emsp; + Denied topics <br>&emsp; + Content filters (hate, sexual, violence, prompt injection) <br>&emsp; + Sensitive information filters (PII redaction) <br>&emsp; + Word filters <br> - **Week 9 capstone lab:** <br>&emsp; + Internal Q&A chatbot using Bedrock + RAG + Guardrails <br>&emsp; + React frontend + API Gateway + Lambda <br>&emsp; + Write a Bedrock sharing blog                                                                                                          | 06/12/2026 | 06/12/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 9 Achievements:
* Mastered Generative AI on AWS:
  * Understood Foundation Model capabilities and limits for each family
  * Selected the right model for each task:
    * **Claude 3.5 Sonnet/Haiku:** strong reasoning, code generation
    * **Amazon Titan Embeddings v2:** text embeddings, supporting 25+ languages
    * **Llama 3:** open-source, self-hostable
    * **Cohere Command:** RAG specialised
    * **Stable Diffusion / Titan Image Generator:** text-to-image

* Used Amazon Bedrock proficiently:
  * Bedrock Playground tested multiple models quickly and compared responses
  * Called models via AWS SDK (boto3) and CLI
  * Understood tokens (input/output), latency and pricing
  * Converse API made it easy to switch models without changing code
  * Provisioned Throughput for production workloads requiring stable latency

* Mastered Prompt Engineering:
  * Applied Zero/Few-shot, Chain-of-thought depending on the problem
  * System prompts to define persona and output format
  * Forced structured JSON output for easy integration
  * Tuned `temperature` (0-1), `top_p`, `max_tokens` to balance creativity vs accuracy
  * Optimised prompts to reduce tokens and improve quality

* Built a RAG system:
  * Uploaded documents (PDF, Markdown, HTML) to S3
  * Created a Bedrock Knowledge Base with OpenSearch Serverless
  * Automatic document chunking & embedding
  * Contextual queries reducing hallucinations
  * Citation: returning sources for users

* Explored Bedrock Agents:
  * Action Groups calling Lambda functions
  * Agents autonomously planning and invoking tools
  * Session attributes storing context across turns
  * Combined Knowledge Base + Action for both knowledge and execution

* Applied Guardrails:
  * Configured deny topics and content filters (Hate, Sexual, Violence, Misconduct)
  * Custom PII redaction (email, phone numbers)
  * Prompt attack detection
  * Ensured the app was safe and aligned with Responsible AI

* Completed the first Bedrock RAG chatbot:
  * React frontend with a chat UI
  * API Gateway + Lambda calling Bedrock KB
  * Guardrails filtering before returning to the user
  * CloudWatch tracking input/output tokens and latency

* Wrote a sharing blog post on Bedrock to help the community get started with GenAI on AWS.
* New habits formed:
  * Always test Bedrock responses in the Playground before integrating into code
  * Cache responses in ElastiCache with key = hash(prompt+model) to reduce cost
  * Evaluate prompt quality with a fixed test set (accuracy, latency, cost)
  * Default guardrail on every public AI application
* ...