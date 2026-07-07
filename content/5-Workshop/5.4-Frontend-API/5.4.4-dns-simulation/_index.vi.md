---
title : "Build & Deploy React frontend"
date : 2026-04-12
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

Trong phần này bạn sẽ xây dựng **React SPA** đơn giản cho chatbot, build production rồi deploy lên **S3 + CloudFront**.

#### 1. Khởi tạo React project với Vite

```bash
cd ~/fcaj-chat-app

npm create vite@latest frontend -- --template react
cd frontend
npm install
```

#### 2. Cấu hình biến môi trường

Tạo file `.env.production` chứa API URL của bạn (lấy ở 5.4.2):

```bash
cat > .env.production << 'EOF'
VITE_API_URL=https://<API_ID>.execute-api.ap-southeast-1.amazonaws.com/prod
EOF

cat > .env.development << 'EOF'
VITE_API_URL=http://localhost:8000
EOF
```

> 💡 Thay `<API_ID>` bằng API Gateway ID thật.

#### 3. Sửa `src/App.jsx`

```jsx
import { useState } from "react";
import "./App.css";

const API_URL = import.meta.env.VITE_API_URL;

function App() {
  const [messages, setMessages] = useState([
    {
      role: "assistant",
      content:
        "Xin chào! Mình là chatbot FCAJ Workshop. Hỏi mình bất kỳ điều gì về tài liệu AWS nhé.",
    },
  ]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);

  const send = async () => {
    if (!input.trim() || loading) return;
    const userMsg = { role: "user", content: input };
    setMessages((m) => [...m, userMsg]);
    setInput("");
    setLoading(true);

    try {
      const res = await fetch(`${API_URL}/chat`, {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ question: userMsg.content }),
      });
      const data = await res.json();

      setMessages((m) => [
        ...m,
        {
          role: "assistant",
          content: data.answer || data.error || "Lỗi không xác định",
          citations: data.citations || [],
        },
      ]);
    } catch (e) {
      setMessages((m) => [
        ...m,
        { role: "assistant", content: "Lỗi: " + e.message },
      ]);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="chat-app">
      <header>
        <h1>🧠 FCAJ Workshop Chat</h1>
        <small>Powered by Amazon Bedrock + RAG</small>
      </header>

      <div className="messages">
        {messages.map((m, i) => (
          <div key={i} className={`message ${m.role}`}>
            <strong>{m.role === "user" ? "Bạn" : "AI"}:</strong> {m.content}
            {m.citations && m.citations.length > 0 && (
              <details>
                <summary>📚 {m.citations.length} nguồn tham khảo</summary>
                <ul>
                  {m.citations.map((c, j) => (
                    <li key={j}>
                      <a href={c.uri} target="_blank" rel="noreferrer">
                        {c.title || c.uri}
                      </a>
                    </li>
                  ))}
                </ul>
              </details>
            )}
          </div>
        ))}
        {loading && <div className="message assistant loading">...</div>}
      </div>

      <div className="input-row">
        <input
          value={input}
          onChange={(e) => setInput(e.target.value)}
          onKeyDown={(e) => e.key === "Enter" && send()}
          placeholder="Hỏi bất kỳ điều gì về tài liệu AWS..."
          disabled={loading}
        />
        <button onClick={send} disabled={loading}>
          {loading ? "..." : "Gửi"}
        </button>
      </div>
    </div>
  );
}

export default App;
```

#### 4. Thay file `src/App.css`

```css
* { box-sizing: border-box; }
body { margin: 0; font-family: system-ui, -apple-system, sans-serif; background: #f8fafc; }

.chat-app {
  max-width: 800px;
  margin: 0 auto;
  padding: 1.5rem;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}

header { text-align: center; margin-bottom: 1rem; }
header h1 { margin: 0; color: #1e293b; }
header small { color: #64748b; }

.messages {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  margin: 1rem 0;
  flex: 1;
  overflow-y: auto;
  max-height: 65vh;
}

.message {
  padding: 0.75rem 1rem;
  border-radius: 12px;
  max-width: 80%;
  line-height: 1.5;
  word-wrap: break-word;
}

.message.user {
  background: #2563eb;
  color: white;
  align-self: flex-end;
}

.message.assistant {
  background: #f1f5f9;
  color: #1e293b;
  align-self: flex-start;
}

.message.loading {
  opacity: 0.6;
  font-style: italic;
}

details {
  margin-top: 0.5rem;
  font-size: 0.85rem;
}

details ul {
  margin: 0.25rem 0 0 1rem;
  padding: 0;
}

.input-row {
  display: flex;
  gap: 0.5rem;
  padding: 1rem 0;
}

.input-row input {
  flex: 1;
  padding: 0.75rem;
  border: 1px solid #cbd5e1;
  border-radius: 8px;
  font-size: 1rem;
}

.input-row input:focus {
  outline: none;
  border-color: #2563eb;
}

.input-row button {
  padding: 0.75rem 1.5rem;
  background: #2563eb;
  color: white;
  border: 0;
  border-radius: 8px;
  cursor: pointer;
  font-size: 1rem;
}

.input-row button:disabled {
  background: #94a3b8;
  cursor: not-allowed;
}
```

