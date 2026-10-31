<p align="center"><img src="toll-icon.png" width="96" alt="Toll"></p>

# Toll MCP server

The check AI agents call before a trade. Coin and product checks on **Robinhood Chain, Solana, BSC and Arc**, read from the chain itself: **is there a real product behind a coin** (the LARP check), who launched it, who sniped it in the first seconds, which wallets moved together, the dev's record, and whether the liquidity can be pulled. Plus **agent checks**: does an ERC-8004 agent's card load, do its MCP and x402 endpoints answer, and is its x402 payTo its registered wallet.

Toll is itself ERC-8004 agent #20030 on Robinhood Chain.

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
| `check_coin` | One token on Robinhood Chain, Solana, BSC or Arc: where and when it launched, the dev and its record, snipers in the first 3 seconds, bundles, wallets that moved together, who holds the liquidity, and a plain tally of the findings (against / careful / clear), split into what was fixed at launch and the state now. Leave out `chain` and Toll finds it. |
| `larp_check` | Is there a real product behind a coin on Robinhood Chain or Solana? Toll reads its site, posts and code, lists each thing it claims, and checks every claim against the chain and its own working parts (app, API, MCP server, x402, extension, Telegram bot, public code). The label is product found / partly found / not found / not read: a fact about what we could check, never a verdict. |
| `dev_record` | Every coin one wallet launched (Robinhood Chain, Arc or BSC): how many, how many graduated, whether the dev sold. |
| `wallet_privacy` | What anyone can learn about a wallet from Robinhood Chain: a privacy grade, each leak (funding, linked wallets, holdings, active hours) and tips. It measures what is visible, not whether the wallet is good or bad. |
| `fresh_launches` | The newest coins on Robinhood Chain from Toll's live engine, with short notes on each. |
| `chain_pulse` | How busy Robinhood Chain is right now: launches, graduations, new pools, trades and volume. |
| `launch_record` | The call Toll froze 3 minutes after a Robinhood Chain coin launched, and how it ended 24 hours later. |
| `check_agent` | One ERC-8004 agent on Robinhood Chain by its id: does its card load, do its MCP server and x402 endpoint answer, is its x402 payTo the wallet it registered, has its wallet been used. Facts from what the card and endpoints answered, never a verdict. |

Every answer ends with a link to the full report on t0ll.xyz.

## Example

> **You:** is 0x72bfcf23b089403a1056fd9719c577966b941f57 bundled?
>
> **Agent (calls `check_coin`):** $SWARM, a Robinhood Chain token launched on Pons 9 days ago, graduated. 1 against, 3 careful, 3 clear. One wallet bought 1.7% of supply in the first 3 seconds; 5 wallets bought in the same tenth of a second 54 s after launch (0.44%); no declared bundle. The dev holds 0% now…

## Sell over x402: the Toll facilitator

Toll runs an x402 facilitator for USDG on Robinhood Chain at **<https://x402.t0ll.xyz>**: keyless, free, no account.
Before it settles, it checks the payTo against the seller's ERC-8004 identity
([ERC-8478](https://github.com/ethereum/ERCs/pull/2132)), and every settlement comes back with a Toll-signed EIP-712
receipt (who paid whom, how much, which transaction, what the payee check said).

Point your x402 server at the facilitator URL and name USDG and its signing domain explicitly (most x402 libraries have
no default for chain 4663):

```json
{
  "scheme": "exact",
  "network": "eip155:4663",
  "asset": "0x5fc5360D0400a0Fd4f2af552ADD042D716F1d168",
  "amount": "50000",
  "payTo": "0xYourAgentWallet",
  "maxTimeoutSeconds": 300,
  "extra": {
    "name": "Global Dollar",
    "version": "1",
    "erc8004": { "registry": "eip155:4663:0x8004A169FB4a3325136EB29fA0ceB6D2e539a432", "agentId": "<your agent id>" }
  }
}
```

Endpoints: `/verify`, `/settle`, `/supported`, `/stats`, `/receipt/<id or tx>`. The
[x402 Register](https://x402.t0ll.xyz/#register) lists every agent on Robinhood Chain that sells over x402 and whether
its payTo is bound to its identity.

## Limits

- Free: 10 fresh coin checks a day per caller. A coin someone already checked recently is free to read again. Resets at 00:00 UTC.
- Need more? Agents can pay per check with x402 at `https://t0ll.xyz/api/x402/check/<address>`.
- The LARP check shares the same free coins; past them, `https://t0ll.xyz/api/x402/larp/<address>?chain=rh|sol` (0.05 USDG, charged only for a whole answer).

## What it is not

Toll states facts read from the chain and never says a coin is safe. What the facts mean is your decision. Information only, not financial advice.

## Links

- Site and full checks: https://t0ll.xyz
- MCP page: https://t0ll.xyz/agents/mcp
- Agent census (every ERC-8004 agent on Robinhood Chain): https://t0ll.xyz/agents/census
- Telegram bot: https://t.me/t0ll_bot
- On X: tag @tollchk under a post with a coin's address
