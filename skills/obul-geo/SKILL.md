---
name: obul-geo
description: "USE THIS SKILL WHEN: the user wants to track brand visibility across AI search engines, check how LLMs mention a brand, analyze sentiment in AI responses, or measure share of voice against competitors. Provides GEO (Generative Engine Optimization) tracking via the Obul proxy."
homepage: https://geo.x402endpoints.com
metadata:
  obul-skill:
    emoji: "📊"
    requires:
      env: ["OBUL_API_KEY"]
      primaryEnv: "OBUL_API_KEY"
registries: {}
---

# GEO Visibility Tracking

GEO (Generative Engine Optimization) visibility tracking service. Fan out prompts to 5 LLMs (Perplexity, OpenAI, Gemini, Claude, Grok), then analyze brand mentions, sentiment, citations, and share of voice. Through the Obul proxy, each request is paid individually — no separate API keys required.

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

Base URL: `https://proxy.obul.ai/proxy/https/geo.x402endpoints.com`

To get an Obul API key, sign up at **https://my.obul.ai**.

## Common Operations

### Check Brand Visibility

Fan out a prompt to 5 LLMs and analyze brand mentions, sentiment, citations, and competitor presence.

**Pricing:** $0.05

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/v1/visibility/check",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "prompt": "best API platforms for AI developers",
    "brand": "Obul",
    "aliases": ["obul.ai"],
    "competitors": ["RapidAPI", "OpenRouter"],
    "models": ["perplexity", "openai", "gemini", "claude", "grok"]
  }
}
```

**Response:** JSON with per-model results (mention position, sentiment, citations, competitor counts) and a summary including mention rate, average position, dominant sentiment, and share of voice.

### Suggest Prompts

Generate AI-powered prompt suggestions that would lead to brand mentions at a given funnel stage.

**Pricing:** $0.01

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/v1/prompts/suggest",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "url": "https://obul.ai",
    "brand": "Obul",
    "stage": "consideration"
  }
}
```

**Response:** JSON with an array of suggested prompts, each with a stage label and rationale.

### Validate Citations

Check if citation URLs from LLM responses are live, whether they mention the brand, and classify domains as own/competitor/neutral.

**Pricing:** $0.001

```json
{
  "method": "POST",
  "url": "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/v1/citations/analyze",
  "headers": {
    "Content-Type": "application/json",
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  },
  "body": {
    "urls": ["https://techcrunch.com/article", "https://blog.example.com/post"],
    "brand": "Obul",
    "aliases": ["obul.ai"],
    "competitors": ["RapidAPI", "OpenRouter"],
    "ownDomains": ["obul.ai", "my.obul.ai"]
  }
}
```

**Response:** JSON array with each URL's live status, HTTP status code, brand mention count, domain, and domain classification (own/competitor/neutral).

### Get Pricing

Retrieve current per-endpoint pricing.

**Pricing:** $0.00

```json
{
  "method": "GET",
  "url": "https://proxy.obul.ai/proxy/https/geo.x402endpoints.com/pricing",
  "headers": {
    "x-obul-api-key": "{{OBUL_API_KEY}}"
  }
}
```

**Response:** JSON with pricing for each endpoint.

## Endpoint Pricing Reference

| Endpoint                 | Price  | Purpose                                      |
|--------------------------|--------|----------------------------------------------|
| `POST /v1/visibility/check` | $0.05  | Brand visibility check across 5 LLMs        |
| `POST /v1/prompts/suggest`  | $0.01  | AI-powered prompt suggestions                |
| `POST /v1/citations/analyze` | $0.001 | Citation URL validation                      |
| `GET /pricing`           | $0.00  | Current pricing info                         |
| `GET /health`            | $0.00  | Health check                                 |
| `GET /openapi.json`      | $0.00  | OpenAPI 3.1.0 specification                  |

## When to Use

- **Brand monitoring** — Track how AI search engines mention your brand
- **Competitor analysis** — Compare share of voice against competitors across LLMs
- **GEO optimization** — Identify which prompts lead to brand mentions and which don't
- **Sentiment tracking** — Monitor how LLMs describe your brand (positive, negative, neutral)
- **Citation validation** — Verify if URLs cited by LLMs are real and mention your brand
- **Prompt research** — Generate prompts to test brand visibility at different funnel stages

## Best Practices

- **Include aliases** — Add common misspellings and alternative names for better mention detection
- **List competitors** — Providing competitors enables share of voice and competitor tracking
- **Test all funnel stages** — Use prompt suggest with "awareness", "consideration", and "decision" stages
- **Batch citation checks** — Analyze up to 10 URLs per request to validate LLM citations efficiently

## Error Handling

| Error                    | Cause                                    | Solution                                                                                 |
|--------------------------|------------------------------------------|------------------------------------------------------------------------------------------|
| `402 Payment Required`   | Payment not processed or insufficient    | Verify your OBUL_API_KEY is valid and your account has sufficient balance at my.obul.ai. |
| `400 Bad Request`        | Missing required fields                  | Ensure `prompt` and `brand` are provided for visibility check.                           |
| `429 Too Many Requests`  | Rate limit exceeded                      | Add a short delay between requests. Limit is 20 requests per 60 seconds.                 |
| `502 Bad Gateway`        | Upstream LLM error                       | One or more LLMs failed. Check the `errors` array in the response for details.           |
