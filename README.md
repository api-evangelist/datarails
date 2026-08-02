# Datarails

Datarails is an Excel-native financial planning and analysis (FP&A) platform for the office of the CFO,
marketed as FinanceOS. It consolidates data from ERP, CRM, HRIS, billing and operational systems into a
single finance data model, then lets finance teams budget, forecast, consolidate, report and build
dashboards while continuing to work in Excel through the Datarails Flex add-in.

- Website: https://www.datarails.com/
- Help center: https://support.datarails.com/hc/en-us
- GitHub: https://github.com/Datarails
- Status: https://datarails.statuspage.io/
- Trust center: https://trust.datarails.com/

## API surface

Datarails is an agent-first API provider. Its primary programmatic interface is not REST.

| Surface | Endpoint | Auth |
|---|---|---|
| Datarails FinanceOS MCP Server | `https://mcp.datarails.com/mcp` | OAuth 2.1 + PKCE (S256), dynamic client registration |
| Data Gateway Service (file upload) | `POST https://app.datarails.com/api/v1/fileboxes/upload_file` | HTTP Basic (base64 user:password) |

The MCP server exposes roughly two dozen **read-only** tools across three data-access layers — business
metrics, aliased tables, and raw by-id — for discovering data models, profiling fields, running filtered
queries and asynchronous aggregations, and extracting validated financials into Excel. Datarails
documents that the MCP connection cannot create, update or delete records.

A real OpenAPI 3.1.0 document is served at `https://mcp.datarails.com/openapi.json`, but it describes only
the MCP server's own health, readiness and OAuth endpoints — the data capability is not in it. See
`mcp/datarails-tool-crosswalk.yml` for that divergence.

## Notable

- **Datarails publishes its own Agent Skills.** The first-party Claude Code plugin
  (`Datarails/dr-claude-code-plugins-re`, MIT, v3.0.6) ships 19 `SKILL.md` files and 8 agents. They are
  copied verbatim into `skills/` — nothing in that directory was authored on Datarails' behalf.
- **Full OAuth discovery.** RFC 8414, RFC 9728 (including the resource-scoped path advertised in the 401
  challenge), OIDC discovery, and RFC 7591 dynamic client registration are all live.
- **One coarse scope.** The authorization server advertises exactly one scope, `datarails`. Least
  privilege lives in Datarails' in-app permission model, not in OAuth.
- **No A2A agent card**, no security.txt, no api-catalog, no AsyncAPI (there is no event surface), no
  RFC 9457 errors, no client SDK in any language, and no published vulnerability disclosure program.

Artifacts in this repo were produced by the API Evangelist enrichment pipeline. Provenance is recorded in
each file's `method:` and `source:` frontmatter.
