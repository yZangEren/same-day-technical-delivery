# API Docs Sprint — sample handoff

This is a short sample of the Markdown handoff delivered for a fixed-scope API documentation sprint. It uses a fictional endpoint so no private client data is exposed.

## Create a checkout session

`POST /v1/checkout/sessions`

### Request

```json
{
  "currency": "USD",
  "items": [{"sku": "starter", "quantity": 1}],
  "success_url": "https://example.test/paid"
}
```

### Required checks

- `currency` must be supported and uppercase.
- `items` must contain at least one item; `quantity` must be a positive integer.
- `success_url` must use HTTPS in production.
- The response must include a unique `session_id` and an absolute `checkout_url`.

### Success response — `201 Created`

```json
{
  "session_id": "cs_sample_123",
  "checkout_url": "https://pay.example.test/cs_sample_123",
  "expires_at": "2026-10-03T12:00:00Z"
}
```

### Failure cases

| Status | Condition | Client action |
| --- | --- | --- |
| `400` | Invalid item or currency | Show the field-level error and keep the form editable. |
| `401` | Missing or expired API key | Refresh credentials; never retry with a client secret exposed in the browser. |
| `409` | Idempotency key already used with a different payload | Ask the caller to generate a new key. |
| `429` | Rate limit reached | Retry with the documented backoff and preserve the idempotency key. |

## Acceptance checklist

- [ ] Endpoint, authentication, and required headers are documented.
- [ ] A working request and response example is included.
- [ ] Validation, error statuses, and retry behavior are explicit.
- [ ] One happy-path request and one failure path are verified against the supplied build.
- [ ] The final handoff includes open questions and a short release-note entry.

