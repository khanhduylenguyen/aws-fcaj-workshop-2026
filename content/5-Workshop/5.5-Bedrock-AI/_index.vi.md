---
title: "Trợ lý ảo AI Chatbot với Amazon Bedrock"
date: 2026-07-20
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Giới thiệu Amazon Bedrock

**Amazon Bedrock** là dịch vụ AI tạo sinh (Generative AI) được quản lý hoàn toàn bởi AWS. Medi Path Ease sử dụng Bedrock để xây dựng trợ lý ảo AI giúp trả lời câu hỏi thường gặp về sức khỏe.

##### Các Foundation Models khả dụng

| Model | Provider | Use Case |
|-------|----------|----------|
| Claude 3.5 Sonnet | Anthropic | Trả lời câu hỏi phức tạp |
| Claude 3 Haiku | Anthropic | Chat nhanh, chi phí thấp |
| Llama 3 | Meta | Chat đa mục đích |
| Titan Text | Amazon | Tích hợp sâu AWS |

##### Tại sao chọn Bedrock?

| Ưu điểm | Mô tả |
|---------|--------|
| **Fully Managed** | Không cần quản lý infrastructure |
| **Multiple Models** | Lựa chọn nhiều foundation models |
| **Guardrails** | Lọc nội dung có hại |
| **Knowledge Bases** | Tích hợp RAG dễ dàng |
| **Agents** | Xây dựng AI agents tự động |

#### Cấu hình Amazon Bedrock

##### Bước 1: Enable Model Access

Truy cập AWS Console → Amazon Bedrock → Model access

```bash
# Kiểm tra model access qua AWS CLI
aws bedrock list-foundation-models --region ap-southeast-1
```

> **📸 [HÌNH 1]: Bedrock Model Access Console**

##### Bước 2: Tạo IAM Role cho Lambda (nếu dùng Lambda)

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

#### Tích hợp Backend

##### Cài đặt AWS SDK

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
 * Gửi tin nhắn đến Claude và nhận phản hồi
 */
export async function chatWithClaude(messages, systemPrompt = null) {
    // Format messages theo Claude format
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
 * System prompt cho trợ lý y tế
 */
export const MEDICAL_ASSISTANT_PROMPT = `Bạn là trợ lý y tế ảo của Medi Path Ease. 
Nhiệm vụ của bạn là:
1. Trả lời câu hỏi thường gặp về sức khỏe
2. Cung cấp thông tin y tế cơ bản
3. Hướng dẫn người dùng các bước chăm sóc sức khỏe đơn giản

QUAN TRỌNG:
- Không đưa ra chẩn đoán y khoa cụ thể
- Không thay thế tư vấn của bác sĩ
- Khuyến khích người dùng đặt lịch khám nếu cần
- Chỉ cung cấp thông tin tham khảo
- Nếu câu hỏi nghiêm trọng, khuyên đến gặp bác sĩ ngay

Luôn trả lời bằng tiếng Việt, thân thiện và chuyên nghiệp.`;
```

##### Chat API Route

```javascript
// routes/chat.js
import express from "express";
import { chatWithClaude, MEDICAL_ASSISTANT_PROMPT } from "../services/bedrockService.js";

const router = express.Router();

/**
 * POST /api/chat
 * Gửi tin nhắn chat và nhận phản hồi từ AI
 */
router.post("/", authMiddleware, async (req, res) => {
    try {
        const { messages } = req.body;

        if (!messages || !Array.isArray(messages) || messages.length === 0) {
            return res.status(400).json({ error: "Messages array is required" });
        }

        // Giới hạn số messages để tránh token quá nhiều
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
 * Lấy danh sách tips sức khỏe
 */
router.get("/health-tips", authMiddleware, async (req, res) => {
    const tips = [
        "Uống đủ 2-3 lít nước mỗi ngày để duy trì sức khỏe.",
        "Ngủ đủ 7-8 tiếng mỗi đêm giúp cơ thể phục hồi.",
        "Tập thể dục ít nhất 30 phút mỗi ngày.",
        "Ăn nhiều rau xanh và trái cây giàu vitamin.",
        "Hạn chế thức ăn nhiều đường và chất béo.",
        "Kiểm tra sức khỏe định kỳ 6 tháng/lần."
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
                content: "Xin lỗi, tôi đang gặp sự cố. Vui lòng thử lại sau.",
                timestamp: new Date().toISOString()
            }]);
        } finally {
            setLoading(false);
        }
    };

    return (
        <div className="chatbot-container">
            <div className="chatbot-header">
                <h3>Trợ lý Y tế AI</h3>
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
                    placeholder="Nhập câu hỏi của bạn..."
                    disabled={loading}
                />
                <button onClick={handleSend} disabled={loading || !input.trim()}>
                    Gửi
                </button>
            </div>
        </div>
    );
}
```

#### Cấu hình Bedrock Guardrails

##### Guardrails cho nội dung y tế

```bash
# Tạo Guardrail
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

> **📸 [HÌNH 2]: Bedrock Guardrails Configuration**

#### Resource Summary

| Resource | Giá trị | Mô tả |
|---------|---------|--------|
| Model | Claude 3.5 Sonnet | Foundation model |
| Region | ap-southeast-1 | Singapore region |
| Max Tokens | 1024 | Response limit |
| Temperature | default | Creativity level |

> **📸 [HÌNH 3]: Amazon Bedrock Playground**

#### Tài liệu tham khảo

* [Amazon Bedrock Documentation](https://docs.aws.amazon.com/bedrock/)
* [Claude on Amazon Bedrock](https://docs.anthropic.com/claude/reference/overview)
* [Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
