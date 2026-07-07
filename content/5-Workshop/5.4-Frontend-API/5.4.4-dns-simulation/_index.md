---
title : "Build & Deploy React frontend"
date : 2026-04-12
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

In this section you will build a **React SPA** for the chatbot, run a production build, then deploy it to **S3 + CloudFront**.

#### 1. Scaffold the React project with Vite

```bash
cd ~/fcaj-chat-app

npm create vite@latest frontend -- --template react
cd frontend
npm install
```

#### 2. Configure environment variables

Create `.env.production` with your API URL (from 5.4.2):

```bash
cat > .env.production << 'EOF'
VITE_API_URL=https://<API_ID>.execute-api.ap-southeast-1.amazonaws.com/prod
EOF

cat > .env.development << 'EOF'
VITE_API_URL=http://localhost:8000
EOF
```

> 💡 Replace `<API_ID>` with the real API Gateway ID.

#### 3. Replace `src/App.jsx`

```jsx
import { useState } from "react";
import "./App.css";

const API_URL = import.meta.env.VITE_API_URL;

function App() {
  const [messages, setMessages] = useState([
    {
      role: "assistant",
      content:
        "Hi! I'm the FCAJ Workshop chatbot. Ask me anything about the AWS documents.",
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
          content: data.answer || data.error || "Unknown error",
          citations: data.citations || [],
        },
      ]);
    } catch (e) {
      setMessages((m) => [
        ...m,
        { role: "assistant", content: "Error: " + e.message },
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
            <strong>{m.role === "user" ? "You" : "AI"}:</strong> {m.content}
            {m.citations && m.citations.length > 0 && (
              <details>
                <summary>📚 {m.citations.length} sources</summary>
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
          placeholder="Ask anything about the AWS documents..."
          disabled={loading}
        />
        <button onClick={send} disabled={loading}>
          {loading ? "..." : "Send"}
        </button>
      </div>
    </div>
  );
}

export default App;
```

#### 4. Replace `src/App.css`

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

#### 5. Production build

```bash
npm run build
# Output: dist/ containing index.html + assets/
```

#### 6. Create the S3 bucket for the frontend

```bash
BUCKET="fcaj-chat-ui-$(date +%s)"  # unique bucket name
echo "Bucket name: $BUCKET"

aws s3 mb s3://$BUCKET --region ap-southeast-1

# Remove Block Public Access so the static site can be served
aws s3api delete-public-access-block --bucket $BUCKET --region ap-southeast-1

# Bucket policy allowing public read
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

#### 7. Upload the build to S3

```bash
aws s3 sync dist/ s3://$BUCKET/ --delete --region ap-southeast-1
```

Open the browser: **http://`<bucket>`.s3-website-ap-southeast-1.amazonaws.com**

Try asking: *"What S3 storage classes are available?"* → Frontend calls API → Bedrock retrieves from KB → Claude answers with citations.

![chat ui](/images/5-Workshop/5.4-Frontend-API/chat-ui.png)

#### 8. (Optional) Put CloudFront in front of S3

CloudFront gives you HTTPS + caching + a global CDN:

```bash
# Origin Access Identity so CloudFront can read the (now private) bucket
OAI=$(aws cloudfront create-cloud-front-origin-access-identity \
  --cloud-front-origin-access-identity-config \
  CallerReference=$(date +%s),Comment="OAI for $BUCKET" \
  --query 'CloudFrontOriginAccessIdentity.Id' --output text)

# Update bucket policy: only OAI can read
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

# Create the CloudFront distribution
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

The CloudFront domain looks like `d111111abcdef8.cloudfront.net` — open it in the browser to test.

#### 9. Custom domain (advanced, optional)

If you own a domain (e.g. `chat.fcaj-demo.com`):

1. Create an ACM certificate in **us-east-1** (required by CloudFront).
2. Add `Aliases` + `ViewerCertificate` to the CloudFront distribution.
3. Add a Route 53 `A` record pointing at the CloudFront distribution.

#### Summary

After this section you have:
* React SPA built for production with Vite.
* Frontend hosted on S3 (HTTP) or CloudFront (HTTPS + CDN).
* Full end-to-end pipeline:

```
User → React UI → API Gateway → Lambda → Bedrock KB → answer + citation
```

Users can now chat directly with the Knowledge Base through a polished web UI.

#### References
* [Vite — Getting Started](https://vitejs.dev/guide/)
* [Hosting SPA on S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html)
* [CloudFront with S3 origin](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html)
* [React useState hook](https://react.dev/reference/react/useState)