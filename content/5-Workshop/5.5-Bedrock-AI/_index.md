---
title: "AI Chatbot with Amazon Bedrock"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Introduction to Amazon Bedrock

**Amazon Bedrock** is a fully managed Generative AI service by AWS. Medi Path Ease uses Bedrock to build an AI virtual assistant that answers common health questions.

##### Available Foundation Models

| Model | Provider | Use Case |
|-------|----------|----------|
| Claude 3.5 Sonnet | Anthropic | Answer complex questions |
| Claude 3 Haiku | Anthropic | Fast chat, low cost |
| Llama 3 | Meta | Multi-purpose chat |
| Titan Text | Amazon | Deep AWS integration |

##### Why Choose Bedrock?

| Advantage | Description |
|-----------|-------------|
| **Fully Managed** | No infrastructure management |
| **Multiple Models** | Choose from various foundation models |
| **Guardrails** | Filter harmful content |
| **Knowledge Bases** | Easy RAG integration |
| **Agents** | Build automated AI agents |

#### Amazon Bedrock Configuration

##### Step 1: Enable Model Access

Access AWS Console → Amazon Bedrock → Model access

```bash
# Check model access via AWS CLI
aws bedrock list-foundation-models --region ap-southeast-1
```

> **📸 [IMAGE 1]: Bedrock Model Access Console**

##### Step 2: Create IAM Role for Lambda (if using Lambda)

```json
{
    "Version": "2012-10-17",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "bedrock:InvokeModel",
            "bedrock:InvokeModelWithResponseStream"
        ],
        "Resource": "arn:aws:bedrock:ap-southeast-1::foundation-model/anthropic.claude-3-sonnet-20240229-v1:0"
    }]
}
```

#### Backend Integration

##### Install AWS SDK

```bash
npm install @aws-sdk/client-bedrock-runtime
```

##### Bedrock Service

```javascript
// services/bedrockService.js
import {
    BedrockRuntimeClient,
    InvokeModelCommand
} from "@aws-sdk/client-bedrock-runtime";

const bedrockClient = new BedrockRuntimeClient({
    region: process.env.AWS_REGION || "ap-southeast-1"
});

const MODEL_ID = "anthropic.claude-3-sonnet-20240229-v1:0";

/**
 * Send message to Claude and get response
 */
export async function chatWithClaude(messages, systemPrompt = null) {
    // Format messages to Claude format
    const claudeMessages = messages.map(msg => ({
        role: msg.role === "user" ? "user" : "assistant",
        content: msg.content
    }));

    const requestBody = {
        anthropic_version: "bedrock-2023-05-31",
        max_tokens: 1024,
        messages: claudeMessages,
        ...(systemPrompt && { system: systemPrompt })
    };

    const command = new InvokeModelCommand({
        modelId: MODEL_ID,
        contentType: "application/json",
        accept: "application/json",
        body: JSON.stringify(requestBody)
    });

    const response = await bedrockClient.send(command);
    const responseBody = JSON.parse(new TextDecoder().decode(response.body));

    return {
        content: responseBody.content[0].text,
        stopReason: responseBody.stop_reason
    };
}

/**
 * System prompt for medical assistant
 */
export const MEDICAL_ASSISTANT_PROMPT = `You are a virtual medical assistant for Medi Path Ease. 
Your tasks are:
1. Answer common health questions
2. Provide basic medical information
3. Guide users through simple health care steps

IMPORTANT:
- Do not provide specific medical diagnoses
- Do not replace doctor consultations
- Encourage users to book appointments if needed
- Only provide reference information
- If questions are serious, advise seeing a doctor immediately

Always respond in Vietnamese, friendly and professional.`;
```

##### Chat API Route

```javascript
// routes/chat.js
import express from "express";
import { chatWithClaude, MEDICAL_ASSISTANT_PROMPT } from "../services/bedrockService.js";

const router = express.Router();

/**
 * POST /api/chat
 * Send chat message and get AI response
 */
router.post("/", authMiddleware, async (req, res) => {
    try {
        const { messages } = req.body;

        if (!messages || !Array.isArray(messages) || messages.length === 0) {
            return res.status(400).json({ error: "Messages array is required" });
        }

        // Limit messages to avoid too many tokens
        const recentMessages = messages.slice(-10);

        const response = await chatWithClaude(
            recentMessages,
            MEDICAL_ASSISTANT_PROMPT
        );

        res.json({
            success: true,
            message: {
                role: "assistant",
                content: response.content,
                timestamp: new Date().toISOString()
            }
        });
    } catch (error) {
        console.error("Chat error:", error);
        res.status(500).json({ error: "Failed to get AI response" });
    }
});

/**
 * GET /api/chat/health-tips
 * Get health tips list
 */
router.get("/health-tips", authMiddleware, async (req, res) => {
    const tips = [
        "Drink 2-3 liters of water daily to maintain health.",
        "Get 7-8 hours of sleep each night to help the body recover.",
        "Exercise at least 30 minutes every day.",
        "Eat lots of green vegetables and vitamin-rich fruits.",
        "Limit sugary and fatty foods.",
        "Have regular health check-ups every 6 months."
    ];

    res.json({
        success: true,
        tips
    });
});

export default router;
```

