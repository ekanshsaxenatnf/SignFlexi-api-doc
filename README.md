# SignFlexi API Documentation

## Overview

This documentation provides a detailed guide for using the SignFlexi API to generate bearer tokens and sign PDFs. The API has separate endpoints for sandbox and production environments.

- **Sandbox Base URL:** `https://api-sandbox.signflexi.com/api/v1`
- **Production Base URL:** `https://api.signflexi.com/api/v1`

---

## Authentication: Generate Bearer Token

**Endpoint:** `/token`  
**Method:** `POST`

This endpoint is used to generate a bearer token that is valid for 1 hour. This token is required for subsequent API calls.

### Request

**Headers:**

```http
x-api-key: YOUR_API_KEY
x-app-id: YOUR_APP_ID
content-type: application/json
```

**Request Body**

```json
{
  "client_id": "YOUR_CLIENT_ID",
  "client_secret": "YOUR_CLIENT_SECRET"
}
```

### Response

**Success (200)**

```json
{
  "access_token": "YOUR_ACCESS_TOKEN",
  "token_type": "bearer",
  "expires_in": 3600,
  "expires_at": 1732882484
}
```

**Error (401)**

```json
{
  "message": "Unauthorized"
}
```

---

## Sign a PDF: Single Page

**Endpoint:** `/sign`  
**Method:** `POST`

This endpoint is used to apply a digital signature to a PDF based on provided page and location.

### Request

**Headers**

```http
authorization: Bearer YOUR_ACCESS_TOKEN
x-app-id: YOUR_APP_ID
x-api-key: YOUR_API_KEY
content-type: application/json
```

**Request Body**

```json
{
  "pdfFile": "base64 encoded pdf file",
  "fileName": "sample.pdf",
  "fileIdentifier": "unique_identifier",
  "signatures": [
    {
      "page": 1,
      "x": 100,
      "y": 100,
      "height": 50,
      "width": 150
    }
  ]
}
```

- pdfFile: Base64-encoded PDF file.
- fileName: Name of the PDF file.
- fileIdentifier: A unique identifier for the file (e.g., invoice number).
- signatures: Array of signature objects with the following properties:
  - page: Page number where the signature will be applied (starting from 1).
  - x: X-coordinate of the signature.
  - y: Y-coordinate of the signature.
  - height: Height of the signature.
  - width: Width of the signature.

To apply signatures to multiple pages, include multiple page objects within the `signatures` array in the request body

### Response

**Success (200)**

```json
{
  "signedPdf": "signed pdf in base64 format",
  "signedUrl": "https://example.com/signedfile.pdf"
}
```

- signedPdf: The signed PDF in Base64 format.
- signedUrl: A URL to download the signed PDF. The URL is valid for 1 hour.

**Error (500)**

```json
{
  "message": "Internal Server Error"
}
```
