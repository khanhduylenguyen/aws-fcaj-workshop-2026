---
title: "Event 1 – FCAJ Community Day MAY 2026"
date: 2026-05-23
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event Report: FCAJ Community Day MAY 2026

## 1. Event Overview

| | |
|---|---|
| **Event Name** | FCAJ Community Day MAY 2026 |
| **Date & Time** | 08:30, May 23, 2026 |
| **Venue** | 26th Floor, Bitexco Financial Tower, 02 Hai Trieu Street, District 1, HCMC |
| **Organizer** | First Cloud AI Journey (FCAJ) community & AWS partners |
| **My Role** | Attendee |

## 2. Event Objectives

FCAJ Community Day is a periodic offline meetup organized by the **First Cloud AI Journey** community, designed to:

- Update attendees on the latest **Cloud & Generative AI** trends on AWS.
- Showcase AWS AI/Data services such as **Amazon Quick Suite** (no-code data workflow) and **Amazon CloudFront** (Edge & CDN security).
- Share **hackathon field reports** — especially GenAI products built under intense time pressure.
- Deep-dive into **Enterprise-Grade Multi-Agent architectures** applied to real business problems (banking, fintech).

## 3. Speakers

| # | Speaker | Role | Organization |
|---|---|---|---|
| 1 | Tinh Truong | Platform Engineer | GoTymeX |
| 2 | Anh Pham | Cloud Consultant | G-ASIAPACIFIC Vietnam |
| 3 | Thinh Nguyen | DevOps Engineer | FCAJ |
| 4 | Thao Nguyen, Mai Nguyen, Uyen Le | GenAI Engineers | Team VIB |
| 5 | Duc Dao | Solutions Architect | Cloud Kinetics |
| 6 | Vy Lam | Senior Business Systems Analyst | VPBank |

## 4. Session Highlights

### 4.1. Context Is Everything – Making AI Actually Work for You *(Tinh Truong – GoTymeX)*

- **Why many AI projects fail:** not because the model is weak, but because of **missing or wrong Context**. The same prompt with a different context can produce wildly different outputs.
- **Mindset shift:** move from "writing better prompts" to **managing AI memory** (the *Second AI Brain* concept) — retrieval layers, memory state, and agent state to keep AI grounded in long-running contexts.
- **Practical tips:** how to structure context so AI outputs are more stable and hallucination rates drop.

### 4.2. Friendly AI Assistant with Amazon Quick *(Anh Pham)*

- **Quick Chat Agent** — AI assistant for data analysis and insight extraction.
- **Quick Flows** — build automated workflows in natural language; no code required.
- **Quick Spaces & Quick Sight** — collaborative workspace for sharing insights and turning raw data into dashboards.

### 4.3. From Edge To Origin – CloudFront as Your Foundation *(Duc Dao – Cloud Kinetics)*

- **Universal CDN:** web, API, media and streaming workloads all go through CloudFront.
- **Cost optimization:** smart caching plus tiered pricing significantly reduce delivery cost.
- **Reliability & Security:** improved DDoS resistance, built-in WAF, edge-level system protection.

### 4.4. 36 Hours with LotusHacks – Building UTMorpho from Idea to Reality *(Thinh Nguyen + LotusHacks team)*

- The journey from zero to a clearly defined problem statement.
- A **36-hour continuous coding sprint** — real hackathon workflow.
- Lessons learned: the setbacks, the turning points, and how to deliver the final demo convincingly.

### 4.5. Non-Determinism of "Deterministic" LLM Settings *(Duc Dao)*

- **How LLMs choose the next token:** probability-driven, not rule-based.
- **The truth about `Temperature = 0`:** it does NOT guarantee perfect determinism. Inference optimizations (KV-cache, batching, sampling tweaks) still affect the output.
- **Mitigation strategies:** fixed seeds, controlled retries, or self-consistency decoding when you need reproducible answers.

### 4.6. Enterprise-Grade Multi-Agent System – Startup Credit Scoring *(Vy Lam – VPBank + VIB team)*

- **The banking pain point:** startup financial data does not match existing core-banking schemas.
- **Single Agent vs Multi-Agent:** when to use a single agent (simple task) versus multiple specialized agents (smaller scope, better hallucination control).
- **"Virtual Credit Committee" blueprint:** a multi-agent system with strong **Guardrails, clear Compliance and measurable Operational ROI**.

## 5. Three Pillars of Enterprise-Grade AI

From the talks, I synthesize **three core principles** for enterprise AI systems:

| Pillar | Meaning | Real-world AWS example |
|---|---|---|
| **Securely** | Strong security, PII & prompt-injection filtering | Bedrock Guardrails, WAF + CloudFront |
| **Reliably** | Stable, fault-tolerant, always-on | CloudFront CDN at the Edge, LLM fallback |
| **Scalably** | Grow under load without rewriting the architecture | Scale OpenSearch OCU, multi-region |

## 6. Applying to My Internship Project (FCAJ Internal Knowledge Assistant)

- **CloudFront for the frontend:** add Amazon CloudFront to the deployment plan of the **FCAJ Internal Knowledge Assistant** capstone so the React SPA meets the **Securely + Reliably** bar.
- **Stronger Guardrails:** when building the RAG chatbot on **Bedrock + Knowledge Base**, apply guardrail layers to **both input AND output** rather than filtering on only one side.
- **Bring the "36-hour sprint" spirit** of LotusHacks into the biweekly project milestones.
- **Explore Amazon Quick Suite** for log analysis and operational dashboard automation, reducing manual ops effort.

## 7. Personal Reflection

The things that impressed me most:

- **Multi-disciplinary speakers:** six sessions by engineers spanning platform, consulting, DevOps, GenAI engineering, solutions architecture and banking business analysis. For the first time I could see the full lifecycle of an AI product — from low-level infrastructure all the way up to a real enterprise problem.
- **Vy Lam's (VPBank) talk** hit hardest. The "Virtual Credit Committee" is a brilliantly real-world problem: it has to satisfy strict financial regulations while still benefiting from multi-agent intelligence.
- **A new mindset:** a product is more than code. It starts with the *right problem*, the *right context*, and an *architecture strong enough* to support the idea. LotusHacks' UTMorpho is a textbook example.
- **Gifts & networking:** beyond knowledge, I received a small souvenir from the organizers and made new connections in the FCAJ community — useful for both my capstone and post-internship work.

## 8. Some Photos from the Event

*Add your event photos here*

> Overall, FCAJ Community Day MAY 2026 was an eye-opening experience. What I had learned theoretically in online workshops was suddenly being applied in real production systems by senior engineers.
