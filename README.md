<div align="center">

<img src="https://pbs.twimg.com/profile_images/2050882410242596864/pEYVkW_b_400x400.jpg" alt="SwanFi Labs" width="88" style="border-radius: 16px;" />

<br />
<br />

# SwanFi Labs

**Real-time BSC token discovery & trading infrastructure**

<p>
  <a href="https://swanfi.pro">
    <img src="https://img.shields.io/badge/Platform-Live-22c55e?style=for-the-badge&labelColor=09090b" />
  </a>
  &nbsp;
  <a href="https://api.swanfi.pro/health">
    <img src="https://img.shields.io/badge/API-v1.0-3b82f6?style=for-the-badge&labelColor=09090b" />
  </a>
  &nbsp;
  <a href="https://t.me/swanfiAnn">
    <img src="https://img.shields.io/badge/Telegram-Community-26A5E4?style=for-the-badge&logo=telegram&logoColor=white&labelColor=09090b" />
  </a>
  &nbsp;
  <a href="https://x.com/swanfipro">
    <img src="https://img.shields.io/badge/X_(Twitter)-Follow-ffffff?style=for-the-badge&logo=x&logoColor=white&labelColor=09090b" />
  </a>
</p>

<br />

> **Index every token launch. Track every transaction. Stream it all — in real time.**

</div>

---

## Overview

SwanFi is a **high-performance on-chain data platform** built for the BNB Smart Chain ecosystem. We provide developers and traders with the infrastructure to discover, analyze, and act on BSC token data the moment it hits the chain.

Our platform covers the full token lifecycle — from launch on Flap.sh and Four.meme, through bonding curve progression, to migration on PancakeSwap — and surfaces everything through a clean, versioned developer API and live WebSocket feeds.

```
New Token Launch → Indexing → Analytics → REST API / WebSocket → Your App
```

No polling. No stale data. No abstraction layers between you and the chain.

---

## Platform Services

