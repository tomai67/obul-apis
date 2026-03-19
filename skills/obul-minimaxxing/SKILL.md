---
name: obul-minimaxxing
description: "USE THIS SKILL WHEN: the user wants to generate text using MiniMax M2.5 chat completions. Provides pay-per-request access to MiniMax's large language model via x402 USDC micropayments on Base."
homepage: https://www.minimaxi.me
metadata:
  obul-skill:
    emoji: "🧠"
    requires:
      env: [ "OBUL_API_KEY" ]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# MiniMax

MiniMax provides high-performance chat completions through their M2.5 model. Through the Obul proxy, each request is paid individually in USDC via x402 micropayments on Base — no crypto wallet or gas fees required.

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

Base URL: `https://proxy.obul.ai/proxy/https/chat.minimaxi.me`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Chat Completions

Send a chat completion request using the OpenAI-compatible API.

**Pricing:** $0.001 per request

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/chat.minimaxi.me/v1/chat/completions",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "model": "MiniMax-M2.5",
    "messages": [
      {
        "role": "user",
        "content": "Hello"
      }
    ]
  }
}
```

## Endpoint Pricing Reference

| Endpoint | Price | Purpose |
|----------|-------|---------|
| `POST /v1/chat/completions` | $0.001/request | Generate chat completions |

## When to Use

- **Text generation** — Generate text for any use case
- **Conversational AI** — Build chatbots
- **Content creation** — Write articles, summaries, code

## Error Handling

| Error | Cause | Solution |
|-------|-------|----------|
| `402 Payment Required` | Insufficient balance | Verify your OBUL_API_KEY is valid at my.obul.ai |
| `400 Bad Request` | Invalid request | Ensure required fields are present |
