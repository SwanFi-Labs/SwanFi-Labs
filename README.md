<div align="center">
  <img src="https://pbs.twimg.com/profile_images/2050882410242596864/pEYVkW_b_400x400.jpg" alt="SwanFi Labs" width="80" />
  
  <h1>SwanFi Labs</h1>
  
  <p><strong>Real-time BSC token discovery & trading infrastructure</strong></p>

  <p>
    <a href="https://swanfi.pro">
      <img src="https://img.shields.io/badge/Platform-Live-brightgreen?style=for-the-badge" />
    </a>
    <a href="https://api.swanfi.pro/health">
      <img src="https://img.shields.io/badge/API-v1.0-blue?style=for-the-badge" />
    </a>
    <a href="https://t.me/swanfiAnn">
      <img src="https://img.shields.io/badge/Telegram-Community-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" />
    </a>
    <a href="https://x.com/swanfipro">
      <img src="https://img.shields.io/badge/X-Follow-000000?style=for-the-badge&logo=x&logoColor=white" />
    </a>
  </p>
</div>

---

## What is SwanFi?

SwanFi is a high-performance, real-time token discovery and analytics platform built for the BNB Smart Chain ecosystem. We index every token launch, track bonding curve progression, monitor whale movements, and deliver live on-chain data to traders — milliseconds after it happens on-chain.

No delays. No middlemen. Raw on-chain data, served fast.

---

## Platform Services

