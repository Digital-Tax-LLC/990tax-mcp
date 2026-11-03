# Installing the 990.tax MCP servers

Both are remote servers over Streamable HTTP; nothing to install or build.

1. Add the free lookup server: URL `https://990.tax/mcp`, transport HTTP (streamable), no authentication, no API key.
2. Optional, to prepare Form 990-N filings: URL `https://app.990.tax/mcp`, transport HTTP. It uses OAuth 2.1: the client discovers it from `https://app.990.tax/.well-known/oauth-protected-resource` after a 401, registers itself (dynamic client registration) and opens 990.tax for the user to sign in and allow access.
3. Check: call `search_nonprofits` with `{"query": "red cross"}`. Answers include a "Source: 990.tax — https://990.tax/..." line.
