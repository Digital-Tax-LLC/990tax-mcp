# Google Connected Apps listing (Gemini app and AI Mode in Search)

Google's Connected Apps Platform puts an MCP server into the Gemini app (web, Android, iOS) and AI Mode in
Search. Users call the app with `@990.tax` or turn it on under Settings & help → Connected apps. Apps are
registered, tested and sent for review in the Google Connected Console (https://connected.google.com/console).
Developer guide: https://developers.google.com/connected-apps/gpc/perform-the-steps.

Each connected app has one MCP endpoint, so 990.tax registers the lookup server first. The filing server follows
as a second app once its OAuth meets Google's account-linking rules (see "Filing server" below).

## App 1: 990.tax (nonprofit lookup)

| Console field | Value |
|---|---|
| App name | 990.tax |
| Connection type | MCP server |
| Category | Small business finance (the console offers no tax or nonprofit category; this cannot be changed later) |
| Endpoint | `https://990.tax/mcp` (Streamable HTTP) |
| Access credentials | No: public, no sign-in |
| Icon | [`logo-512.png`](../logo-512.png) (PNG, 512 × 512, 19 KB) |
| Terms of Service URL | https://990.tax/terms-of-service |
| Policy URL | https://990.tax/privacy-policy |
| Website URL | https://990.tax/mcp |
| Domain verification | `google-connected-apps.json` from the console, served at `https://990.tax/.well-known/google-connected-apps` with HTTP 200 and no redirect |
| Countries | United States |
| Products | Gemini app and Search |

**Description** (one sentence, under 30 words):
IRS public records on 1.9 million+ US nonprofits: exempt status, revocations, missing filings, Form 990 financials and pay, which 990 to file and when it's due.

**Capabilities:**
- Look up any US nonprofit by name or EIN
- Check tax-exempt status, automatic revocations and missing filing years
- Show Form 990 revenue, expenses, assets and officer pay
- Work out which Form 990 an organization files and when it's due
- Nonprofit statistics and rankings by state and category
- Explain IRS e-file reject codes and how to fix them

**Example prompts:**
- @990.tax is the Laramie Rotary Foundation in Wyoming still tax-exempt?
- @990.tax which 990 does a PTO with $30,000 in receipts and a June 30 year end file, and when is it due?
- @990.tax what does IRS reject code R0000-922-01 mean?
- Use 990.tax to show five years of revenue and expenses for the American Red Cross.

**Tools** (all eight are static, annotated `readOnlyHint: true`, `destructiveHint: false`; none needs user
confirmation): `search_nonprofits`, `get_nonprofit_profile`, `check_exempt_status`, `which_990_form`,
`get_filing_due_date`, `get_nonprofit_stats`, `get_top_nonprofits`, `explain_irs_reject_code`.

**Review notes for Google:** the server needs no account; reviewers can run every example prompt as is. No tool
files a return, takes a payment, signs or deletes anything. Answers cite the IRS source file and date and link
the 990.tax page. 990.tax is a private company, not the IRS, and answers are information, not tax advice.

## Filing server (second app, later)

`https://app.990.tax/mcp` uses OAuth 2.1 with PKCE (S256), dynamic client registration and Client ID Metadata
Documents. For it to pass Google's review:

1. Refresh tokens: Google asks that refresh tokens do not rotate, so the account link persists. The 990.tax
   authorization server rotates them today (single use, 30 days). It needs a change in `990-dashboard`
   (`src/lib/mcp/oauth.ts`) before the filing app is submitted.
2. Redirect URIs: allow `https://oauth-redirect.googleusercontent.com/r/*` and Google's sandbox variants.
3. Review account: Google needs a seeded test 990.tax account with two-factor sign-in turned off.