#### 5. Build production

```bash
npm run build
# Output: dist/ chứa index.html + assets/
```

#### 6. Tạo S3 bucket cho frontend

```bash
BUCKET="fcaj-chat-ui-$(date +%s)"  # tên bucket unique
echo "Bucket name: $BUCKET"

aws s3 mb s3://$BUCKET --region ap-southeast-1

# Disable Block Public Access để serve static website
aws s3api delete-public-access-block --bucket $BUCKET --region ap-southeast-1

# Bucket policy cho phép public read
cat > bucket-policy.json << EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::${BUCKET}/*"
    }
  ]
}
EOF

aws s3api put-bucket-policy --bucket $BUCKET --policy file://bucket-policy.json

# Enable static website hosting
aws s3 website s3://$BUCKET \
  --index-document index.html \
  --error-document index.html

echo "Website URL: http://${BUCKET}.s3-website-ap-southeast-1.amazonaws.com"
```

#### 7. Upload build lên S3

```bash
aws s3 sync dist/ s3://$BUCKET/ --delete --region ap-southeast-1
```

Mở trình duyệt: **http://`<bucket>`.s3-website-ap-southeast-1.amazonaws.com**

Thử hỏi: *"S3 có những storage class nào?"* → Frontend gọi API → Bedrock retrieve từ KB → Claude trả lời + citation.

![chat ui](/images/5-Workshop/5.4-Frontend-API/chat-ui.png)

#### 8. (Tuỳ chọn) Đặt CloudFront phía trước S3

CloudFront giúp HTTPS + cache + CDN tốc độ toàn cầu:

```bash
# Origin Access Identity để CloudFront đọc bucket private
OAI=$(aws cloudfront create-cloud-front-origin-access-identity \
  --cloud-front-origin-access-identity-config \
  CallerReference=$(date +%s),Comment="OAI for $BUCKET" \
  --query 'CloudFrontOriginAccessIdentity.Id' --output text)

# Update bucket policy: chỉ cho OAI đọc
cat > bucket-policy-cf.json << EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity ${OAI}" },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::${BUCKET}/*"
    }
  ]
}
EOF
aws s3api put-bucket-policy --bucket $BUCKET --policy file://bucket-policy-cf.json

# Tạo CloudFront distribution
cat > cf-config.json << EOF
{
  "CallerReference": "$(date +%s)",
  "Comment": "FCAJ Chat UI CDN",
  "DefaultRootObject": "index.html",
  "Origins": {
    "Quantity": 1,
    "Items": [{
      "Id": "S3-${BUCKET}",
      "DomainName": "${BUCKET}.s3.ap-southeast-1.amazonaws.com",
      "S3OriginConfig": {
        "OriginAccessIdentity": "origin-access-identity/cloudfront/${OAI}"
      }
    }]
  },
  "DefaultCacheBehavior": {
    "TargetOriginId": "S3-${BUCKET}",
    "ViewerProtocolPolicy": "redirect-to-https",
    "AllowedMethods": ["GET", "HEAD", "OPTIONS"],
    "CachedMethods": ["GET", "HEAD"],
    "ForwardedValues": {
      "QueryString": false,
      "Cookies": {"Forward": "none"}
    },
    "MinTTL": 0,
    "DefaultTTL": 3600,
    "MaxTTL": 86400
  },
  "Enabled": true
}
EOF

aws cloudfront create-distribution \
  --distribution-config file://cf-config.json \
  --query 'Distribution.DomainName' --output text
```

CloudFront domain sẽ trả về dạng `d111111abcdef8.cloudfront.net` — mở trong trình duyệt để test.

#### 9. Custom domain (nâng cao, tuỳ chọn)

Nếu bạn có domain riêng (ví dụ `chat.fcaj-demo.com`):

1. Tạo ACM certificate ở region **us-east-1** (CloudFront yêu cầu).
2. Thêm `Aliases` + `ViewerCertificate` vào CloudFront distribution.
3. Tạo Route 53 record `A → CloudFront distribution`.

#### Tổng kết

Sau phần này bạn có:
* React SPA chạy production với Vite.
* Frontend host trên S3 (HTTP) hoặc CloudFront (HTTPS + CDN).
* Toàn bộ pipeline end-to-end:

```
User → React UI → API Gateway → Lambda → Bedrock KB → trả lời + citation
```

User có thể chat trực tiếp với Knowledge Base thông qua giao diện web đẹp mắt.

#### Tài liệu tham khảo
* [Vite — Getting Started](https://vitejs.dev/guide/)
* [Hosting SPA on S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html)
* [CloudFront with S3 origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html)
* [React useState hook](https://react.dev/reference/react/useState)