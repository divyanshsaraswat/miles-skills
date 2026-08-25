# Errors & response format — Zoho Invoice API

## Response envelope

```json
{
  "code": 0,
  "message": "success",
  "<resource_name>": { ... }
}
```
`code: 0` → success. Any other `code` → error; `message` explains it. List endpoints use the plural resource name as the key (`"invoices": [...]`) and add a `page_context` object (see SKILL.md §3.6).

Dates are ISO 8601: `YYYY-MM-DDThh:mm:ssTZD`, e.g. `2016-06-11T17:38:06-0700`.

## HTTP status codes

| Code | Meaning | Notes |
|---|---|---|
| 200 | Success | |
| 201 | Created | Resource created (POST) |
| 400 | Bad request | Malformed or missing parameter |
| 401 | Unauthorized | Invalid/expired access token — refresh it (SKILL.md §2.4) |
| 403 | Forbidden | Not enough permission, or not a user of this org |
| 404 | Not Found | Wrong URL — check `references/endpoints.md` |
| 405 | Method Not Allowed | Wrong HTTP verb for this endpoint |
| 406 | Not Acceptable | Requested response type (`Accept` header) not supported |
| 429 | Too Many Requests | Rate limit hit — 100 req/min/org, or daily plan cap (1000/day on Free) |
| 500 | Server error | Rare; retry with backoff, or contact support@zohoinvoice.com if persistent |

## Rate & concurrency limits

- 100 requests/minute/organization (all plans).
- Daily cap: Free plan 1000 requests/day (paid plans generally higher — check current plan docs if it matters for the task).
- Concurrent in-flight requests per org: 5 (Free), 10 (Paid, soft limit). Batch/parallel jobs should chunk requests rather than firing everything at once — a burst above the concurrent limit returns error code `1070`.

## Common body-level error codes seen across resources

| Code | Meaning |
|---|---|
| 0 | Success |
| 44 | Blocked — too many requests/minute from this account or org |
| 45 | API call limit exceeded for the day |
| 1000 | Internal error |
| 1070 | Too many concurrent in-process requests |

Resource-specific codes (e.g. invoice-only codes) are listed on that resource's doc page — see `references/invoice-example.md` for the invoice ones as an example of the pattern.