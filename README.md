# 990.tax MCP servers

Remote [Model Context Protocol](https://modelcontextprotocol.io) servers from **990.tax** (Digital Tax LLC, an IRS-authorized e-file provider) for Claude, ChatGPT, Gemini, Grok, Cursor, VS Code and any MCP client. This repository holds the documentation and client configuration only; the servers run at 990.tax.

| Server | URL | Sign-in | What it does |
|---|---|---|---|
| **Nonprofit lookup** (free) | `https://990.tax/mcp` | none | IRS public records on 1.9 million+ US tax-exempt organizations: exempt status, revocations, missing filings, Form 990 financials and officer pay, which form to file and when it's due, statistics, rankings, IRS e-file reject codes |
| **Form 990-N filing** | `https://app.990.tax/mcp` | your 990.tax account (OAuth) | Start a Form 990-N (e-Postcard) draft prefilled from IRS records, fill in the answers, check it against IRS e-file rules, get the link to review, confirm and pay on 990.tax, and track IRS acceptance |

No tool on either server files a return, takes a payment, signs or deletes anything. Filing happens on 990.tax after the organization's officer reviews and confirms every answer and pays there.

## Connect

### Claude (claude.ai, desktop, mobile)
Settings → Connectors → **Add custom connector**. Name `990.tax`, URL `https://990.tax/mcp` (lookup) or `https://app.990.tax/mcp` (filing; Claude opens 990.tax to sign in).

### Claude Code
```bash
claude mcp add --transport http 990tax https://990.tax/mcp
claude mcp add --transport http 990tax-filing https://app.990.tax/mcp
```

### ChatGPT
Settings → Apps → Advanced settings → **Developer mode** → Create app → URL `https://990.tax/mcp`, authentication none. (For the filing server use `https://app.990.tax/mcp` with OAuth.)

### Gemini, Grok
Gemini: through Google's connected apps when 990.tax is listed. Grok: Connectors → New Connector → Custom → URL.

### Cursor
`~/.cursor/mcp.json` — see [`.mcp.json`](.mcp.json):
```json
{ "mcpServers": { "990tax": { "url": "https://990.tax/mcp" }, "990tax-filing": { "url": "https://app.990.tax/mcp" } } }
```

### VS Code
`.vscode/mcp.json`:
```json
{ "servers": { "990tax": { "type": "http", "url": "https://990.tax/mcp" }, "990tax-filing": { "type": "http", "url": "https://app.990.tax/mcp" } } }
```

## Tools

**Nonprofit lookup** (`https://990.tax/mcp`), all read-only:
`search_nonprofits` · `get_nonprofit_profile` · `check_exempt_status` · `which_990_form` · `get_filing_due_date` · `get_nonprofit_stats` · `get_top_nonprofits` · `explain_irs_reject_code`

**Form 990-N filing** (`https://app.990.tax/mcp`):
`list_my_filings` · `start_990n_draft` · `update_990n_draft` · `check_990n_draft` · `get_990n_review_link` · `get_filing_status`

## Example prompts
- Is the Laramie Rotary Foundation in Wyoming still tax-exempt, and are any years missing from IRS records?
- Our PTO had about $30,000 in receipts with a June 30 year end. Which 990 do we file for tax year 2025 and when is it due?
- How many nonprofits in Texas file the 990-N, and which Texas nonprofits pay their top executive the most?
- Show five years of revenue and expenses for the American Red Cross.
- What does IRS reject code R0000-922-01 mean and how do we fix it?
- (filing) Start our 990-N for tax year 2024, EIN 12-3456789, and check it for errors.
- (filing) Did the IRS accept our 990-N?

## Data, privacy, support
- Data: IRS public files (EO Business Master File, e-Postcard file, automatic revocation list, Publication 78, e-filed Form 990 returns), refreshed monthly. Every answer names its source file and date and links the 990.tax page.
- Privacy: https://990.tax/mcp#privacy and https://990.tax/privacy-policy. Terms: https://990.tax/terms-of-service.
- Support: team@990.tax. Security: https://990.tax/.well-known/security.txt.
- Docs: https://990.tax/mcp. MCP Registry: `tax.990/nonprofit-lookup`, `tax.990/990n-filing`.

990.tax is a private company, not the IRS. Answers are information from IRS records and rules, not tax advice.

Digital Tax LLC · 30 N Gould St Ste N, Sheridan, WY 82801
