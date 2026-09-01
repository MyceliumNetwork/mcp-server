# Mycelium Emissions Database MCP Server

A hosted [Model Context Protocol](https://modelcontextprotocol.io) server that gives AI assistants direct access to the Mycelium open corporate carbon emissions database: company search, Mycelium Scores, and full per-scope greenhouse gas figures.

- **Endpoint (streamable HTTP):** `https://mycelium.global/mcp/server`
- **Auth:** none - anonymous access with a per-IP rate limit, so a pasted URL works in any MCP client
- **Info page:** https://mycelium.global/mcp
- **REST API + OpenAPI spec:** https://github.com/MyceliumNetwork/api
- **Website:** https://mycelium.global

## Setup

### Claude (claude.ai, Claude Desktop)

Settings → Connectors → Add custom connector → paste `https://mycelium.global/mcp/server`.

### Claude Code

```bash
claude mcp add --transport http mycelium https://mycelium.global/mcp/server
```

### Cursor / generic MCP client

```json
{
  "mcpServers": {
    "mycelium": {
      "url": "https://mycelium.global/mcp/server"
    }
  }
}
```

## Tools

### `search-entities`

Search the emissions database for companies by name. Returns matching companies with their Mycelium Score and the slug handle used by the other tools.

| Input | Type | Description |
| --- | --- | --- |
| `query` | string (required) | Company name or name fragment |
| `country` | string | Optional ISO 3166-1 alpha-3 filter, e.g. `GBR`, `DEU` |
| `limit` | integer | Max matches to return, 1-25, default 10 |

### `get-entity-profile`

A company's public Mycelium profile: identity and registration, Mycelium Score (0 to 10) with industry rank, Transparency Score, and a per-year summary of total emissions in tonnes CO2e split into reported and estimated.

| Input | Type | Description |
| --- | --- | --- |
| `entity` | string (required) | Slug from `search-entities`, or Mycelium public id GUID |

### `get-emission-figures`

Full greenhouse gas figures for one reporting year in the [OpenCCF](https://mycelium.global) data model: per-scope lines (Scope 1, 2 and 3 with categories) in kgCO2e, each tagged as reported by the company or estimated by Mycelium.

| Input | Type | Description |
| --- | --- | --- |
| `entity` | string (required) | Slug from `search-entities`, or Mycelium public id GUID |
| `year` | integer | Reporting year, e.g. 2023. Defaults to the latest on file |

## Provenance

Every emissions line is tagged as **reported** by the company or **estimated** by Mycelium. Clients should preserve that distinction and never present an estimate as a company's own disclosure.

## About the data

Mycelium is an open corporate carbon disclosure platform covering hundreds of thousands of entities, built from company reporting, registry data (GLEIF, Companies House and others) and modelled estimates. The same data is available over REST: see the [Mycelium Network API](https://github.com/MyceliumNetwork/api).

## License

Documentation in this repository is MIT licensed (see [LICENSE](LICENSE)). Use of the hosted service is governed by the terms at https://mycelium.global.
