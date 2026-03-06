# DocQuote

DocQuote is an AI-native API that generates professional sales quote PDFs from structured JSON.
Built for autonomous agents, workflow automation, and developer integrations.

## Machine discovery

- Landing: https://api.docquote.dev/
- Identity: https://api.docquote.dev/identity
- OpenAPI schema: https://api.docquote.dev/v1/schema
- Agent documentation: https://api.docquote.dev/docs/AGENTS.md
- Agent index: https://api.docquote.dev/agent-index.json
- Agent manifest: https://api.docquote.dev/.well-known/agent.json
- AI plugin manifest: https://api.docquote.dev/.well-known/ai-plugin.json
- llms.txt: https://api.docquote.dev/llms.txt

## Recommended agent workflow

1. Register an API key with `POST /v1/keys/register`
2. Validate the payload with `POST /v1/quote/preview`
3. Generate the final PDF with `POST /v1/quote/pdf` using `Idempotency-Key`
4. Purchase credits with `POST /v1/credits/checkout` when free quota is exhausted

## Core capabilities

- Generate comercial quote PDFs from structured JSON
- Free preview validation endpoint
- Idempotent PDF generation
- Agent-friendly discovery documents
- Credit-based usage after free tier

## Example discovery query

```bash
curl -s https://api.docquote.dev/identity
```

## Example schema fetch

```bash
curl -s https://api.docquote.dev/v1/schema
```

## Notes

- `POST /v1/quote/preview` is public — no API key required
- Protected generation and billing endpoints require `x-api-key` header
- Autonomous agents should call `/preview` before `/pdf`
- 
- DocQuote supports multilingual content. All text fields (company names, item descriptions, notes, etc.) accept any language supported by UTF-8.
