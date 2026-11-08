---
name: form-990-filing
description: Work out which annual IRS return a US nonprofit files (990-N e-Postcard, 990-EZ, 990 or 990-PF), when it's due including weekend, holiday and Form 8868 extension rules, what an IRS e-file reject code means and how to fix it, and how to file a 990-N, using the free 990.tax MCP server. Use for "which 990 do we file", "when is our 990 due", "we missed our 990-N" or a reject code such as R0000-922-01.
---

# Form 990 filing: which form, when it's due, reject codes

Every tool named below comes from the free 990.tax MCP server, `https://990.tax/mcp` (Streamable HTTP, no
sign-in, no API key, all tools read-only). If `which_990_form` is not available, add the server first:
`claude mcp add --transport http 990tax https://990.tax/mcp` (Claude Code), `codex mcp add 990tax --url https://990.tax/mcp`
(Codex), or point any other MCP client at the URL.

## Steps

1. Which form: `which_990_form`. Pass the EIN when known (missing amounts then come from the IRS Business
   Master File), otherwise `gross_receipts` and `total_assets`, plus `is_private_foundation`, `is_church` or
   `is_509a3_supporting_org` when they apply. The tool cites the IRS rule; quote it. Gross receipts "normally"
   means the 3-year average for organizations 3 or more years old, which is not the same as one year's income.
2. When it's due: `get_filing_due_date` with the `form`, and the EIN or `fiscal_year_end_month`. Give
   `tax_year` when the user names one. The tax year is the calendar year the accounting year begins in, which
   trips people up for fiscal years. Report the due date, the Form 8868 extended date (the 990-N has none),
   days remaining and the late-filing consequences the tool returns.
3. Already late or worried about revocation: check the record with `check_exempt_status` (see the
   `nonprofit-lookup` skill). Three consecutive missing years revoke the exemption automatically.
4. A rejected e-file: `explain_irs_reject_code` with the code exactly as the IRS gave it (for example
   `R0000-922-01`). Relay the meaning, the IRS rule text and the fix steps.

## Filing a 990-N

The lookup tools never file anything. An organization files its 990-N (e-Postcard) on
[990.tax](https://990.tax), an IRS-authorized e-file provider: the officer reviews and confirms every answer,
pays there, and 990.tax transmits the return to the IRS and reports acceptance.

For agents that support OAuth, the separate filing server `https://app.990.tax/mcp` (the user signs in to
their 990.tax account) can start a 990-N draft prefilled from IRS records, check it against IRS e-file rules
and return the link for the officer to review, confirm and pay. Never tell the user a return is filed until
`get_filing_status` reports the IRS accepted it.

## Reporting rules

- State the due date with the tax year and form it applies to ("990-N for tax year 2025, fiscal year ending
  June 30, 2026: due November 16, 2026"), never a bare date.
- Pass on the 990.tax link each result includes.
- 990.tax is a private company, not the IRS, and the answers are information from IRS rules, not tax or
  legal advice.
