# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
npm install

# Start the server
node index.js

# The server runs on PORT env var or defaults to 3000
```

No test runner is configured (`npm test` exits with an error).

## Architecture

Single-file Express server (`index.js`) that exposes two endpoints:

- `POST /verify-medication` — accepts `{ base64Image: string }` (JPEG, up to 10MB), sends it to Claude via the Anthropic SDK using vision, and returns `{ contains_medication: bool, confidence: "high"|"medium"|"low" }`.
- `GET /health` — liveness check.

The Claude call uses `claude-sonnet-4-6` with `max_tokens: 100`. The response text is regex-parsed to extract a JSON object (`/\{.*\}/s`) before being returned, handling any stray text around the JSON.

## Environment

Requires `ANTHROPIC_API_KEY` in `.env` (loaded via `dotenv`). The `.env` file is not committed — create it locally with:

```
ANTHROPIC_API_KEY=sk-ant-...
PORT=3000
```
