---
name: obul-bittensor
description: "USE THIS SKILL WHEN: the user wants to access decentralized AI inference via Bittensor subnets. Provides pay-per-use access to 13 AI subnets (text, image, video, 3D, code, translation, time-series, voice) through a single x402 gateway via the Obul proxy."
homepage: https://github.com/wizerai1111/swarmrails-bittensor
metadata:
  obul-skill:
    emoji: "🧠"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# Bittensor AI Subnets (SwarmRails)

Access 13 specialized Bittensor AI subnets through a single x402 gateway powered by SwarmRails. Bittensor is a
decentralized AI network where specialized subnets handle different AI tasks — text generation, image creation, video
synthesis, 3D modeling, translation, time-series prediction, and more. Through the Obul proxy, each inference request is
paid individually — no SwarmRails account or API key required.

All subnets share a single POST endpoint. The `netuid` parameter selects which subnet handles your request.

## Authentication

All requests route through the Obul proxy. Include your Obul API key in every request:

```json
{
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

Base URL: `https://proxy.obul.ai/proxy/https/xosljjzcpsouwifbclsy.supabase.co/functions/v1/payment_gate`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Subnet Reference

All 13 subnets are accessed via the same POST endpoint. Select the subnet with the `netuid` body parameter. Most subnets
return results synchronously. Subnets 18 (Video) and 29 (3D) are **async** — they return a `job_id` that you poll for
results.

| netuid | Name                 | What It Does                                                       | Price  | Response  |
|--------|----------------------|--------------------------------------------------------------------|--------|-----------|
| 1      | Text Prompting       | General LLM text generation (Q&A, writing, summarization)          | $0.005 | Sync      |
| 3      | Machine Translation  | Translate text between languages                                   | $0.005 | Sync      |
| 4      | Targon (Reasoning)   | Verified LLM responses with source citations and fact-checking     | $0.05  | Sync      |
| 5      | Image Generation     | Text-to-image generation                                           | $0.075 | Sync      |
| 6      | Fine-tuned LLM       | Domain-specialized inference (medical, legal, etc.)                | $0.01  | Sync      |
| 8      | Time Series          | Financial/crypto price predictions (8hr forecasts, 5-min intervals)| $0.05  | Sync      |
| 11     | Code Generation      | AI agent policy optimization via reinforcement learning            | $0.01  | Sync      |
| 13     | Data Analysis        | Structured data extraction and analysis from unstructured input    | $0.005 | Sync      |
| 16     | Voice TTS/Cloning    | Text-to-speech and voice cloning from audio samples                | $0.025 | Sync      |
| 18     | Video Generation     | Text-to-video generation (takes several minutes)                   | $2.00  | **Async** |
| 21     | Web Scraping         | Scrape and extract content from web pages                          | $0.01  | Sync      |
| 24     | Multimodal Reasoning | Image/video understanding combined with text reasoning             | $0.02  | Sync      |
| 29     | 3D Asset Generation  | Generate 3D models (GLB, OBJ, FBX) from text or images            | $0.75  | **Async** |

## Common Operations

### Generate Text (Subnet 1)

General-purpose LLM text generation via the Text Prompting subnet. Handles Q&A, writing, summarization, and any
text-based AI task.

**Pricing:** $0.005

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/xosljjzcpsouwifbclsy.supabase.co/functions/v1/payment_gate",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "netuid": 1,
    "prompt": "Explain the differences between REST and GraphQL APIs in 3 bullet points",
    "agent_id": "my-agent-001"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "netuid": 1,
  "response": "• REST uses fixed endpoints with HTTP methods...\n• GraphQL uses a single endpoint with a query language...\n• REST can over-fetch data while GraphQL returns exactly what you request..."
}
```

### Generate Image (Subnet 5)

Generate images from text descriptions via the Image Generation subnet.

**Pricing:** $0.075

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/xosljjzcpsouwifbclsy.supabase.co/functions/v1/payment_gate",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "netuid": 5,
    "prompt": "A futuristic city skyline at sunset, cyberpunk aesthetic, neon lights reflecting on glass buildings",
    "agent_id": "my-agent-001"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "netuid": 5,
  "image_url": "https://...generated-image-url..."
}
```

### Predict Time Series (Subnet 8)

Financial and crypto price predictions using the Time Series subnet. Returns 8-hour forecasts at 5-minute intervals.

**Pricing:** $0.05

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/xosljjzcpsouwifbclsy.supabase.co/functions/v1/payment_gate",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "netuid": 8,
    "prompt": "BTC/USD price prediction",
    "agent_id": "my-agent-001"
  }
}
```

**Response:**
```json
{
  "status": "success",
  "netuid": 8,
  "predictions": [
    { "timestamp": "2026-03-03T12:00:00Z", "price": 97250.50 },
    { "timestamp": "2026-03-03T12:05:00Z", "price": 97280.75 }
  ],
  "interval": "5m",
  "horizon": "8h"
}
```

### Generate Video — Async (Subnet 18)

Generate video from text via the Video Generation subnet. This is an **async** operation — it returns a `job_id` that
you must poll for completion. Video generation takes several minutes.

**Pricing:** $2.00

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/xosljjzcpsouwifbclsy.supabase.co/functions/v1/payment_gate",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "netuid": 18,
    "prompt": "A drone shot flying over a tropical island with crystal clear water, cinematic lighting",
    "agent_id": "my-agent-001"
  }
}
```

