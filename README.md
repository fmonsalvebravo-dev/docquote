# DocQuote Platform

DocQuote Platform is an AI-native document generation API that generates professional PDF documents from structured JSON: sales quotes, proforma invoices, invoices, credit notes, receipts, and purchase orders.
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
- Normal: `quote → proforma (optional) → invoice`
- Post-payment: `invoice → receipt` (to confirm payment received)
- Correction: `invoice → credit note` (to reduce or cancel a prior invoice)

Endpoints: `/v1/quote/pdf` · `/v1/proforma/pdf` · `/v1/invoice/pdf` · `/v1/receipt/pdf` · `/v1/credit-note/pdf`

**Procurement workflow** (issued by the buying organization):
- Purchase order (PO): `/v1/po/pdf`

Document types are independent — use any combination as needed. One API key and shared credits work across all types.

> **Note:** DocQuote generates commercial documents such as quotes, purchase orders, and invoices. These documents are **not tax-compliant electronic invoices for jurisdictions that require government e-invoicing systems** (for example Chile SII, Mexico SAT, Brazil NF-e, etc.). If legal tax invoices are required, the generated document should be treated as a commercial draft and processed through the appropriate government e-invoicing system.

## Recommended agent workflow

1. Register an API key with `POST /v1/keys/register`
2. Validate with the appropriate `/preview` endpoint (no auth, no billing)
3. Generate PDF with the appropriate `/pdf` endpoint using `x-api-key` and `Idempotency-Key`
4. Purchase credits with `POST /v1/credits/checkout` when free quota is exhausted

## Core capabilities

- Generate quote, proforma invoice, invoice, receipt, credit note, and purchase order PDFs from structured JSON
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

DocQuote supports multilingual content across all document types. Agents may send company names, customer names, item descriptions, and notes in any UTF-8 language. The API renders text exactly as provided in the JSON payload.

## Using DocQuote with Claude Code

You can generate quotes, proforma invoices, invoices, receipts, credit notes, and purchase orders by describing what you need in plain language. Claude Code will use the DocQuote API at https://api.docquote.dev to produce the PDF.

**Try the demo (no account needed):**

> Please generate a sales quote using the DocQuote API at https://api.docquote.dev. My company is Acme Corp, the customer is Jane Smith at Globex Ltd, and the items are 5 hours of consulting at $150/hr and a software license for $499. Save the PDF as quote.pdf.

**Generate with your own API key:**

> Using the DocQuote API at https://api.docquote.dev and my API key dq_live_..., generate a sales quote for my company TechCo (billing@techco.com), customer Widgets Inc, for 10 units of "Premium Support" at $200 each plus 8% tax. Use Idempotency-Key "quote-widgets-001" and save it as quote-widgets.pdf.

Claude Code can discover the full API documentation automatically by reading https://api.docquote.dev/llms.txt.

## Notes

- All `/preview` endpoints are public — no API key required
- Protected `/pdf` and billing endpoints require `x-api-key` header
- Autonomous agents should call `/preview` before `/pdf`
- Use `Idempotency-Key` on `/pdf` requests — safe to retry without double billing

