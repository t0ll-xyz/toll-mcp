<p align="center"><img src="toll-icon.png" width="96" alt="Toll"></p>

# Toll MCP server

Coin and product checks for AI agents on **Robinhood Chain, Solana and BSC**, read from the chain itself: **is there a real product behind a coin** (the LARP check), who launched it, who sniped it in the first seconds, which wallets moved together, the dev's record, and whether the liquidity can be pulled.

This is a **remote MCP server**. There is nothing to install or run: point your client at

```
https://t0ll.xyz/mcp
```

(Streamable HTTP, no API key, read-only.)

This repository only holds the listing files (`server.json`) and this guide. The server itself runs at t0ll.xyz.

## Connect

**Claude Code**

```
claude mcp add --transport http toll https://t0ll.xyz/mcp
```

**Claude Desktop, Cursor, Windsurf, VS Code** (any client that takes an MCP JSON config)

```json
{
  "mcpServers": {
    "toll": {
      "type": "http",
      "url": "https://t0ll.xyz/mcp"
    }
  }
}
```

Clients that only speak stdio can use a bridge such as `npx mcp-remote https://t0ll.xyz/mcp`.

## Tools

| Tool | What it answers |
|---|---|
| `check_coin` | One token on Robinhood Chain, BSC or Solana: where and when it launched, the dev and its record, snipers in the first 3 seconds, bundles, wallets that moved together, who holds the liquidity, and a plain tally of the findings (against / careful / clear). Leave out `chain` and Toll finds it. |
| `larp_check` | Is there a real product behind a coin on Robinhood Chain or Solana? Toll reads its site, posts and code, lists each thing it claims, and checks every claim against the chain and its own working parts (app, API, MCP server, x402, extension, Telegram bot, public code). The label is product found / partly found / not found / not read: a fact about what we could check, never a verdict. |
| `dev_record` | Every coin one wallet launched: how many, how many graduated, whether the dev sold. |
| `wallet_privacy` | What anyone can learn about a wallet from Robinhood Chain: a privacy grade, each leak (funding, linked wallets, holdings, active hours) and tips. It measures what is visible, not whether the wallet is good or bad. |
| `fresh_launches` | The newest coins on Robinhood Chain from Toll's live engine, with short notes on each. |
| `chain_pulse` | How busy Robinhood Chain is right now: launches, graduations, new pools, trades and volume. |
| `launch_record` | The call Toll froze 3 minutes after a Robinhood Chain coin launched, and how it ended 24 hours later. |

Every answer ends with a link to the full report on t0ll.xyz.

## Example

> **You:** is 0x72bfcf23b089403a1056fd9719c577966b941f57 bundled?
>
> **Agent (calls `check_coin`):** $SWARM, a Robinhood Chain token launched on Pons 9 days ago, graduated. 1 against, 3 careful, 3 clear. One wallet bought 1.7% of supply in the first 3 seconds; 5 wallets bought in the same tenth of a second 54 s after launch (0.44%); no declared bundle. The dev holds 0% now…

## Limits

- Free: 10 fresh coin checks a day per caller. A coin someone already checked recently is free to read again. Resets at 00:00 UTC.
- Need more? Agents can pay per check with x402 at `https://t0ll.xyz/api/x402/check/<address>`.
- The LARP check shares the same free coins; past them, `https://t0ll.xyz/api/x402/larp/<address>?chain=rh|sol` (0.05 USDG, charged only for a whole answer).

## What it is not

Toll states facts read from the chain and never says a coin is safe. What the facts mean is your decision. Information only, not financial advice.

## Links

- Site and full checks: https://t0ll.xyz
- MCP page: https://t0ll.xyz/agents/mcp
- Telegram bot: https://t.me/t0ll_bot
- On X: tag @tollchk under a post with a coin's address
