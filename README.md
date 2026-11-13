# Private Company Funding History MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-private-company-funding-history/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-private-company-funding-history) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-private-company-funding-history%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-private-company-funding-history&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-private-company-funding-history)](https://www.npmjs.com/package/@mambalabsdev/mcp-private-company-funding-history) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-private-company-funding-history)](https://www.npmjs.com/package/@mambalabsdev/mcp-private-company-funding-history) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-private-company-funding-history)](https://github.com/mambalabsdev/mcp-private-company-funding-history/blob/main/LICENSE)

An MCP server for the Mamba Labs Private Company Funding History actor on Apify. Get the SEC funding filings of private US companies: one company row plus one row per round.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Give company legal names or SEC CIKs. For each company the actor finds its SEC entity, lists every Form D (private placement) and Form C (crowdfunding) offering it filed, and reads the latest document of each. You get one company row with totals and dates, and one row per offering with amounts offered and sold, the first sale date, security types, investor count, executives, the crowdfunding platform for Form C, and a link to the filing.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-private-company-funding-history": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-private-company-funding-history"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `get_company_funding_history` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "What has Ramp Business Corp raised according to its SEC filings?"
- "List every Form D funding round Notion Labs filed."
- "Which crowdfunding platform did FERRA Kitchen use, and what was its target?"

## Tool

`get_company_funding_history`. One tool. Example arguments:

```json
{
  "companies": [
    "Ramp Business Corp",
    "Notion Labs, Inc.",
    "1717731"
  ]
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `companies` | array | no | One company per item: its legal name as filed with the SEC (Ramp Business Corp, Commure, Inc.) or its SEC CIK (1803782). Legal suffixes such as Inc or LLC can be left off. As text, put one company per line; commas inside a name are kept. |
| `match` | string | no | exact (default): the EDGAR name must equal your name once legal suffixes are removed. starts_with: if there is no exact match, take the most viewed EDGAR name that starts with your name and is not a fund, SPV, or series. Check entity_name on the row. |
| `forms` | string | no | both (default): Form D private placements and Form C crowdfunding offerings. d: Form D only. c: Form C only. |
| `filed_after` | string | no | Optional. Keep offerings with a filing on or after this date, as YYYY-MM-DD or a year such as 2022. |
| `max_rounds_per_company` | number | no | Newest offerings first, 1 to 200. Default 50. The company row counts every offering on file. |
| `max_charge_usd` | string | no | Optional. The run stops before a charge that would pass this amount. Rows already charged are kept. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `company-history`, one company with funding filings found, company row plus its first 25 round rows | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `extra-rounds`, each further block of up to 25 round rows for one company | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `lookup-no-funding`, one name or CIK looked up with no SEC match or no funding filings | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `apify-actor-start`, per run start | $0.0012 | $0.0012 | $0.0012 | $0.0012 |

A refused input and a refused SEC read are free. A name with no match or no funding filings is one 0.001 USD lookup. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/private-company-funding-history](https://apify.com/mambalabs/private-company-funding-history)

## License

MIT. See [LICENSE](LICENSE).
