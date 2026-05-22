# e-Boekhouden App API v4 (Unofficial)

Unofficial OpenAPI/Swagger specification for the internal **e-Boekhouden iPhone app API**.

This specification was inferred from network traffic captured from the official iOS application using Charles Proxy.

## Disclaimer

This project is:

- **Unofficial**
- **Not affiliated with e-Boekhouden.nl**
- Based on observed HTTP traffic
- Incomplete by definition unless all app flows are captured

Use at your own risk.

## Base URL

```text
https://appapi.e-boekhouden.nl/api/v4
```

## Authentication

The app uses JWT bearer authentication.

### Token renewal / login

```http
POST /auth/renew
```

Example request:

```json
{
  "username": "your-username",
  "password": "your-password",
  "previousToken": ""
}
```

Example response:

```json
{
  "token": "JWT_TOKEN"
}
```

Authenticated requests use:

```http
Authorization: Bearer JWT_TOKEN
```

---

# Main Features Discovered

## File Archive (`bestanden`)

The iPhone app exposes a full document archive API.

### List root folders

```http
GET /bestanden
```

### Open folder

```http
GET /bestanden/{directoryId}
```

### Create folder

```http
POST /bestanden/directory
```

Example:

```json
{
  "id": 0,
  "parentDirectoryId": 75161614,
  "naam": "2026-06",
  "setAsMenHInbox": false
}
```

### Upload file

```http
POST /bestanden/upload
```

Example:

```json
{
  "fileName": "invoice.pdf",
  "data": "BASE64_FILE",
  "directoryId": 123456,
  "scan": false
}
```

### Link uploaded file to mutation

```http
POST /bestanden/koppel
```

Example:

```json
{
  "fileId": 123456,
  "soort": 1,
  "koppelId": 9903
}
```

### Retrieve linked files

```http
POST /bestanden/gekoppeld
```

### Remove file link

```http
DELETE /bestanden/koppel/{linkId}
```

---

# OpenAPI Specification

The repository contains a sanitized OpenAPI specification:

```text
e-boekhouden-appapi-v4-github-safe.openapi.json
```

The specification was generated automatically from captured traffic and manually sanitized for public publication.

Sensitive data removed:

- usernames
- passwords
- JWT tokens
- email addresses
- uploaded file contents
- long identifiers

---

# Suggested Usage

Possible use cases:

- automate document uploads
- build integrations
- attach invoices to mutations
- archive PDFs automatically
- create bookkeeping automation workflows
- OCR + AI classification pipelines
- synchronize documents externally

---

# Reverse Engineering Notes

Useful tools:

- Charles Proxy
- mitmproxy
- HAR export
- Postman
- OpenAPI Generator
- Swagger Editor

---

# Legal

This repository contains only inferred API metadata and does not include proprietary source code from e-Boekhouden.nl.

If requested by the rights holder, this repository may be modified or removed.


## Search Endpoints

Additional endpoints discovered from later traffic captures.

### Search files/folders

```http
GET /bestanden/search?search=invoice
```

Supports free text search inside the document archive.

### Search mutations

```http
GET /mutaties/search?search=supplier
```

or directly by mutation number:

```http
GET /mutaties/search?mutatienummer=9903
```

Useful for automatically locating bookkeeping entries before linking uploaded files.