| Service | URL | Description |
|---|---|---|
| Trading App | [swanfi.pro](https://swanfi.pro) | Real-time token discovery & trading interface |
| REST API | [api.swanfi.pro](https://api.swanfi.pro) | Public & private data endpoints |
| WebSocket | [ws.swanfi.pro](https://ws.swanfi.pro) | Live price, trade & token feeds |

---

## Core Features

**Token Discovery**
- Real-time detection of new token launches across Flap.sh and Four.meme
- Live bonding curve progress with target tracking
- Instant migration detection to PancakeSwap DEX

**On-Chain Analytics**
- Transaction history with buy/sell classification
- Holder tracking with balance, PnL, and paperhand detection
- Developer activity monitoring (dev buy, dev sell, dev hold)
- Top trader leaderboard with realized PnL

**Data Infrastructure**
- Multi-provider BSC RPC with latency-aware routing and circuit breaker
- WebSocket pub/sub broker with per-channel subscriptions
- OHLCV candle system (1s to 1d timeframes) with real-time aggregation
- PostgreSQL with optimized indexing for high-frequency writes

**API Access**
- Public API with per-key rate limiting (60 req/min)
- Private API for platform-internal use
- Versioned endpoints (`/v1/`) for stability guarantees

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
  <img src="https://img.shields.io/badge/Neon-00E699?style=flat-square&logo=postgresql&logoColor=black" />
</p>

---

## API Reference

SwanFi provides a public REST API for developers building on top of BSC token data.

**Base URL:** `https://api.swanfi.pro/v1`

**Authentication:** All requests require an API key via `x-api-key` header.

```http
GET /v1/tokens/latest
GET /v1/tokens/migrating
GET /v1/tokens/:address/trades
```

**Query Parameters:**

| Endpoint | Parameter | Values | Default |
|---|---|---|---|
| `/v1/tokens/latest` | `source` | `four_meme`, `flap` | all |
| `/v1/tokens/latest` | `sort` | `newest`, `oldest`, `marketcap`, `volume` | `newest` |
| `/v1/tokens/latest` | `limit` | `1–100` | `50` |
| `/v1/tokens/migrating` | `source` | `four_meme`, `flap` | all |
| `/v1/tokens/migrating` | `sort` | `newest`, `marketcap`, `volume` | `marketcap` |
| `/v1/tokens/:address/trades` | `wallet` | `0x...` | all wallets |
| `/v1/tokens/:address/trades` | `position` | `BUY`, `SELL` | all |
| `/v1/tokens/:address/trades` | `from` | unix timestamp | — |
| `/v1/tokens/:address/trades` | `to` | unix timestamp | — |
| `/v1/tokens/:address/trades` | `limit` | `1–200` | `50` |

**Rate limit:** 60 requests / minute per key

**Example Request:**

```bash
curl -H "x-api-key: YOUR_API_KEY" \
  "https://api.swanfi.pro/v1/tokens/latest?source=flap&sort=marketcap&limit=20"
```

**Example Response:**

```json
[
  {
    "tokenAddress": "0x...",
    "name": "Token Name",
    "symbol": "TKN",
    "sourceFrom": "flap",
    "basePair": "BNB",
    "launchTime": "2026-05-04T10:00:00.000Z",
    "priceUsdt": 0.0000035,
    "marketCap": 3500,
    "volume24h": 1200,
    "txCount": 48,
    "holderCount": 12
  }
]
```

> To request API access, reach out via [Telegram](https://t.me/swanfiAnn) or [X](https://x.com/swanfipro)

---

## WebSocket Reference

**Endpoint:** `wss://ws.swanfi.pro/ws`

**Protocol:** JSON over WebSocket

**Subscribe to a channel:**

```json
{ "action": "subscribe", "channel": "new_token" }
```

**Available Channels:**

| Channel | Description |
|---|---|
| `new_token` | New token launch events |
| `token_update` | Price, marketcap, volume updates |
| `migrate` | Token migration to DEX events |
| `transaction:all` | All buy/sell transactions |
| `transaction:{address}` | Transactions for a specific token |
| `price:{address}` | Price updates for a specific token |
| `candle:{address}` | OHLCV candle updates (1s to 1d) |
| `holder:{address}` | Holder balance changes |

**Example Message:**

```json
{
  "channel": "token_update",
  "data": {
    "tokenAddress": "0x...",
    "price": 0.0000038,
    "marketcap": 3800,
    "volume24h": 1500,
    "txCount": 52,
    "holderCount": 15,
    "mode": "bonding",
    "progress": 12.5,
    "targetUSD": 10000
  },
  "ts": 1714000000000
}
```

---

## System Architecture

```
BSC Mainnet
    │
    ├── WebSocket Block Listener
    │       │
    │       ├── Flap.sh Handler        → new tokens, buys, sells
    │       ├── Four.meme Handler      → new tokens, buys, sells
    │       └── PancakeSwap Handler    → post-migrate swaps
    │               │
    │               ├── transaction.repository  → PostgreSQL
    │               ├── candleBuilder           → token_candles (1s–1d)
    │               ├── bonding.service         → liquidity state
    │               └── wsbroker               → WebSocket clients
    │
    ├── WS Server    → ws.swanfi.pro
    │       └── Pub/Sub broker
    │               ├── new_token
    │               ├── token_update
    │               ├── transaction:{address}
    │               ├── candle:{address}
    │               └── migrate
    │
    └── REST API     → api.swanfi.pro
            ├── /tokens/*          (private)
            ├── /wallets/*         (private)
            ├── /platform/*        (private)
            └── /v1/*              (public, rate-limited)
```

---

## Supported Launchpads

| Platform | Status | Chain |
|---|---|---|
| [Flap.sh](https://flap.sh) | ✅ Live | BNB Chain |
| [Four.meme](https://four.meme) | ✅ Live | BNB Chain |
| PancakeSwap V2/V3 (post-migrate) | ✅ Live | BNB Chain |

---

## Data Coverage

| Metric | Coverage |
|---|---|
| Token launches | Real-time, all Flap.sh & Four.meme |
| Transactions | Every on-chain BUY/SELL event |
| Candle data | 1s, 15s, 30s, 1m, 5m, 15m, 30m, 1h, 4h, 1d |
| Holder snapshots | Updated every 60 seconds via multicall |
| Bonding progress | Atomic real-time updates per transaction |
| Migration events | Detected within 1 block of on-chain confirmation |

---

## Get API Access

SwanFi API is currently in **private beta**. To request access:

1. Follow us on [X (@swanfipro)](https://x.com/swanfipro)
2. Join our [Telegram](https://t.me/swanfiAnn)
3. Send a message with your use case

We review all requests and respond within 24 hours.

---

## Security

- All private endpoints protected by API key authentication
- Rate limiting enforced per key (configurable per tier)
- API keys stored securely with last-used tracking
- CORS restricted to approved origins
- No user funds handled — read-only data infrastructure

---

<div align="center">
  <br/>
  <p>
    <a href="https://swanfi.pro">swanfi.pro</a> ·
    <a href="https://t.me/swanfiAnn">Telegram</a> ·
    <a href="https://x.com/swanfipro">X (Twitter)</a>
  </p>
  <p><sub>Built with precision for BSC traders · © 2026 SwanFi Labs</sub></p>
</div>
