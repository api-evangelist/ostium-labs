---
name: Read and stream Ostium market data
description: Pull pairs, prices, candles and simulated orderbook/slippage, and stream live prices — no auth required.
api: https://docs.ostium.com/developer/sdk/overview
method: generated
source: https://docs.ostium.com/developer/reference
operations: [getPairs, getAllPrices, getCandles, getSimOrderbook, getSimSlippage, streamPrices]
---

# Read and stream Ostium market data

Market/data reads need no credentials — use `createReadOnly` (or hit the public Builder API directly). No wallet or key is required.

## Steps

1. **List markets.** Call `getPairs` for all pairs, leverage limits, live prices and market-state fields.
2. **Quote prices.** Call `getAllPrices` for current mid/bid/ask across pairs (Builder API `GET /v1/prices`).
3. **Chart history.** Call `getCandles` for OHLC data (supports pagination); Builder API `POST /v1/ohlc`.
4. **Estimate execution.** Call `getSimOrderbook` for a synthetic bid/ask book and `getSimSlippage` for long/short slippage at different notionals before opening.
5. **Stream live.** Call `streamPrices` for a WebSocket price feed (Builder API `WS /v1/prices/stream`).

## Rules

- Read errors surface as `OstiumSubgraphError` codes: `INVALID_PARAMS`, `FETCH_FAILED`, `NOT_FOUND`, `PARSE_ERROR`.
- Subgraph reads go through `/v1/subgraph/gn` (GraphQL); the SDK wraps it.