| Service | Endpoint | Description |
|---|---|---|
| **Trading App** | [swanfi.pro](https://swanfi.pro) | Real-time token discovery & trading interface |
| **REST API** | [api.swanfi.pro](https://api.swanfi.pro) | Versioned data endpoints for developers |
| **WebSocket** | [ws.swanfi.pro](https://ws.swanfi.pro) | Live price, trade & token event feeds |

---

## Core Capabilities

<table>
<tr>
<td width="50%" valign="top">

**🔍 Token Discovery**
- Real-time detection of new launches on Flap.sh and Four.meme
- Live bonding curve progress with USD target tracking
- Instant migration detection to PancakeSwap DEX
- Token metadata, supply, and pricing from block zero

</td>
<td width="50%" valign="top">

**📊 On-Chain Analytics**
- Full transaction history with BUY/SELL classification
- Holder tracking: balance, PnL, paperhand detection
- Developer activity monitoring across the full lifecycle
- Top trader leaderboard with realized and unrealized PnL

</td>
</tr>
<tr>
<td width="50%" valign="top">

**⚡ Real-Time Feeds**
- Sub-second WebSocket event delivery
- Per-token subscription channels
- OHLCV candles from 1s to 1d, aggregated live
- Whale movement and holder change alerts

</td>
<td width="50%" valign="top">

**🛠 Developer Infrastructure**
- Versioned REST API with stability guarantees
- API key auth with per-key rate limiting and usage analytics
- Consistent response schemas across all endpoints
- Built for high-frequency trading app integrations

</td>
</tr>
</table>

---

## Tech Stack

<p>
  <img src="https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white" />
  <img src="https://img.shields.io/badge/Fastify-000000?style=flat-square&logo=fastify&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black" />
  <img src="https://img.shields.io/badge/BNB_Chain-F0B90B?style=flat-square&logo=binance&logoColor=black" />
  <img src="https://img.shields.io/badge/WebSocket-010101?style=flat-square&logo=socket.io&logoColor=white" />
  <img src="https://img.shields.io/badge/ethers.js-2535A0?style=flat-square&logo=ethereum&logoColor=white" />
  <img src="https://img.shields.io/badge/Neon_DB-00E699?style=flat-square&logo=postgresql&logoColor=black" />
</p>

---

## REST API Reference

**Base URL:** `https://api.swanfi.pro/v1`

All requests require an API key passed via the `x-api-key` header. Keys are rate-limited to **60 requests/minute** by default.

```http
x-api-key: swfi_your_api_key_here
```

---

### `GET /v1/tokens`

Returns the latest token launches across supported launchpads. Supports filtering by source, sorting, and pagination.

**Query Parameters**

| Parameter | Type | Values | Default | Description |
|---|---|---|---|---|
| `source` | string | `flap`, `four_meme` | all | Filter by launchpad origin |
| `sort` | string | `newest`, `oldest`, `marketcap`, `volume` | `newest` | Sort order |
| `limit` | integer | `1 – 100` | `50` | Number of results |
| `offset` | integer | `0+` | `0` | Pagination offset |

**Example — cURL**

```bash
curl -H "x-api-key: swfi_your_key" \
  "https://api.swanfi.pro/v1/tokens?source=flap&sort=marketcap&limit=20"
```

**Example — JavaScript**

```js
const res = await fetch("https://api.swanfi.pro/v1/tokens?source=four_meme&sort=newest&limit=10", {
  headers: { "x-api-key": "swfi_your_key" }
});
const tokens = await res.json();
```

**Example — Python**

```python
import requests

r = requests.get(
    "https://api.swanfi.pro/v1/tokens",
    params={"source": "flap", "sort": "marketcap", "limit": 20},
    headers={"x-api-key": "swfi_your_key"}
)
tokens = r.json()
```

**Response**

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

---

### `GET /v1/transactions`

Returns buy/sell transaction history across all tokens or filtered to a specific token. Ideal for whale tracking, activity feeds, and trade analytics.

**Query Parameters**

| Parameter | Type | Values | Default | Description |
|---|---|---|---|---|
| `token` | string | `0x...` | — | Filter by token address |
| `wallet` | string | `0x...` | all | Filter by wallet address |
| `position` | string | `BUY`, `SELL` | all | Transaction direction |
| `from` | integer | unix timestamp | — | Start of time range |
| `to` | integer | unix timestamp | — | End of time range |
| `limit` | integer | `1 – 200` | `50` | Number of results |

**Example — cURL**

```bash
curl -H "x-api-key: swfi_your_key" \
  "https://api.swanfi.pro/v1/transactions?token=0xabc123&position=BUY&limit=50"
```

**Example — JavaScript**

```js
const res = await fetch("https://api.swanfi.pro/v1/transactions?token=0xabc123&position=BUY", {
  headers: { "x-api-key": "swfi_your_key" }
});
const txs = await res.json();
```

**Response**

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

---

### `GET /v1/wallets/:address/tracker`

Returns a complete on-chain portfolio snapshot for any wallet. Includes realized PnL, unrealized PnL, current token positions, and historical trade activity.

**Path Parameters**

| Parameter | Description |
|---|---|
| `:address` | Wallet address (`0x...`) |

**Query Parameters**

| Parameter | Type | Values | Default | Description |
|---|---|---|---|---|
| `limit` | integer | `1 – 100` | `50` | Number of positions to return |

**Example — cURL**

```bash
curl -H "x-api-key: swfi_your_key" \
  "https://api.swanfi.pro/v1/wallets/0xuser...wallet/tracker"
```

**Example — Python**

```python
import requests

address = "0xuser...wallet"
r = requests.get(
    f"https://api.swanfi.pro/v1/wallets/{address}/tracker",
    headers={"x-api-key": "swfi_your_key"}
)
tracker = r.json()
```

**Response**

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

---

## WebSocket Reference

**Endpoint:** `wss://ws.swanfi.pro/ws`

SwanFi's WebSocket server delivers real-time on-chain events using a lightweight JSON pub/sub protocol. Subscribe to any channel on connection — events stream instantly as they are confirmed on-chain.

### Subscribing

```json
{ "action": "subscribe", "channel": "new_token" }
```

```json
{ "action": "unsubscribe", "channel": "new_token" }
```

### Available Channels

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

### Event Payloads

**`new_token`**

```json
{
  "channel": "new_token",
  "data": {
    "tokenAddress": "0xabc123...def456",
    "name": "PepeFlight",
    "symbol": "PFLT",
    "sourceFrom": "flap",
    "basePair": "BNB",
    "launchTime": "2026-05-13T08:00:00.000Z",
    "deployer": "0xdev...wallet"
  },
  "ts": 1747123200000
}
```

**`token_update`**

```json
{
  "channel": "token_update",
  "data": {
    "tokenAddress": "0xabc123...def456",
    "priceUsdt": 0.0000038,
    "marketCap": 3800,
    "volume24h": 1500,
    "txCount": 52,
    "holderCount": 15,
    "mode": "bonding",
    "progress": 12.5,
    "targetUSD": 10000
  },
  "ts": 1747123260000
}
```

**`transaction:{address}`**

```json
{
  "channel": "transaction:0xabc123...def456",
  "data": {
    "txHash": "0xdeadbeef...1234",
    "walletAddress": "0xuser...wallet",
    "position": "BUY",
    "amountBNB": 0.5,
    "amountToken": 1312500,
    "priceUsdt": 0.0000038,
    "valueUsdt": 190.0,
    "timestamp": "2026-05-13T08:04:22.000Z"
  },
  "ts": 1747123462000
}
```

**`candle:{address}`**

```json
{
  "channel": "candle:0xabc123...def456",
  "data": {
    "timeframe": "1m",
    "open": 0.0000034,
    "high": 0.0000041,
    "low": 0.0000033,
    "close": 0.0000038,
    "volume": 420.5,
    "time": 1747123200
  },
  "ts": 1747123260000
}
```

**`migrate`**

```json
{
  "channel": "migrate",
  "data": {
    "tokenAddress": "0xabc123...def456",
    "symbol": "PFLT",
    "sourceFrom": "flap",
    "pancakePair": "0xpair...address",
    "finalMarketCap": 10200,
    "migratedAt": "2026-05-13T09:15:00.000Z"
  },
  "ts": 1747127700000
}
```

---

### Full Connection Example

```js
const ws = new WebSocket("wss://ws.swanfi.pro/ws");

ws.onopen = () => {
  // Subscribe to global token launches
  ws.send(JSON.stringify({ action: "subscribe", channel: "new_token" }));

  // Subscribe to a specific token's transactions
  ws.send(JSON.stringify({ action: "subscribe", channel: "transaction:0xabc123...def456" }));
};

ws.onmessage = (event) => {
  const msg = JSON.parse(event.data);
  console.log(`[${msg.channel}]`, msg.data);
};

ws.onerror = (err) => console.error("WebSocket error:", err);
ws.onclose = () => console.log("Disconnected");
```

---

## API Key Dashboard

Every SwanFi API key comes with built-in usage analytics and quota monitoring, accessible via the developer dashboard at [swanfi.pro](https://swanfi.pro).

**What you get:**
- Daily and monthly usage counters per key
- Real-time quota tracking with overage protection
- Request logs with endpoint-level breakdown
- Key expiration management and rotation support
- Configurable rate limit tiers for high-volume applications

**Usage Stats Endpoint**

```bash
curl -H "x-api-key: swfi_your_key" \
  "https://api.swanfi.pro/v1/keys/me/usage"
```

```json
{
  "keyId": "key_abc123",
  "plan": "developer",
  "usageToday": 1240,
  "dailyQuota": 10000,
  "usagePercent": 12.4,
  "remainingQuota": 8760,
  "rateLimitPerMinute": 60,
  "resetsAt": "2026-05-14T00:00:00.000Z"
}
```

---

## Infrastructure

SwanFi is built from the ground up for **low-latency, high-frequency** on-chain data workloads.

```
BSC Mainnet (Full Node)
        │
        ├── Block Listener (WebSocket RPC)
        │       │
        │       ├── Flap.sh Event Handler       → new tokens, swaps, burns
        │       ├── Four.meme Event Handler      → new tokens, swaps, migrations
        │       └── PancakeSwap V2/V3 Handler    → post-migration DEX swaps
        │               │
        │               ├── Transaction Indexer   → PostgreSQL (Neon)
        │               ├── Candle Aggregator     → OHLCV (1s–1d, real-time)
        │               ├── Bonding Curve Tracker → progress, mode, target
        │               ├── Holder Multicall      → balance snapshots (60s)
        │               └── WebSocket Broker      → per-channel pub/sub
        │
        ├── WebSocket Server   wss://ws.swanfi.pro
        │       └── Pub/Sub Broker
        │               ├── new_token
        │               ├── token_update
        │               ├── migrate
        │               ├── transaction:{address}
        │               ├── candle:{address}
        │               ├── price:{address}
        │               └── holder:{address}
        │
        └── REST API Server    https://api.swanfi.pro
                └── /v1/*   (public, rate-limited, versioned)
```

**Key infrastructure properties:**

- **Fastify** backend — benchmarked for ultra-low overhead HTTP handling
- **WebSocket pub/sub broker** with per-channel subscriptions and connection pooling
- **Multi-provider BSC RPC** with latency-aware routing and automatic failover
- **Multicall-based holder indexing** — batch balance queries per block
- **Real-time OHLCV aggregation** — candles built tick-by-tick, not from snapshots
- **Bonding curve engine** — atomic progress updates per transaction confirmation
- **Neon PostgreSQL** — optimized schema with composite indexes for high-frequency writes

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

SwanFi API infrastructure is built with developer security as a first-class concern.

- **SHA-256 hashed API keys** — raw keys are never stored; only secure digests
- **Per-key rate limiting** — enforced at the gateway level, configurable per tier
- **Request monitoring** — all API activity logged with endpoint-level granularity
- **Abuse protection** — automated detection and blocking of malicious usage patterns
- **CORS enforcement** — restricted to approved origins
- **Read-only data platform** — no user funds, no signing, no custody

---

## Get API Access

SwanFi API is currently in **private beta**. We're onboarding developers building trading bots, analytics dashboards, portfolio trackers, and BSC data tooling.

**To request access:**

1. Follow [@swanfipro](https://x.com/swanfipro) on X
2. Join the [Telegram community](https://t.me/swanfiAnn)
3. Send a message describing your use case

All requests are reviewed and responded to within **24 hours**.

---

<div align="center">

<br />

**[swanfi.pro](https://swanfi.pro)** · **[API Docs](https://api.swanfi.pro/v1)** · **[Telegram](https://t.me/swanfiAnn)** · **[X / Twitter](https://x.com/swanfipro)**

<br />

<sub>Built with precision for BSC traders &nbsp;·&nbsp; © 2026 SwanFi Labs</sub>

</div>
