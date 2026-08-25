# OAuth 2.0 setup — Zoho Invoice API

Zoho uses standard OAuth 2.0 authorization-code flow. All steps below are one-time per org/user, except step 4 (refreshing), which you do every time an access token expires (~hourly).

## Data centers

Use the accounts domain matching the user's Zoho Invoice DC (check the URL they use to log in — `invoice.zoho.com` = `.com`, `invoice.zoho.in` = `.in`, etc.):

| Data Center | Domain | Accounts (OAuth) base URI | API base URI |
|---|---|---|---|
| United States | .com | https://accounts.zoho.com/ | https://www.zohoapis.com/invoice/ |
| Europe | .eu | https://accounts.zoho.eu/ | https://www.zohoapis.eu/invoice/ |
| India | .in | https://accounts.zoho.in/ | https://www.zohoapis.in/invoice/ |
| Australia | .com.au | https://accounts.zoho.com.au/ | https://www.zohoapis.com.au/invoice/ |
| Japan | .jp | https://accounts.zoho.jp/ | https://www.zohoapis.jp/invoice/ |
| Canada | .ca | https://accounts.zohocloud.ca/ | https://www.zohoapis.ca/invoice/ |
| China | .com.cn | (not listed separately — use zoho.com.cn accounts) | https://www.zohoapis.com.cn/invoice/ |
| Saudi Arabia | .sa | (not listed separately) | https://www.zohoapis.sa/invoice/ |

Everything below uses `.com`; substitute the right domain otherwise.

## Step 1 — Register a client

Browser step, done by the user (or you walking them through it):
1. Go to `https://accounts.zoho.com/developerconsole`.
2. Click **Add Client ID**, fill in the required details (client type: server-based apps for most integrations).
3. You get a `client_id` and `client_secret`. Treat `client_secret` like a password — never print it back in full, never commit it to code you hand the user.

## Step 2 — Get a grant `code`

**Method 1 — full redirect flow** (for a real web app with a callback URL):

Redirect the user to:
```
https://accounts.zoho.com/oauth/v2/auth?
  scope={comma-separated scopes}
  &client_id={client_id}
  &state={opaque string, round-tripped back to you}
  &response_type=code
  &redirect_uri={must match what was registered}
  &access_type=offline
  &prompt=Consent
```
Example:
```
https://accounts.zoho.com/oauth/v2/auth?scope=ZohoInvoice.invoices.CREATE,ZohoInvoice.invoices.READ,ZohoInvoice.invoices.UPDATE,ZohoInvoice.invoices.DELETE&client_id=1000.XXXX&state=testing&response_type=code&redirect_uri=http://www.example.com/callback&access_type=offline
```
On accept, Zoho redirects to `redirect_uri` with `?code=...&state=...`. **The `code` is valid for only 60 seconds** — exchange it immediately (step 3).

**Method 2 — Self Client** (fastest for personal/server scripts, no redirect URI needed):
1. In the developer console, open the client's overflow menu → **Self Client**.
2. Enter the scopes and an expiry time for the code.
3. Click **View Code** to generate the grant code directly (no browser redirect involved).

Use Method 2 whenever the user just wants to script against their own Zoho Invoice account — it's simpler and avoids setting up a redirect endpoint.

## Step 3 — Exchange the code for tokens

```
POST https://accounts.zoho.com/oauth/v2/token
  ?code={code from step 2}
  &client_id={client_id}
  &client_secret={client_secret}
  &redirect_uri={same redirect_uri used in step 2, or the placeholder used for Self Client}
  &grant_type=authorization_code
```
Response includes `access_token`, `refresh_token` (only present because `access_type=offline` was used), and `expires_in` (seconds, typically 3600).

**Store the `refresh_token`** — it doesn't expire on its own and lets you skip steps 1–3 forever after this. Max 20 refresh tokens per user; the oldest is silently dropped once you exceed that, regardless of whether it's still in use — don't over-generate them.

## Step 4 — Refresh an access token

Do this whenever a call 401s, or proactively before a long-running job:
```
POST https://accounts.zoho.com/oauth/v2/token
  ?refresh_token={refresh_token}
  &client_id={client_id}
  &client_secret={client_secret}
  &redirect_uri={same as before}
  &grant_type=refresh_token
```
Returns a fresh `access_token` (no new refresh token — keep reusing the one you already have).

## Step 5 — Revoke a token (cleanup / user offboarding)

```
POST https://accounts.zoho.com/oauth/v2/token/revoke?token={access_or_refresh_token}
```

## Calling the API with the token

```
Authorization: Zoho-oauthtoken {access_token}
```
Header only — never as a query/URL param.

## Scopes cheat sheet

Format: `ZohoInvoice.{module}.{ACTION}`, comma-separated for multiple. Actions: `CREATE`, `READ`, `UPDATE`, `DELETE` (or `.fullaccess.all` for everything).

| Module | Covers |
|---|---|
| `contacts` | Contacts API |
| `settings` | Items, expense categories, users, taxes, currencies |
| `estimates` | Estimates API |
| `invoices` | Invoices API |
| `customerpayments` | Customer Payments API |
| `creditnotes` | Credit Notes API |
| `projects` | Projects API |
| `expenses` | Expenses API |

Request only what the task needs, e.g. a "read-only invoice dashboard" task needs just `ZohoInvoice.invoices.READ,ZohoInvoice.contacts.READ`.