---
title : "Test endpoint với curl / Postman"
date : 2026-04-12
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

Trong phần này bạn sẽ kiểm tra toàn bộ pipeline end-to-end bằng cách gửi 1 câu hỏi tới API Gateway và đọc response trả về từ Bedrock Knowledge Base.

#### 1. Test nhanh với curl

Thay `<API_ID>` bằng ID API Gateway của bạn (lấy ở phần 5.4.2):

```bash
API_URL="https://<API_ID>.execute-api.ap-southeast-1.amazonaws.com/prod/chat"

curl -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "AWS Lambda là gì?"}'
```

Response mẫu (Bedrock trả lời + citation trích từ tài liệu trong KB):

```json
{
  "answer": "AWS Lambda là dịch vụ compute serverless của AWS, cho phép bạn chạy code mà không cần quản lý server. Lambda tự động scale theo lưu lượng và bạn chỉ trả tiền cho thời gian compute thực tế...",
  "citations": [
    {
      "uri": "s3://fcaj-bedrock-docs-xxx/aws-overview.pdf",
      "title": "s3://fcaj-bedrock-docs-xxx/aws-overview.pdf"
    },
    {
      "uri": "s3://fcaj-bedrock-docs-xxx/lambda-faq.md",
      "title": "s3://fcaj-bedrock-docs-xxx/lambda-faq.md"
    }
  ]
}
```

Nếu thấy `answer` và `citations` ⇒ pipeline OK.

#### 2. Test thêm các truy vấn khác

Hãy thử 4 loại truy vấn để verify RAG pipeline xử lý đúng:

```bash
# (a) Câu hỏi có trong Knowledge Base → trả lời + citation
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "S3 có những storage class nào?"}' | jq .

# (b) Câu hỏi ngoài Knowledge Base → trả lời + disclaimer "không tìm thấy"
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "Thời tiết hôm nay ở Tokyo?"}' | jq .

# (c) Câu hỏi tiếng Anh (verify multilingual)
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": "What is Amazon Bedrock?"}' | jq .

# (d) Câu hỏi rỗng → 400 error
curl -s -X POST "$API_URL" \
  -H "Content-Type: application/json" \
  -d '{"question": ""}' | jq .
```

**Kỳ vọng:**

| # | Câu hỏi | Kết quả |
|---|---|---|
| (a) | "S3 có những storage class nào?" | 200 + answer dài + 1-3 citations từ KB |
| (b) | "Thời tiết ở Tokyo?" | 200 + answer ngắn kiểu "Tôi không tìm thấy..." + 0 citation |
| (c) | "What is Amazon Bedrock?" | 200 + answer tiếng Anh + citations |
| (d) | "" | 400 + `{"error": "Missing 'question' field"}` |

#### 3. Test với Postman (tuỳ chọn)

Nếu thích GUI:

1. Mở Postman → **New → HTTP Request**.
2. Method: `POST`, URL: `$API_URL`.
3. Tab **Headers** → thêm `Content-Type: application/json`.
4. Tab **Body** → chọn **raw / JSON** → nhập `{"question": "..."}`.
5. Click **Send** → response hiển thị ở khung dưới.

![postman test](/images/5-Workshop/5.4-Frontend-API/postman.png)

#### 4. Xem log để debug

Nếu response lỗi, xem ngay CloudWatch Logs của Lambda:

```bash
aws logs tail /aws/lambda/fcaj-chat-handler --follow --region ap-southeast-1
```

Hoặc mở **CloudWatch console → Log groups → /aws/lambda/fcaj-chat-handler → chọn log stream mới nhất**.

Các lỗi thường gặp:

| Lỗi | Nguyên nhân | Cách sửa |
|---|---|---|
| `AccessDeniedException` | IAM role thiếu quyền `bedrock:RetrieveAndGenerate` | Xem lại policy ở 5.4.1 |
| `ResourceNotFoundException` | `KB_ID` sai hoặc KB chưa ACTIVE | Verify `aws bedrock-agent get-knowledge-base` |
| `ThrottlingException` | Gọi quá nhiều request | Đợi vài giây rồi retry |
| Lambda timeout (30s) | KB quá lớn, retrieve chậm | Tăng timeout hoặc giảm top-K |

![cloudwatch logs](/images/5-Workshop/5.4-Frontend-API/cloudwatch.png)

#### 5. Test trực tiếp với Lambda console

Mở **Lambda console → fcaj-chat-handler → Test tab**:

```json
{
  "question": "AWS Lambda là gì?"
}
```

Click **Test** → response xuất hiện ở **Execution result**. Cách này bypass API Gateway, hữu ích để debug Lambda trước khi debug API Gateway.

#### Tổng kết

Sau phần này bạn đã verify:
* Lambda + Bedrock KB + API Gateway hoạt động đúng.
* RAG pipeline retrieve + generate trả lời + citation đầy đủ.
* Xử lý được 4 case: có trong KB / ngoài KB / đa ngôn ngữ / input rỗng.

Phần tiếp theo (5.4.4) sẽ **build & deploy React frontend** lên S3 + CloudFront để user có giao diện chat đẹp.

#### Tài liệu tham khảo
* [Test API Gateway with curl](https://docs.aws.amazon.com/apigateway/latest/developerguide/how-to-test-method.html)
* [Bedrock KB error handling](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base-error.html)
* [CloudWatch Logs Insights query](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax.html)