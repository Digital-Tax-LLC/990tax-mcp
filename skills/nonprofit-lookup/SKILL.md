---
name: nonprofit-lookup
description: Look up a US nonprofit in IRS records (exempt status, automatic revocation, missing filing years, Form 990 revenue, expenses, assets and officer pay, state and national statistics, rankings) with the free 990.tax MCP server. Use for questions like "is this charity still tax-exempt", "how much does this nonprofit's CEO make" or "how many nonprofits are in Texas".
---

# US nonprofit lookup (IRS records)

Every tool named below comes from the free 990.tax MCP server, `https://990.tax/mcp` (Streamable HTTP, no
sign-in, no API key, all tools read-only). If `search_nonprofits` is not available, add the server first:
`claude mcp add --transport http 990tax https://990.tax/mcp` (Claude Code), `codex mcp add 990tax --url https://990.tax/mcp`
(Codex), or point any other MCP client at the URL.

## Steps

1. Find the organization. Call `search_nonprofits` with the name (or EIN) and, when the user gives one, the
   two-letter `state`. Many names repeat across states and chapters: if more than one match is plausible,
   show the candidates (name, city, state, EIN) and ask which one, rather than picking.
2. With the EIN, choose the tool for the question:
   - Still tax-exempt? Revoked? Years missing? `check_exempt_status`. Three consecutive missing years
     revoke the exemption automatically (IRC 6033(j)).
   - Basic record, deductibility, which return it files, last filing: `get_nonprofit_profile`.
   - Revenue, expenses and assets over time: `get_nonprofit_profile` with `include: ["financials"]`.
   - Officer, director and key employee pay: `get_nonprofit_profile` with `include: ["officers"]`.
   - Every e-Postcard and e-filed return on record: `get_nonprofit_profile` with `include: ["filing_history"]`.
3. Questions about many organizations: `get_nonprofit_stats` (`scope` national, state or an NTEE major
   letter) for counts and totals, `get_top_nonprofits` for ranked lists (highest-paid executives, largest,
   top fundraising, richest, biggest employers), national or per state.

## Reporting rules

- Name the source and its date for every figure. Each result says which IRS file it comes from (EO Business
  Master File, e-Postcard file, automatic revocation list, Publication 78, e-filed returns) and links the
  matching 990.tax page; pass that link on so the user can check it.
- IRS public files lag the IRS by weeks. A return filed recently may not show yet, so say "not on the IRS
  public files as of <date>", never "never filed".
- Financials and pay come only from e-filed Form 990, 990-EZ and 990-PF returns. A 990-N (e-Postcard) filer
  reports no financials; say that instead of reporting zero.
- Fiscal years differ by organization. Label each figure with its tax period, not just a calendar year.
- 990.tax is a private company, not the IRS, and the answers are information from IRS records, not tax or
  legal advice.

## When the user needs to file

If the organization's own return is missing or due, use the `form-990-filing` skill for which form applies,
the due date and how to file.
