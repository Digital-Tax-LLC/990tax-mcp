# 990.tax

Two MCP servers from 990.tax (Digital Tax LLC, an IRS-authorized e-file provider):

- `990tax` (`https://990.tax/mcp`): free lookups of IRS public records on US tax-exempt organizations. No sign-in; every tool is read-only.
- `990tax-filing` (`https://app.990.tax/mcp`): starts and checks a Form 990-N (e-Postcard) draft in the user's 990.tax account. It needs a sign-in: run `/mcp auth 990tax-filing`.

Rules:
- No tool files a return, takes a payment, signs or deletes anything. The organization's officer reviews, confirms and pays on 990.tax. Never say a return is filed until `get_filing_status` reports the IRS accepted it.
- State due dates with the form and tax year they apply to, and pass on the 990.tax link each answer includes.
- 990.tax is a private company, not the IRS; answers are information from IRS records and rules, not tax advice.

The `nonprofit-lookup` and `form-990-filing` skills give step-by-step instructions.
