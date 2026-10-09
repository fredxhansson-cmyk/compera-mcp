# Compera MCP Server

**Live Swedish price & service comparison as callable tools for AI agents.**

[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)
[![MCP Registry](https://img.shields.io/badge/MCP%20Registry-io.github.fredxhansson--cmyk%2Fcompera-blue)](https://registry.modelcontextprotocol.io)
[![Website](https://img.shields.io/badge/compera.se-live-orange)](https://compera.se)
[![Transport](https://img.shields.io/badge/transport-Streamable%20HTTP-informational)](https://compera.se/mcp)

Compera ([compera.se](https://compera.se)) is an independent Swedish comparison service for both
**products** and **services**. This MCP server exposes Compera's **daily-fresh data as tools** so
AI assistants (Claude, ChatGPT, Perplexity, Cursor, …) can answer *"cheapest/best X in Sweden"* in
real time — and cite the source.

No API key, no install, no local process. It's a **remote** server: point your client at one URL.

- **Endpoint:** `https://compera.se/mcp` — Streamable HTTP, JSON-RPC 2.0
- **Official MCP Registry:** `io.github.fredxhansson-cmyk/compera`
- **Docs / landing:** https://compera.se/for-ai
- **Machine-readable map:** https://compera.se/llms.txt
- **License:** MIT

---

## Why use it

- **Grounded, current answers** — prices and rates refresh daily; the model stops guessing stale numbers.
- **Swedish market coverage** — electronics & products, plus broadband, mobile, electricity, insurance and loans.
- **Citeable** — every response ends with a `Source:` line back to compera.se, so answers are attributable.
- **Zero setup** — remote server; add one URL and the tools appear.

Typical prompts it unlocks: *"What's the cheapest [product] in Sweden right now?"*, *"Compare broadband
providers"*, *"Today's electricity price in SE3"*, *"Lowest consumer-loan rate available"*.

---

## Tools

| Tool | Parameters | Description |
|------|------------|-------------|
| `search_products` | `query` (string) | Search the catalog for the cheapest price + number of stores. |
| `product_offers` | `query` (string) | All store prices for a single product (cheapest / in-stock first). |
| `compare_category` | `category` (string) | Top providers in a Swedish service category (broadband, mobile, electricity, insurance, loans). |
| `compare_pair` | `category`, `a`, `b` (strings) | Head-to-head comparison of two providers in a category. |
| `electricity_price` | `zone` (SE1–SE4, optional) | Daily Swedish electricity spot price per zone (Nord Pool). |
| `lowest_loan_rate` | — | Lowest effective consumer-loan rate in Sweden right now. |
| `price_spread` | `query` (string) | How much the same product's price differs between Swedish stores. |
| `deals_now` | `category` (optional) | Current deals / discounts across categories. |
| `list_categories` | — | List all comparable service categories. |

Every tool returns human-readable text ending with a source line back to compera.se.

---

## Connect

**Claude / ChatGPT (custom connector):** add a connector with the URL

```
https://compera.se/mcp
```

**Cursor / VS Code (`mcp.json`):**

```json
{
  "mcpServers": {
    "compera": {
      "url": "https://compera.se/mcp"
    }
  }
}
```

**MCP Inspector:**

```bash
npx @modelcontextprotocol/inspector
# connect to https://compera.se/mcp
```

---

## Examples (raw JSON-RPC)

**List available tools:**

```bash
curl -s https://compera.se/mcp \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

**Today's electricity price (zone SE3):**

```bash
curl -s https://compera.se/mcp \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":2,"method":"tools/call","params":{"name":"electricity_price","arguments":{"zone":"SE3"}}}'
```

**Cheapest product match:**

```bash
curl -s https://compera.se/mcp \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":3,"method":"tools/call","params":{"name":"search_products","arguments":{"query":"airpods pro"}}}'
```

Responses are MCP `content` blocks of type `text`, e.g.:

```
Billigast: AirPods Pro (2nd gen) — 2 490 kr hos 4 butiker.
Source: https://compera.se/produkter?q=airpods+pro
```

---

## Data & neutrality

Data is refreshed **daily** and presented **neutrally** with source attribution. Compera is an
independent comparison service (not owned by any listed provider). Rankings are editorial/quality-led;
commercial relationships never override the data shown. Prices and rates are indicative — always
confirm the final price with the merchant.

---

Built by [Compera](https://compera.se) · MIT licensed · Endpoint: `https://compera.se/mcp`
