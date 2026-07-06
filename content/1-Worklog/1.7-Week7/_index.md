---
title: "Week 7 Worklog"
date: 2026-05-24
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
### Week 7 Objectives:
* Get familiar with the AWS AI/ML ecosystem: AI Services (Rekognition, Comprehend, Polly, Translate) and ML Services (SageMaker).
* Master the workflow of training and deploying ML models on the cloud.
* Build a simple end-to-end AI project (NLP + Vision + Speech) and write a sharing blog.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                                                                                                                                                                                                                            | Start Date | Completion Date | Reference Material                        |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 2   | - Overview of AI/ML on AWS: <br>&emsp; + AI services vs ML services <br>&emsp; + Bedrock, SageMaker, Rekognition, Comprehend, Polly, Translate, Lex <br>&emsp; + Real-world use cases <br> - Learn Amazon Rekognition: <br>&emsp; + Label Detection & Image Properties <br>&emsp; + Facial Analysis (age, emotion, gender) <br>&emsp; + Text in Image (OCR) <br>&emsp; + Moderation labels <br>&emsp; + Custom Labels | 05/25/2026 | 05/25/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 3   | - Learn Amazon Comprehend (NLP): <br>&emsp; + Sentiment Analysis <br>&emsp; + Entity Recognition (PERSON, ORG, LOCATION) <br>&emsp; + Key phrase extraction <br>&emsp; + Topic Modeling <br>&emsp; + Custom Comprehend <br> - **Practice:** Run sentiment analysis on a set of English reviews and extract entities & key phrases                                                                                                                                                | 05/26/2026 | 05/26/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 4   | - Learn Amazon Polly & Translate: <br>&emsp; + Polly: Text-to-Speech, SSML, voices per language <br>&emsp; + Translate: Language translation, custom terminology <br> - Learn Amazon Lex: <br>&emsp; + Chatbot voice & text <br>&emsp; + Intents, Slots, Utterances <br> - **Practice:** Build a multilingual chatbot using Lambda + Translate + Polly; serve audio via S3 + CloudFront                                                                                                                                        | 05/27/2026 | 05/27/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 5   | - Learn Amazon SageMaker: <br>&emsp; + Notebook Instance <br>&emsp; + Built-in Algorithm (XGBoost, Linear Learner, BlazingText, ...) <br>&emsp; + Training Job with custom Docker <br>&emsp; + Endpoint (Real-time, Serverless) <br>&emsp; + SageMaker Studio (IDE) <br>&emsp; + SageMaker MLOps (Pipelines, Model Registry) <br> - **Practice:** Train a house-price prediction model with XGBoost in SageMaker, deploy as endpoint, invoke predict from Lambda                                                                 | 05/28/2026 | 05/29/2026      | <https://cloudjourney.awsstudygroup.com/> |
| 6   | - **Week 7 capstone lab:** <br>&emsp; + Web app analysing images (Rekognition) + text (Comprehend) + audio (Polly) <br>&emsp; + Serverless API (Lambda + API Gateway) calling AI services <br>&emsp; + React frontend uploading image & text, displaying results + audio <br>&emsp; + Wrap up AI/ML knowledge & update workshop <br> - Write blog "AI Services on AWS - Overview for Beginners"                                                                                                                                  | 05/29/2026 | 05/29/2026      | <https://cloudjourney.awsstudygroup.com/> |


### Week 7 Achievements:
* Gained an overview of the AWS AI/ML ecosystem:
  * Clearly distinguished AI Services (managed, no training needed) from ML Services (SageMaker, custom training)
  * Selected appropriate services for real-world problems:
    * OCR on images → Rekognition
    * Sentiment analysis → Comprehend
    * Text-to-speech → Polly
    * Multilingual translation → Translate
    * Custom models on custom data → SageMaker

* Mastered AI Services:
  * **Rekognition:** label detection, FaceDetail, DetectText OCR
  * **Comprehend:** Sentiment (+/-/Neutral), Entity (PERSON, DATE, MONEY), Key phrase
  * **Polly:** Text-to-Speech with many voices (Joanna, Matthew for EN; Giang, Linh for VI)
  * **Translate:** translate across 70+ languages, supports custom terminology
  * **Lex:** build chatbots for customer service

* Deployed a serverless multilingual chatbot:
  * Web frontend accepting VI/EN questions
  * Lambda processing → Translate → Polly → returning audio
  * Audio stored in S3, delivered via CloudFront
  * Caching translation and audio to reduce cost

* Got hands-on with SageMaker:
  * Created a Notebook Instance for EDA
  * Used the built-in XGBoost algorithm on the Boston Housing dataset
  * Deployed the model as an Endpoint and invoked it from Lambda
  * Compared Real-time Endpoint (always on) and Serverless Inference (cold start)
  * SageMaker Studio IDE provides a full-featured Jupyter environment

* Completed the AI services integration lab:
  * React frontend uploading images + text
  * API Gateway + Lambda calling Rekognition, Comprehend, Polly
  * Results returned as JSON + audio link
  * CI/CD deployed to S3 + CloudFront

* Published a sharing blog post on AI/ML on AWS for the community.
* New habits formed:
  * Always read the pricing model before using an AI service (per request/image/character)
  * Cache AI results in DynamoDB or ElastiCache to save cost
  * Evaluate model bias & safety before going to production
  * When using SageMaker, stop the notebook when not in use to avoid charges
* ...