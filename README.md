# Datarails

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

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
