# QueueStorm Investigator

AI/API SupportOps Copilot — SUST CSE Carnival 2026 · Codex Community Hackathon

## Setup & Run

```bash
npm install
cp .env.example .env
# .env ফাইলে GEMINI_API_KEY বসাও
node server.js
```

## Endpoints

- `GET  /health`          → `{"status":"ok"}`
- `POST /analyze-ticket`  → Structured JSON analysis

## Tech Stack

- Node.js + Express
- Google Gemini 1.5 Flash (primary AI reasoning) — Free API
- Rule-based fallback (if Gemini unavailable)

## AI Approach

Uses Gemini 1.5 Flash for intelligent investigation:
- Cross-references complaint text with transaction history
- Detects inconsistent patterns (e.g. repeated counterparty = not a wrong transfer)
- Handles Bangla/Banglish complaints natively
- Prompt-injection resistant (complaint treated as DATA only)
- Falls back to deterministic rule-based logic if API fails

## Safety Logic

- customer_reply never asks for PIN, OTP, password
- Never promises refund — uses "any eligible amount will be returned through official channels"
- Post-generation safety check strips any accidental credential requests
- Phishing cases always escalate to fraud_risk with critical severity

## MODELS

| Model | Where | Why |
|-------|-------|-----|
| gemini-1.5-flash | Google AI Studio (cloud, free tier) | Fast, free, strong reasoning, good Bangla support |

## Environment Variables

```
GEMINI_API_KEY=  # Required — get free from aistudio.google.com
PORT=3000        # Optional, defaults to 3000
```

## Deploy on Render/Railway

1. Push code to GitHub
2. Create new Web Service
3. Add environment variable: `GEMINI_API_KEY=your_key`
4. Build command: `npm install`
5. Start command: `node server.js`

## Limitations

- Depends on Gemini API availability (rule-based fallback covers outages)
- Gemini free tier has rate limits (60 requests/minute) — sufficient for hackathon judging
- Bangla detection based on `language` field