**Response:**
```json
{
  "status": "pending",
  "netuid": 18,
  "job_id": "job-uuid-12345",
  "message": "Video generation started. Poll for status using the job_id."
}
```

### Poll Job Status (Async Results)

Check the status of an async job (video generation or 3D asset generation). Poll every 15-30 seconds until the status is
`completed` or `failed`.

**Pricing:** $0.0001

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/xosljjzcpsouwifbclsy.supabase.co/functions/v1/payment_gate?job_id=job-uuid-12345",
  "headers": {
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response (completed):**
```json
{
  "status": "completed",
  "job_id": "job-uuid-12345",
  "netuid": 18,
  "result_url": "https://...generated-video-url...",
  "duration_seconds": 185
}
```

**Response (pending):**
```json
{
  "status": "pending",
  "job_id": "job-uuid-12345",
  "netuid": 18,
  "progress": 45
}
```

**Status values:** `pending`, `processing`, `completed`, `failed`

## Endpoint Pricing Reference

| Endpoint                          | Price   | Purpose                                                 |
|-----------------------------------|---------|---------------------------------------------------------|
| `POST payment_gate` netuid=1      | $0.005  | Text generation (Q&A, writing, summarization)           |
| `POST payment_gate` netuid=3      | $0.005  | Machine translation between languages                   |
| `POST payment_gate` netuid=4      | $0.05   | Verified reasoning with citations (Targon)              |
| `POST payment_gate` netuid=5      | $0.075  | Text-to-image generation                                |
| `POST payment_gate` netuid=6      | $0.01   | Fine-tuned domain-specific LLM inference                |
| `POST payment_gate` netuid=8      | $0.05   | Time-series financial/crypto predictions                |
| `POST payment_gate` netuid=11     | $0.01   | Code generation and policy optimization                 |
| `POST payment_gate` netuid=13     | $0.005  | Structured data extraction and analysis                 |
| `POST payment_gate` netuid=16     | $0.025  | Text-to-speech and voice cloning                        |
| `POST payment_gate` netuid=18     | $2.00   | Video generation (async — poll for results)             |
| `POST payment_gate` netuid=21     | $0.01   | Web scraping and content extraction                     |
| `POST payment_gate` netuid=24     | $0.02   | Multimodal image/video reasoning                        |
| `POST payment_gate` netuid=29     | $0.75   | 3D asset generation (async — poll for results)          |
| `GET payment_gate?job_id=...`     | $0.0001 | Poll async job status (video, 3D)                       |

## When to Use

- **Decentralized AI inference** — Access Bittensor's network of specialized subnets through a single API.
- **Multiple AI modalities** — Text, image, video, 3D, code, translation, and voice through one endpoint.
- **Time-series financial predictions** — Crypto and financial price forecasting not available in other Obul skills.
- **Verified/fact-checked responses** — Use Targon (netuid 4) for LLM responses with source citations.
- **Voice cloning and TTS** — Generate speech or clone voices from audio samples via subnet 16.
- **3D asset generation** — Create 3D models (GLB, OBJ, FBX) from text or images via subnet 29.

## Best Practices

- **Choose the right netuid** — Each subnet is specialized. Use the Subnet Reference table to match your task to the
  correct `netuid`. Using the wrong subnet will produce poor results.
- **Handle async subnets correctly** — Subnets 18 (Video) and 29 (3D) are async. After submitting, poll the GET
  endpoint every 15-30 seconds until the status is `completed` or `failed`. Do not resubmit the POST request.
- **Start with cheap subnets** — Test your prompts on low-cost subnets (netuid 1 at $0.005) before using expensive ones
  (netuid 18 at $2.00).
- **Write detailed prompts** — More specific prompts produce better results across all subnets. Include context, style,
  and format preferences.
- **Use agent_id for tracking** — Include an `agent_id` in your requests to track usage across multiple agents or
  sessions.
- **Budget for async jobs** — Video ($2.00) and 3D ($0.75) are the most expensive operations. Factor this into your
  agent's budget planning.

## Error Handling

| Error                      | Cause                                       | Solution                                                                                 |
|----------------------------|---------------------------------------------|------------------------------------------------------------------------------------------|
| `402 Payment Required`     | Payment not processed or insufficient       | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai. |
| `400 Bad Request`          | Missing or invalid parameters               | Ensure `netuid` and `prompt` are present. Check that `netuid` is a valid subnet number.  |
| `404 Not Found`            | Invalid job_id in polling request           | Verify the job_id matches the one returned from the POST request.                        |
| `408 Request Timeout`      | Subnet inference took too long              | Retry the request. Some subnets may be slower during high demand.                        |
| `429 Too Many Requests`    | Rate limit exceeded                         | Add delays between requests. Back off and retry after a few seconds.                     |
| `500 Internal Server Error`| Upstream SwarmRails or subnet issue         | Wait and retry. If persistent, the service may be experiencing downtime.                 |
| `failed` status (async)    | Async job failed during processing          | Check your prompt for issues. Try a simpler prompt or retry the request.                 |
| `pending` status (async)   | Job still processing                        | Not an error — continue polling every 15-30 seconds until completed or failed.           |
