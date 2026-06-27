<div align="center">

<img src="https://pbs.twimg.com/profile_images/2067141659448733696/FFVwzHuq.jpg" alt="SwanFi" width="80" style="border-radius: 20px;" />

# SwanFi

**Real-time BSC token discovery & trading infrastructure**

[![Platform](https://img.shields.io/badge/Platform-Live-22c55e?style=flat-square&labelColor=0a0a0f)](https://swanfi.pro)
[![API](https://img.shields.io/badge/API-v1.0-3b82f6?style=flat-square&labelColor=0a0a0f)](https://api.swanfi.pro)
[![X](https://img.shields.io/badge/X-@swanfidotpro-000000?style=flat-square&logo=x&logoColor=white&labelColor=0a0a0f)](https://x.com/swanfidotpro)
[![Telegram](https://img.shields.io/badge/Telegram-Community-26A5E4?style=flat-square&logo=telegram&logoColor=white&labelColor=0a0a0f)](https://t.me/swanficommunity)

> Index every token launch. Track every transaction. Stream it all — in real time.

</div>

---

## Overview

SwanFi is a high-performance on-chain data platform built for the **BNB Smart Chain** ecosystem. We provide developers and traders with the infrastructure to discover, analyze, and act on BSC token data the moment it hits the chain.

From launch on **Flap.sh** and **Four.meme**, through bonding curve progression, to migration on **PancakeSwap** — we surface everything through a clean developer API and live WebSocket feeds.

```
New Token Launch → Indexing → Analytics → REST API / WebSocket → Your App
```

No polling. No stale data. No abstraction layers between you and the chain.

---

## Platform

| Service | Endpoint | Description |
|---|---|---|
| **Trading App** | [swanfi.pro](https://swanfi.pro) | Real-time token discovery & trading interface |
| **REST API** | [api.swanfi.pro](https://api.swanfi.pro) | Versioned data endpoints for developers |
| **WebSocket** | `wss://ws.swanfi.pro` | Live price, trade & token event feeds |

---

## Features

- **🔍 Token Discovery** — Real-time detection of new launches on Flap.sh and Four.meme with live bonding curve progress and instant migration detection.
- **📊 On-Chain Analytics** — Full transaction history, holder tracking, PnL analysis, and top trader leaderboards.
- **⚡ Real-Time Feeds** — Sub-second WebSocket delivery with per-token subscription channels, OHLCV candles, and whale alerts.
- **🛠 Developer Infrastructure** — Versioned REST API with API key auth, rate limiting, usage analytics, and consistent response schemas.

---

## Tech Stack

![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Fastify](https://img.shields.io/badge/Fastify-000000?style=flat-square&logo=fastify&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black)
![BNB Chain](https://img.shields.io/badge/BNB_Chain-F0B90B?style=flat-square&logo=binance&logoColor=black)
![WebSocket](https://img.shields.io/badge/WebSocket-010101?style=flat-square&logo=socket.io&logoColor=white)
![ethers.js](https://img.shields.io/badge/ethers.js-2535A0?style=flat-square&logo=ethereum&logoColor=white)

---

## Quick Start

### REST API

Base URL: `https://api.swanfi.pro/v1`

All requests require an API key via the `x-api-key` header.

```bash
curl -H "x-api-key: swfi_your_key" \
  "https://api.swanfi.pro/v1/tokens?source=four_meme&sort=newest&limit=10"
```

### WebSocket

```javascript
const ws = new WebSocket("wss://ws.swanfi.pro/ws");

ws.onopen = () => {
  ws.send(JSON.stringify({ action: "subscribe", channel: "new_token" }));
};

ws.onmessage = (event) => {
  const msg = JSON.parse(event.data);
  console.log(`[${msg.channel}]`, msg.data);
};
```

---

## API Reference

<details>
<summary><b>GET /v1/tokens</b> — Latest token launches</summary>

Returns the latest token launches across supported launchpads.

**Query Parameters**

| Parameter | Type | Values | Default | Description |
|---|---|---|---|---|
| `source` | string | `flap`, `four_meme` | all | Filter by launchpad |
| `sort` | string | `newest`, `oldest`, `marketcap`, `volume` | `newest` | Sort order |
| `limit` | integer | `1–100` | `50` | Number of results |
| `offset` | integer | `0+` | `0` | Pagination offset |

**Example Response**

```json
[
  {
    "tokenAddress": "0xabc123...def456",
    "name": "PepeFlight",
    "symbol": "PFLT",
    "sourceFrom": "flap",
    "basePair": "BNB",
    "launchTime": "2026-05-13T08:00:00.000Z",
    "priceUsdt": 0.0000038,
    "marketCap": 3800,
    "volume24h": 1500,
    "txCount": 52,
    "holderCount": 15,
    "mode": "bonding",
    "progress": 12.5,
    "targetUSD": 10000
  }
]
```
</details>

<details>
<summary><b>GET /v1/transactions</b> — Buy/sell history</summary>

Returns transaction history across all tokens or filtered to a specific token.

**Query Parameters**

| Parameter | Type | Values | Default | Description |
|---|---|---|---|---|
| `token` | string | `0x...` | — | Filter by token address |
| `wallet` | string | `0x...` | all | Filter by wallet address |
| `position` | string | `BUY`, `SELL` | all | Transaction direction |
| `from` | integer | unix timestamp | — | Start of time range |
| `to` | integer | unix timestamp | — | End of time range |
| `limit` | integer | `1–200` | `50` | Number of results |

**Example Response**

```json
[
  {
    "txHash": "0xdeadbeef...1234",
    "tokenAddress": "0xabc123...def456",
    "walletAddress": "0xuser...wallet",
    "position": "BUY",
    "amountBNB": 0.5,
    "amountToken": 1312500,
    "priceUsdt": 0.0000038,
    "valueUsdt": 190.0,
    "timestamp": "2026-05-13T08:04:22.000Z",
    "isWhale": false,
    "isDev": false
  }
]
```
</details>

<details>
<summary><b>GET /v1/wallets/:address/tracker</b> — Portfolio snapshot</summary>

Returns a complete on-chain portfolio snapshot for any wallet.

**Example Response**

```json
{
  "walletAddress": "0xuser...wallet",
  "totalRealizedPnlUsdt": 412.50,
  "totalUnrealizedPnlUsdt": -38.00,
  "totalValueUsdt": 890.00,
  "tradeCount": 134,
  "winRate": 0.61,
  "positions": [
    {
      "tokenAddress": "0xabc123...def456",
      "symbol": "PFLT",
      "balance": 1312500,
      "avgBuyPriceUsdt": 0.0000032,
      "currentPriceUsdt": 0.0000038,
      "unrealizedPnlUsdt": 7.87,
      "unrealizedPnlPercent": 18.75,
      "totalInvestedUsdt": 42.00
    }
  ]
}
```
</details>

---

## WebSocket Channels

| Channel | Description |
|---|---|
| `new_token` | New token launch detected on Flap.sh or Four.meme |
| `token_update` | Price, market cap, volume, and holder count updates |
| `migrate` | Token migration from bonding curve to PancakeSwap DEX |
| `transaction:all` | All buy/sell events across all tokens |
| `transaction:{address}` | Buy/sell events for a specific token |
| `price:{address}` | Real-time price ticks for a specific token |
| `candle:{address}` | OHLCV candle updates (1s to 1d) for a specific token |
| `holder:{address}` | Holder balance changes for a specific token |

---

## Supported Launchpads

| Platform | Status | Chain | Scope |
|---|---|---|---|
| [Flap.sh](https://flap.sh) | ✅ Live | BNB Chain | Launch, bonding, migrate |
| [Four.meme](https://four.meme) | ✅ Live | BNB Chain | Launch, bonding, migrate |
| PancakeSwap V2 / V3 | ✅ Live | BNB Chain | Post-migration DEX swaps |

---

## Data Coverage

| Data Type | Coverage |
|---|---|
| Token launches | Real-time — all Flap.sh & Four.meme |
| Transactions | Every on-chain BUY/SELL event |
| Candle timeframes | 1s · 15s · 30s · 1m · 5m · 15m · 30m · 1h · 4h · 1d |
| Holder snapshots | Updated every 60 seconds via multicall |
| Bonding progress | Atomic real-time updates per transaction |
| Migration events | Detected within 1 block of on-chain confirmation |

---

## Security

- **SHA-256 hashed API keys** — raw keys are never stored
- **Per-key rate limiting** — configurable per tier
- **Request monitoring** — endpoint-level granularity
- **CORS enforcement** — restricted to approved origins
- **Read-only data platform** — no user funds, no signing, no custody

---

## Get API Access

SwanFi API is currently in **private beta**. To request access:

1. Follow [@swanfidotpro](https://x.com/swanfidotpro) on X
2. Join the [Telegram community](https://t.me/swanficommunity)
3. Send a message describing your use case

All requests are reviewed within **24 hours**.

---

<div align="center">

**[swanfi.pro](https://swanfi.pro)** · **[API Docs](https://api.swanfi.pro)** · **[Telegram](https://t.me/swanficommunity)** · **[X / Twitter](https://x.com/swanfidotpro)**

<sub>Built with precision for BSC traders · © 2026 SwanFi Labs</sub>

</div>
