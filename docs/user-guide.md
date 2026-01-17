# 📘 DocuMagic PDF API — User Guide

## 🎤 How to Present This in an Interview
“This User Guide shows how I write task‑oriented, beginner‑friendly documentation. It walks developers through authentication, their first API call, error handling, and best practices. It complements the API Reference Guide by focusing on workflow and usability rather than raw endpoint details.”

---

# 1. Introduction
The DocuMagic PDF API allows developers to upload documents, convert them to PDF, and retrieve the processed files programmatically. This guide provides a step‑by‑step introduction to using the API, including authentication, making your first request, handling responses, and troubleshooting common issues.

This guide is intended for developers who are new to the API and want a practical, task‑oriented walkthrough.

---

# 2. How the API Works (Conceptual Overview)

The DocuMagic API follows a simple three‑step workflow:

1. Upload a document using the `/upload` endpoint  
2. Check the processing status using `/documents/{id}`  
3. Download the converted PDF once processing is complete  

The API uses API key authentication, JSON responses, and RESTful conventions.

---

# 3. Getting Started

## 3.1 Create a Developer Account
To begin using the API:
- Visit the DocuMagic Developer Portal  
- Create an account  
- Navigate to **API Keys**  
- Generate a new key  

Your API key is required for all requests.

## 3.2 Install Required Tools
You can use any HTTP client. Common options include:
- cURL  
- Postman  
- Python `requests`  
- JavaScript `fetch`  
- PowerShell  

Examples in this guide use **cURL** for simplicity.

---

# 4. Authentication

## 4.1 Using Your API Key
All requests must include the Authorization header:

```
Authorization: Bearer <YOUR_API_KEY>
```

## 4.2 Security Best Practices
- Never embed API keys in client‑side code  
- Store keys in environment variables  
- Rotate keys regularly  
- Delete unused or compromised keys  

## 4.3 Common Authentication Errors
- **401 Unauthorized** — Missing or invalid key  
- **403 Forbidden** — Key lacks required permissions  

---

# 5. Your First API Call

This section walks you through the complete workflow: upload → check status → download.

---

## 5.1 Step 1: Upload a Document

### Purpose
Start the PDF conversion process by uploading a file.

### Example (cURL)
```
curl -X POST "https://api.documagic.com/v1/documents/upload" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -F "file=@invoice.docx"
```

### Successful Response
```json
{
  "documentId": "a92f3c1e-4b12-4c8e-9f1a-2d9e7b1a4f22",
  "status": "processing",
  "estimatedCompletionSeconds": 12
}
```

### Notes
- Supported file types: `.docx`, `.xlsx`, `.pptx`, `.txt`  
- Large files may take longer to process  

---

## 5.2 Step 2: Check Document Status

### Purpose
Determine whether the PDF is ready for download.

### Example
```
curl -X GET "https://api.documagic.com/v1/documents/a92f3c1e-4b12-4c8e-9f1a-2d9e7b1a4f22" \
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Possible Status Values

| Status | Meaning |
|--------|---------|
| processing | Conversion in progress |
| completed | PDF is ready |
| failed | Conversion failed |

### Completed Response Example
```json
{
  "documentId": "a92f3c1e-4b12-4c8e-9f1a-2d9e7b1a4f22",
  "status": "completed",
  "downloadUrl": "https://api.documagic.com/v1/documents/a92f3c1e/download"
}
```

---

## 5.3 Step 3: Download the PDF

### Example
```
curl -L -o output.pdf \
  "https://api.documagic.com/v1/documents/<documentId>/download" \
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Notes
- The `-L` flag follows redirects  
- The file will be saved as `output.pdf`  

---

# 6. Handling Errors

## 6.1 Common Error Responses
```json
{
  "error": "invalid_file_type",
  "message": "The uploaded file type is not supported."
}
```

## 6.2 Troubleshooting Tips
- **400 invalid_file_type** → Check supported formats  
- **404 not_found** → Verify the document ID  
- **429 rate_limit_exceeded** → Wait for the `Retry-After` header  
- **500 server_error** → Retry or contact support  

---

# 7. Rate Limits

| Limit Type | Value |
|------------|--------|
| Requests per minute | 60 |
| Concurrent uploads | 5 |
| Burst limit | 10 |

If you exceed limits, you’ll receive:
```
429 Too Many Requests
Retry-After: 30
```

---

# 8. Best Practices

## 8.1 Use Webhooks for Large Files
Provide a `callbackUrl` during upload to receive a notification when processing completes.

## 8.2 Implement Retry Logic
Retry on:
- 429 (rate limit)  
- 500 (server error)  

## 8.3 Store Document IDs
Document IDs are required for status checks and downloads.

---

# 9. Versioning

**Current Version: v1**

Semantic versioning:
- **MAJOR** — breaking changes  
- **MINOR** — new features  
- **PATCH** — bug fixes  

### Deprecation Policy
- Deprecated endpoints remain available for 12 months  
- Migration guides accompany major updates  

---

# 10. Support
For help, visit the DocuMagic Developer Portal or contact support with:
- Your document ID  
- The full error response  
- The request timestamp  
