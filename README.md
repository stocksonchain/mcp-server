# Stocks On Chain — MCP server

A read-only MCP server for **tokenized stocks**: which listed companies exist as
tokens, which issuer and chain each token lives on, its contract address,
supply, price and corporate actions.

Hosted and remote. Nothing to install, no account, no API key, no wallet.

```
https://stocksonchain.io/mcp
```

Published by [Gwei, Inc.](https://stocksonchain.io/about) · Data under
[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) ·
[stocksonchain.io](https://stocksonchain.io)

## Connect

The transport is `streamable-http`. Point any MCP client at the URL above.

```json
{
  "mcpServers": {
    "stocks-on-chain": {
      "type": "streamable-http",
      "url": "https://stocksonchain.io/mcp"
    }
  }
}
```

It is also on the official [MCP Registry](https://registry.modelcontextprotocol.io)
as `io.stocksonchain/stocks-on-chain`, so a client that reads the registry can
find it without this file:

```
curl "https://registry.modelcontextprotocol.io/v0/servers?search=stocksonchain"
```

`mcp-server.json` in this repository is that registry entry.

## Other clients

This repository is also a Gemini CLI extension and a Cursor plugin. Both point at the same hosted server; nothing runs locally.

```
gemini extensions install https://github.com/stocksonchain/mcp-server
```

Cursor reads `.cursor-plugin/plugin.json` and `mcp.json` at the root of this repository.

## Tools

| Tool | What it answers |
| --- | --- |
| `search_stocks` | Find stocks by ticker or company name. A real ticker that is **not** tokenized reports that, which is an answer rather than an empty result. |
| `get_stock` | Everything held about one stock: each instrument (one per issuer, with the mechanics that govern corporate actions), each listing under it (chain, contract address, supply, price), and its recorded events. |
| `list_issuers` | Every issuer, its legal wrapper, the chains it issues on, and a link to where it publishes its own rule on who may buy. |
| `list_chains` | Every chain carrying tokenized stocks, with its explorer and the issuer-by-chain matrix. |
| `list_events` | The corporate-action tape: splits, distributions and multiplier changes, each with a transaction hash. |
| `get_contract` | What is held about one address, plus what the code there actually is: runtime bytecode hash, the implementation behind a proxy, and how many tracked addresses run byte-identical code. |
| `get_manifest` | The service manifest: endpoints, licence and terms. |

## Three things to know before you quote it

**Every figure carries the time it was read.** Quote the timestamp with the
number. The chain is read about once a day.

**Nothing is ranked, rated or recommended.** Every ordered list states its own
sort key, and none of them is a ranking. There is no "best", no safety score and
no risk grade, and no field that could carry one.

**Eligibility is a link, never a quote.** `list_issuers` gives you the URL where
each issuer publishes its own rule on who may buy. It does not give you the rule.
An issuer can change who it deals with without editing the page we cite, so a
stored quote goes stale invisibly — send the reader to the source instead of
repeating a rule.

`get_contract` follows the same principle: it never says an address is safe,
genuine or correct, and there is no field that could. Check an address against
the issuer before acting on it.

## Free HTTP endpoints

The same data, keyless, if you would rather not speak MCP:

| | |
| --- | --- |
| Every token | `https://stocksonchain.io/api/tokens.json` |
| One stock | `https://stocksonchain.io/api/tokens/{slug}.json` |
| Corporate actions | `https://stocksonchain.io/api/events.json` |
| Daily totals | `https://stocksonchain.io/api/history/totals.json` |
| Index for models | `https://stocksonchain.io/llms.txt` |

Every page also has a Markdown twin: append `.md`, or send
`Accept: text/markdown`.

Method, and what is **not** read, is at
[stocksonchain.io/methodology](https://stocksonchain.io/methodology).

## Not investment advice

This is a reference publisher. It sells nothing, names nowhere to buy, and never
asks anyone to sign in or use a wallet. A stock token is not a share; what it
legally is depends on the wrapper its issuer chose, and each issuer describes its
own.

## About this repository

It holds the registry manifest and this README. **The server is not built here** —
it runs as part of the site, whose source is private.

`mcp-server.json` is generated (`npm run mcp:manifest` in the site repo) from the
same tool definitions the server serves, so it cannot drift from the running
server. Do not hand-edit it here; regenerate it there and copy it across.
