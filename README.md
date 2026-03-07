# DocQuote Platform

DocQuote Platform is an AI-native document generation API that generates professional sales quotes, purchase orders, and invoices from structured JSON.
One API key works across all document types; the free tier and credits are shared.
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

## Document workflows

**Sales workflow** (issued by the selling organization):
- Quote → Invoice: `/v1/quote/pdf` then `/v1/invoice/pdf`

**Procurement workflow** (issued by the buying organization):
- Purchase order → supplier invoice: `/v1/po/pdf`

Document types are independent — use any combination as needed. One API key and shared credits work across all types.

## Recommended agent workflow

1. Register an API key with `POST /v1/keys/register`
2. Validate with the appropriate `/preview` endpoint (no auth, no billing)
3. Generate PDF with the appropriate `/pdf` endpoint using `x-api-key` and `Idempotency-Key`
4. Purchase credits with `POST /v1/credits/checkout` when free quota is exhausted

## Core capabilities

- Generate sales quote PDFs, purchase order PDFs, and invoice PDFs from structured JSON
- One API key for all document types; free tier and credits are shared
- Free `/preview` endpoints for validation and calculation (no auth, no billing)
- Idempotent PDF generation (safe to retry without double billing)
- Agent-friendly discovery documents
- Credit-based usage after the 5-generation free tier

## Example discovery query

```bash
curl -s https://api.docquote.dev/identity
```

## Example schema fetch

```bash
curl -s https://api.docquote.dev/v1/schema
```

## Example payloads and curl scripts

Example JSON payloads and ready-to-run curl scripts are available in the [`examples/`](examples/) directory:

| File | Description |
|------|-------------|
| `preview-minimal.json` | Minimal quote payload |
| `preview-full.json` | Full quote payload with all fields |
| `po-minimal.json` | Minimal purchase order payload |
| `po-full.json` | Full purchase order payload |
| `invoice-full.json` | Full invoice payload |
| `quote-curl.sh` | Quote preview + PDF generation examples |
| `po-curl.sh` | Purchase order preview + PDF generation examples |
| `invoice-curl.sh` | Invoice preview + PDF generation examples |

```bash
curl -X POST https://api.docquote.dev/v1/quote/preview \
  -H "Content-Type: application/json" \
  -d @examples/preview-minimal.json
```

## Example request

```bash
curl -X POST https://api.docquote.dev/v1/quote/preview \
  -H "Content-Type: application/json" \
  -d '{
    "company": { "name": "DocQuote Ltd." },
    "customer": { "name": "Acme Corp" },
    "items": [
      { "name": "Installation service", "qty": 1, "unitPrice": 120 }
    ]
  }'
```

Multilingual payload example:

```json
{
  "items": [
    { "name": "Servicio de instalación", "qty": 1, "unitPrice": 120 }
  ]
}
```

## Multilingual support

DocQuote supports multilingual sales quote content. Agents may send company names, customer names, item descriptions, and notes in any UTF-8 language. The API renders text exactly as provided in the JSON payload.

## Notes

- `POST /v1/quote/preview` is public — no API key required
- Protected generation and billing endpoints require `x-api-key` header
- Autonomous agents should call `/preview` before `/pdf`
