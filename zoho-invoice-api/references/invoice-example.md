# Invoices — worked example (field-complete)

Source: `https://www.zoho.com/invoice/api/v3/invoices/` (confirmed from live docs). Use this as a template — strip fields you don't need, especially the country-specific ones (🇮🇳/🇬🇧/🇲🇽/🇺🇸/🇨🇦-only).

## Create an invoice

`POST /invoices` — scope `ZohoInvoice.invoices.CREATE`

Required fields: `customer_id`, `date`, `line_items` (each line item needs `item_id`, `rate`, `quantity`).

Query params: `send=true` (email it immediately on creation), `ignore_auto_number_generation=true` (lets you set `invoice_number` manually).

```json
{
  "customer_id": "982000000567001",
  "contact_persons": ["982000000870911"],
  "invoice_number": "INV-00003",
  "reference_number": "PO-1234",
  "date": "2025-11-17",
  "payment_terms": 15,
  "due_date": "2025-12-03",
  "discount": "10%",
  "is_discount_before_tax": true,
  "discount_type": "item_level",
  "is_inclusive_tax": false,
  "line_items": [
    {
      "item_id": "982000000030049",
      "name": "Hard Drive",
      "description": "500GB, USB 2.0 interface",
      "rate": 120,
      "quantity": 1,
      "unit": "pcs",
      "discount": 0,
      "tax_id": "982000000557028"
    }
  ],
  "notes": "Looking forward for your business.",
  "terms": "Terms & Conditions apply",
  "shipping_charge": 0,
  "adjustment": 0
}
```

Minimal version (no discounts/tax/etc.):
```json
{
  "customer_id": "982000000567001",
  "date": "2025-11-17",
  "line_items": [
    { "item_id": "982000000030049", "rate": 120, "quantity": 1 }
  ]
}
```

Response: `201 Created`, `{ "code": 0, "message": "The invoice has been created.", "invoice": { ...full object, invoice_id, status: "draft", sub_total, tax_total, total, balance, invoice_url, ... } }`

## Update an invoice

`PUT /invoices/{invoice_id}` — same body shape as create, `line_items` required (omit a line item to delete it — there is no separate delete-line-item endpoint).

## List invoices — useful filters

`GET /invoices?status=unpaid&customer_id=...&date_start=2025-01-01&date_end=2025-12-31&sort_column=due_date&page=1&per_page=200`

Key filters: `status` (`sent`/`draft`/`overdue`/`paid`/`void`/`unpaid`/`partially_paid`/`viewed`), `filter_by` (`Status.All`, `Status.Unpaid`, `Date.PaymentExpectedDate`, etc.), `customer_id`, `customer_name`, `search_text` (matches invoice number/PO/customer name), `date`/`due_date` with `_start`/`_end`/`_before`/`_after` variants.

## Line item fields worth knowing

| Field | Notes |
|---|---|
| `item_id` | Required — must already exist (create via `POST /items` first) |
| `rate` | Required, per-unit price |
| `quantity` | Required |
| `discount` | String — `"200"` = flat amount, `"10%"` = percentage |
| `tax_id` | ID of a tax or tax group (`GET /settings/taxes` to list) |
| `product_type` | `goods` or `services` |
| `project_id` / `time_entry_ids` | For project/time-billing invoices |

## Get an invoice as PDF

`GET /invoices/{invoice_id}` with header `Accept: application/pdf` (or query `?accept=pdf`). Response is a binary PDF with `Content-Disposition: attachment; filename="INV-xxx.pdf"` — not JSON.

## Body-level error codes (Invoices)

| Code | Message | Meaning |
|---|---|---|
| 1001 | Invoice Number already exist | Pick a different `invoice_number` or let auto-numbering handle it |
| 1002 | Contact Person does not exist | Bad `contact_persons` ID |
| 3009 | Contact details can't be edited — credits applied | Remove applied credits first, or don't touch `customer_id`/contact fields |
| 3010 | Customer can't be changed — payments recorded | Same — payments lock the customer field |
| 4001 | Payments recorded — can't delete | Void instead of delete |
| 12008 | Credits applied — can't delete | Void instead of delete |