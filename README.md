# Compera MCP Server

**Live Swedish price & service comparison as callable tools for AI agents.**

Compera ([compera.se](https://compera.se)) is an independent Swedish comparison service for
both products and services. This MCP server exposes Compera's **daily-fresh data as tools** so
AI assistants (ChatGPT, Claude, Perplexity, …) can answer "cheapest/best X in Sweden" in real
time and cite the source.

- **Endpoint:** `https://compera.se/mcp` (Streamable HTTP, JSON-RPC 2.0)
- **Official MCP Registry:** `io.github.fredxhansson-cmyk/compera`
- **Docs / landing:** https://compera.se/for-ai
- **Machine-readable map:** https://compera.se/llms.txt
- **License:** MIT

## Tools

| Tool | Description |
|------|-------------|
| `search_products` | Search the product catalog for the cheapest price + number of stores. |
| `product_offers` | All store prices for a single product (cheapest / in-stock first). |
| `compare_category` | Top providers in a Swedish service category (broadband, mobile, electricity, insurance, loans). |
| `compare_pair` | Head-to-head comparison of two providers in a category. |
| `electricity_price` | Daily Swedish electricity spot price per zone SE1–SE4 (Nord Pool). |
| `lowest_loan_rate` | Lowest effective consumer loan rate in Sweden right now. |
| `price_spread` | How much the same product's price differs between Swedish stores. |
| `deals_now` | Current deals/discounts across categories. |
| `list_categories` | List all comparable service categories. |

Every tool returns text with a source line back to compera.se.

## Connect

Add a custom connector in Claude or ChatGPT with the URL:

```
https://compera.se/mcp
```

Or test with the MCP Inspector:

```bash
npx @modelcontextprotocol/inspector
# connect to https://compera.se/mcp
```

## Example (raw JSON-RPC)

```bash
curl -s https://compera.se/mcp \
  -H 'content-type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"electricity_price"}}'
```

---

Built by [Compera](https://compera.se). Data is updated daily and presented neutrally with source attribution.
