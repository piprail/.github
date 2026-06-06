<div align="center">

<a href="https://piprail.com">
  <img src="https://piprail.com/og.png" alt="PipRail — the payment layer for the agent economy" width="1000">
</a>

<h1>PipRail</h1>

**The payment layer for the agent economy.**

A backendless TypeScript **SDK** for [x402](https://x402.org) payments — plus an **MCP server** that hands any AI agent a budget-bound wallet. Let any HTTP endpoint charge for itself, and any agent pay for itself, in a couple of lines.

<p>
  <a href="https://www.npmjs.com/package/@piprail/sdk"><img src="https://img.shields.io/npm/v/@piprail/sdk?style=flat-square&color=cb3837&label=%40piprail%2Fsdk" alt="@piprail/sdk npm version"></a>
  <a href="https://www.npmjs.com/package/@piprail/mcp"><img src="https://img.shields.io/npm/v/@piprail/mcp?style=flat-square&color=2ee6a6&label=%40piprail%2Fmcp" alt="@piprail/mcp npm version"></a>
  <a href="https://www.npmjs.com/package/@piprail/sdk"><img src="https://img.shields.io/npm/types/@piprail/sdk?style=flat-square&color=3178c6" alt="TypeScript types"></a>
  <a href="https://github.com/piprail/piprail/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-2ee6a6?style=flat-square" alt="MIT License"></a>
  <a href="https://x402.org"><img src="https://img.shields.io/badge/x402-v2-6e56cf?style=flat-square" alt="x402 v2"></a>
  <a href="https://registry.modelcontextprotocol.io"><img src="https://img.shields.io/badge/MCP_registry-io.github.piprail%2Fmcp-2ee6a6?style=flat-square" alt="Listed in the MCP registry"></a>
</p>

<strong><a href="https://www.npmjs.com/package/@piprail/sdk">Get started →</a></strong> &nbsp;·&nbsp; <a href="https://piprail.com">Website</a> &nbsp;·&nbsp; <a href="https://piprail.com/demo">Live demo</a> &nbsp;·&nbsp; <a href="https://piprail.com/mcp">MCP server</a> &nbsp;·&nbsp; <a href="https://piprail.com/discovery">Discovery</a> &nbsp;·&nbsp; <a href="https://github.com/piprail/piprail/blob/main/sdk/README.md">Docs</a>

</div>

---

## What is PipRail?

PipRail implements the open **402 Payment Required** standard for HTTP and agent payments. There is no backend, no database, no account, no dashboard, and no protocol fee. Payments settle straight into your own wallet, verified locally against your own RPC.

It's a library you `npm install` — not a hosted service you sign up for.

- 🔌 **One parameter picks everything** — name a `chain`, add a wallet, get paid.
- 🤖 **Give your agent a wallet** — [`@piprail/mcp`](https://piprail.com/mcp) lets Claude, Cursor & any MCP client pay x402 URLs on their own, capped by a spend policy the model can't exceed.
- 🧭 **Discoverable** — emit a machine-readable manifest, [register](https://piprail.com/discovery) on the open x402 indexes (402 Index, CDP Bazaar), and let agents find and pay your endpoint. PipRail hosts no registry of its own.
- 🏷️ **No facilitator, no fee** — funds go wallet-to-wallet; you keep 100%.
- 🔒 **Verified locally** — on-chain checks run against your own RPC; nothing leaves your process.
- 🔋 **Affordability-aware** — `planPayment()` checks balance, gas, and recipient-readiness before an agent pays.
- 📦 **Pure TypeScript** — runs headless or in the browser; `viem` peer dep, non-EVM libs lazy-loaded.
- ⚖️ **MIT licensed** — use it, fork it, ship it.

---

## How it works

Charge for an endpoint on the server, pay for one from the agent — the same `chain` parameter drives both sides.

```ts
import { requirePayment } from '@piprail/sdk'

// Server: make an endpoint charge for itself
app.get('/report',
  requirePayment({ chain: 'base', token: 'USDC', amount: '0.05', payTo: '0xYourWallet…' }),
  (_req, res) => res.json({ report: 'TOP SECRET' }),
)
```

```ts
import { PipRailClient } from '@piprail/sdk'

// Agent: pay the 402 automatically
const client = new PipRailClient({ chain: 'base', wallet: { privateKey: process.env.AGENT_KEY } })
const res = await client.get('https://api.example.com/report')
```

Or skip the code entirely — drop the **MCP server** into any agent and it pays x402 URLs itself, budget-bound. Add one block to your MCP client config:

```jsonc
// Claude Desktop · Cursor · Claude Code · Windsurf · VS Code · Cline
{ "piprail": {
    "command": "npx", "args": ["-y", "@piprail/mcp"],
    "env": { "PIPRAIL_PRIVATE_KEY": "0x…", "PIPRAIL_CHAIN": "base", "PIPRAIL_MAX_AMOUNT": "0.10" } } }
```

[See the full examples →](https://github.com/piprail/piprail/tree/main/examples) &nbsp;·&nbsp; [MCP setup guide →](https://piprail.com/mcp)

---

## Supported chains

One protocol, every major chain — **28 chains across 10 driver families** (19 EVM mainnets plus Solana, TON, Tron, NEAR, Sui, Aptos, Algorand, Stellar, and XRPL). USDC nearly everywhere, USDT on most, native gas coins too.
<!-- Update count when adding a chain/family. Full, always-current list: sdk/README.md -->

<div align="center">
<table>
<tr>
  <td align="center"><img src="https://piprail.com/chains/ethereum.svg" alt="Ethereum" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/base.svg" alt="Base" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/arbitrum.svg" alt="Arbitrum" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/optimism.svg" alt="Optimism" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/polygon.svg" alt="Polygon" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/bnb.svg" alt="BNB Chain" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/avalanche.svg" alt="Avalanche" width="44" height="44"></td>
</tr>
<tr>
  <td align="center"><img src="https://piprail.com/chains/mantle.svg" alt="Mantle" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/sonic.svg" alt="Sonic" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/linea.svg" alt="Linea" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/scroll.svg" alt="Scroll" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/celo.svg" alt="Celo" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/zksync.svg" alt="zkSync" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/unichain.svg" alt="Unichain" width="44" height="44"></td>
</tr>
<tr>
  <td align="center"><img src="https://piprail.com/chains/worldchain.svg" alt="World Chain" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/sei.svg" alt="Sei" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/injective.svg" alt="Injective" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/hyperevm.svg" alt="HyperEVM" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/monad.svg" alt="Monad" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/solana.svg" alt="Solana" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/ton.svg" alt="TON" width="44" height="44"></td>
</tr>
<tr>
  <td align="center"><img src="https://piprail.com/chains/tron.svg" alt="Tron" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/near.svg" alt="NEAR" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/sui.svg" alt="Sui" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/aptos.svg" alt="Aptos" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/algorand.svg" alt="Algorand" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/stellar.svg" alt="Stellar" width="44" height="44"></td>
  <td align="center"><img src="https://piprail.com/chains/xrpl.svg" alt="XRPL" width="44" height="44"></td>
</tr>
</table>

<sub><a href="https://github.com/piprail/piprail/blob/main/sdk/README.md">See every chain, token, and preset →</a></sub>

</div>

Any other EVM chain works by passing a viem `Chain` or `{ id, rpcUrl }` — the built-in presets are a convenience, not an allowlist.

---

## Packages

| Package | What it is |
| --- | --- |
| [**@piprail/sdk**](https://www.npmjs.com/package/@piprail/sdk) | The core SDK — take or make x402 payments across 28 chains, in a couple of lines. |
| [**@piprail/mcp**](https://www.npmjs.com/package/@piprail/mcp) | An [MCP](https://modelcontextprotocol.io) server — give any AI agent a budget-bound wallet to pay, discover & register x402 URLs autonomously. Listed in the [MCP registry](https://registry.modelcontextprotocol.io) as `io.github.piprail/mcp`. `npx -y @piprail/mcp` |

## Repositories

| Repository | Description |
| --- | --- |
| [**piprail/piprail**](https://github.com/piprail/piprail) | The `@piprail/sdk` + `@piprail/mcp` source, the static landing site, and runnable examples. |

---

## Contributing

PipRail is open source under the MIT license. Issues and pull requests are welcome on the [main repository](https://github.com/piprail/piprail). The SDK's tests are the canonical contract — behaviour changes start there.

<div align="center">

<strong><a href="https://www.npmjs.com/package/@piprail/sdk">Get started</a></strong> &nbsp;·&nbsp; <a href="https://piprail.com/demo">Try the live demo</a> &nbsp;·&nbsp; <a href="https://github.com/piprail/piprail">Browse the code</a>

<sub>Built on the open <a href="https://x402.org">x402</a> standard · MIT licensed · No backend, no fee, ever.</sub>

</div>