##### Frontend Chat Component

```jsx
// components/Chatbot/Chatbot.jsx
import { useState, useRef, useEffect } from "react";
import { api } from "../../services/api";

export function Chatbot() {
    const [messages, setMessages] = useState([]);
    const [input, setInput] = useState("");
    const [loading, setLoading] = useState(false);
    const messagesEndRef = useRef(null);

    const scrollToBottom = () => {
        messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
    };

    useEffect(() => {
        scrollToBottom();
    }, [messages]);

    const handleSend = async () => {
        if (!input.trim() || loading) return;

        const userMessage = {
            role: "user",
            content: input.trim(),
            timestamp: new Date().toISOString()
        };

        setMessages(prev => [...prev, userMessage]);
        setInput("");
        setLoading(true);

        try {
            const response = await api.post("/chat", {
                messages: [...messages, userMessage]
            });

            setMessages(prev => [...prev, response.message]);
        } catch (error) {
            setMessages(prev => [...prev, {
                role: "assistant",
                content: "Sorry, I'm having trouble. Please try again later.",
                timestamp: new Date().toISOString()
            }]);
        } finally {
            setLoading(false);
        }
    };

    return (
        <div className="chatbot-container">
            <div className="chatbot-header">
                <h3>AI Medical Assistant</h3>
                <span className="status">Online</span>
            </div>

            <div className="messages">
                {messages.map((msg, index) => (
                    <div key={index} className={`message ${msg.role}`}>
                        <div className="message-content">
                            {msg.content}
                        </div>
                        <div className="message-time">
                            {new Date(msg.timestamp).toLocaleTimeString()}
                        </div>
                    </div>
                ))}
                {loading && (
                    <div className="message assistant">
                        <div className="message-content typing">
                            <span className="dot"></span>
                            <span className="dot"></span>
                            <span className="dot"></span>
                        </div>
                    </div>
                )}
                <div ref={messagesEndRef} />
            </div>

            <div className="input-area">
                <input
                    type="text"
                    value={input}
                    onChange={(e) => setInput(e.target.value)}
                    onKeyPress={(e) => e.key === "Enter" && handleSend()}
                    placeholder="Enter your question..."
                    disabled={loading}
                />
                <button onClick={handleSend} disabled={loading || !input.trim()}>
                    Send
                </button>
            </div>
        </div>
    );
}
```

#### Bedrock Guardrails Configuration

##### Guardrails for medical content

```bash
# Create Guardrail
aws bedrock create-guardrail \
    --name medi-path-ease-guardrail \
    --description "Guardrail for medical chatbot" \
    --topic-policy '{
        "topicsConfig": [
            {
                "name": "MedicalDiagnosis",
                "definition": "Providing specific medical diagnoses",
                "type": "DENY",
                "action": "BLOCKED"
            }
        ]
    }' \
    --content-policy '{
        "filtersConfig": [
            {
                "inputModalities": ["TEXT"],
                "outputModalities": ["TEXT"],
                "type": "SEXUAL"
            },
            {
                "inputModalities": ["TEXT"],
                "outputModalities": ["TEXT"],
                "type": "HATE"
            },
            {
                "inputModalities": ["TEXT"],
                "outputModalities": ["TEXT"],
                "type": "VIOLENCE"
            }
        ]
    }'
```

> **📸 [IMAGE 2]: Bedrock Guardrails Configuration**

#### Resource Summary

| Resource | Value | Description |
|---------|--------|-------------|
| Model | Claude 3.5 Sonnet | Foundation model |
| Region | ap-southeast-1 | Singapore region |
| Max Tokens | 1024 | Response limit |
| Temperature | default | Creativity level |

> **📸 [IMAGE 3]: Amazon Bedrock Playground**

#### References

* [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
* [Claude on Amazon Bedrock](https://docs.anthropic.com/claude/reference/overview)
* [Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
