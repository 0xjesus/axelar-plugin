# Axelar Data Provider Plugin

NEAR Intents Bounty - Provider: Axelar

## Quick Start

```bash
bun install
cd packages/_plugin_template && bun test
```

Expected: 15 pass, 0 fail

## Implementation

| Metric | Source |
|:---|:---|
| Volume (24h/7d/30d) | Axelarscan API `/token/transfersTotalVolume` |
| Rates & Fees | AxelarJS SDK `getTransferFee()` + 0.1% fallback |
| Liquidity Depth | Axelarscan API `/getTVL` |
| Available Assets | Axelarscan API `/getAssets` |

All data sources use live Axelar network APIs with conservative fallbacks when unavailable.

## Data Sources

**Volume**
- Endpoint: `POST /token/transfersTotalVolume`
- Returns real transfer volume in USD

**Rates & Fees**
- Method: `getTransferFee(sourceChain, destChain, asset, amount)`
- Real transfer fees from Axelar network

**Liquidity Depth**
- Endpoint: `POST /getTVL`
- Calculation: 50% TVL at 50bps, 75% TVL at 100bps

**Available Assets**
- Endpoint: `GET /getAssets`
- Returns supported assets with addresses per chain

## Technical Details

**Resilience**
- Exponential backoff retry (1s, 2s, 4s)
- Rate limiting (10 req/sec)
- TypeScript + Zod validation
- Graceful error handling

**Configuration**

```bash
BASE_URL=https://api.axelarscan.io/api/v1
TIMEOUT=30000
MAX_REQUESTS_PER_SECOND=10
API_KEY=
```

**Tests**

15 tests covering snapshot structure, volume windows, rate calculation, liquidity thresholds, asset lists, multiple routes, input validation, and health checks.
