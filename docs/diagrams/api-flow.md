# 📊 API Request Flow Diagram (Mermaid)

This file contains the Mermaid diagram source for the DocuMagic PDF API request flow.  
You can paste this directly into GitHub or any Mermaid‑compatible renderer.

```mermaid
flowchart TD

    A[Client Application] -->|1. Sends HTTP Request<br/>URL + Method + Headers + Body| B[API Gateway]

    B -->|Validate Authentication<br/>Bearer Token Check| C{Token Valid?}
    C -->|No| E[Return 401/403 Error]
    C -->|Yes| F[Check Rate Limits]

    F -->|Exceeded| G[Return 429 Error]
    F -->|Within Limits| H[Route to Service Layer]

    H --> I{Which Endpoint?}

    I -->|Upload Document| J[Upload Service<br/>Store File + Start Processing]
    I -->|Check Status| K[Status Service<br/>Return Processing State]
    I -->|Download PDF| L[Download Service<br/>Return PDF Stream]

    J --> M[JSON Response<br/>documentId + status]
    K --> M
    L --> N[Binary PDF Response]

    M --> O[Client Receives JSON]
    N --> O[Client Receives PDF]

    O --> P[Client Handles Result<br/>Display, Store, or Retry]
```

---

## 📘 How to Use This File

- GitHub will automatically render the Mermaid diagram when viewed in the browser.
- VS Code can render it using the **Markdown Preview Mermaid Support** extension.
- Tools like Mermaid Live Editor, Draw.io, or Excalidraw can import this text to generate a visual diagram.

