# 🧩 DocuMagic PDF API — Reference Guide (Portfolio Sample)

## 🎤 How to Present This in an Interview
“This API documentation sample demonstrates how I structure developer‑facing content: clear authentication guidance, consistent endpoint patterns, realistic examples, error documentation, rate limits, and versioning. I focus on making the API predictable and easy to implement, with enough detail for advanced users but a clean structure that supports beginners.”

---

# 📘 Overview
The DocuMagic PDF API allows developers to upload documents, convert them to PDF, and retrieve the processed files programmatically. This reference guide provides authentication details, endpoint specifications, parameters, data types, request/response examples, error codes, rate limits, and versioning information.

---

# 🔐 Authentication

## API Key Authentication
All requests require an API key passed in the `Authorization` header using the Bearer token scheme.

### Header Format
```
Authorization: Bearer <YOUR_API_KEY>
```

### How to Obtain an API Key
- Create an account in the DocuMagic Developer Portal  
- Navigate to **API Keys**  
- Generate a new key (keys can be rotated at any time)

### Token Rotation
API keys expire every 90 days. Developers should:
- Store keys securely  
- Rotate keys before expiration  
- Delete unused or compromised keys  

### Common Authentication Errors

| Status | Meaning | Resolution |
|--------|---------|------------|
| **401 Unauthorized** | Missing or invalid API key | Verify header format and key validity |
| **403 Forbidden** | Key lacks required scope | Request additional scopes in the Developer Portal |

---

# 📤 Endpoint: Upload Document

**POST /v1/documents/upload**

Uploads a file and begins PDF conversion.

## Required Headers

| Header | Type | Description |
|--------|------|-------------|
| Authorization | string | Bearer token |
| Content-Type | string | Must be `multipart/form-data` |

## Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| file | file | Yes | The document to convert |
| callbackUrl | string | No | URL to notify when conversion completes |

## Request Example
```
POST /v1/documents/upload HTTP/1.1
Authorization: Bearer <YOUR_API_KEY>
Content-Type: multipart/form-data

file=@invoice.docx
callbackUrl=https://example.com/webhook
```

## Successful Response
```json
{
  "documentId": "a92f3c1e-4b12-4c8e-9f1a-2d9e7b1a4f22",
  "status": "processing",
  "estimatedCompletionSeconds": 12
}
```

---

# 📥 Endpoint: Download PDF

**GET /v1/documents/{documentId}/download**

Downloads the converted PDF file.

### Response
- `Content-Type: application/pdf`  
- Binary PDF stream

---

# 🧪 Error Codes

| HTTP Code | Error | Description | How to Fix |
|-----------|--------|-------------|------------|
| **400** | invalid_file_type | Unsupported file format | Upload `.docx`, `.xlsx`, `.pptx`, or `.txt` |
| **401** | unauthorized | Missing or invalid API key | Check Authorization header |
| **404** | not_found | Document ID does not exist | Verify the ID |
| **429** | rate_limit_exceeded | Too many requests | Retry after `Retry-After` header |
| **500** | server_error | Unexpected failure | Retry or contact support |

### Error Response Example
```json
{
  "error": "invalid_file_type",
  "message": "The uploaded file type is not supported."
}
```

---

# ⏱️ Rate Limits

| Limit Type | Value |
|------------|--------|
| Requests per minute | 60 |
| Concurrent uploads | 5 |
| Burst limit | 10 requests |

When limits are exceeded, the API returns:
```
429 Too Many Requests
Retry-After: 30
```

---

# 🔄 Versioning

**Current Version: v1**

Semantic versioning:

- **MAJOR** — breaking changes  
- **MINOR** — new features  
- **PATCH** — bug fixes  

### Deprecation Policy
- Deprecated endpoints remain available for 12 months  
- Deprecation notices appear in response headers  
- Migration guides are provided for major changes  

---

# 🧭 Changelog

### v1.1
- Added `callbackUrl` parameter  
- Improved error messages  
- Reduced average processing time  

### v1.0
- Initial release
