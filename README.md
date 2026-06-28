# medication-verify-server

A lightweight Express API that uses Google Gemini vision to detect whether an image contains medication (pills, tablets, capsules, etc.).

## Requirements

- Node.js 18+
- Google Gemini API key

## Setup

```bash
npm install
```

Create a `.env` file in the project root:

```
GEMINI_API_KEY=your-api-key-here
```

## Running

```bash
# Production
npm start

# Development (with auto-reload)
npm run dev
```

Server listens on port `3000`.

## API

### `POST /verify-medication`

Analyzes a base64-encoded JPEG image and returns whether it contains medication.

**Request**

```json
{
  "base64Image": "<base64-encoded JPEG string>"
}
```

**Response**

```json
{
  "contains_medication": true,
  "confidence": "high"
}
```

| Field | Type | Values |
|---|---|---|
| `contains_medication` | boolean | `true` / `false` |
| `confidence` | string | `"high"` / `"medium"` / `"low"` |

**Limits:** Image payload up to 10MB.

**Error responses**

| Status | Body |
|---|---|
| `400` | `{ "error": "No image provided" }` |
| `500` | `{ "error": "Verification failed" }` |

---

### `GET /health`

Liveness check.

```json
{ "status": "ok" }
```

## Stack

- [Express 5](https://expressjs.com/) — HTTP server
- [Google Generative AI SDK](https://www.npmjs.com/package/@google/generative-ai) — Gemini `gemini-2.5-flash-lite` model
- [dotenv](https://www.npmjs.com/package/dotenv) — environment variable loading
- [cors](https://www.npmjs.com/package/cors) — cross-origin requests
