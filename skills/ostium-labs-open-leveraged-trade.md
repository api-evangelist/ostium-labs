---
name: Open a leveraged trade on Ostium
description: Approve USDC, pick a market, and open a leveraged perpetual position with the Ostium Builder SDK.
api: https://docs.ostium.com/developer/sdk/overview
method: generated
source: https://docs.ostium.com/developer/reference
operations: [checkUsdcAllowance, approveUsdc, getPairs, getAllPrices, openTrade, getOpenOrders, getOpenPositions]
---

# Open a leveraged trade on Ostium

Use the `@ostium/builder-sdk` in a write-enabled client mode (self-signed or delegated). All collateral is USDC on Arbitrum; use `testnet: true` (Arbitrum Sepolia, chainId 421614) while developing.

## Steps

1. **Ensure USDC allowance.** Call `checkUsdcAllowance` for the required notional. If insufficient, call `approveUsdc` to let `TradingStorage` spend the trader's USDC. A missing allowance throws `OstiumError` code `ALLOWANCE_INSUFFICIENT`.
2. **Pick the market.** Call `getPairs` to list all 75 pairs with leverage caps, fees and market hours, and `getAllPrices` for the current mid/bid/ask. Confirm the market is open (stocks have day-trading windows).
3. **Open the trade.** Call `openTrade` with the pair, direction (long/short), collateral, leverage and order type (market, limit or stop). Optionally set take-profit / stop-loss bounds.
4. **Confirm.** Poll `getOpenOrders` for a pending limit/stop, or `getOpenPositions` once a market order fills to read margin summary and withdrawable collateral.

## Rules

- Write actions are on-chain transactions/user operations; replay safety comes from blockchain nonces, not an idempotency key. Do not blindly retry a submitted `openTrade` — re-read state first.
- Handle `OstiumError` codes: `VALIDATION_FAILED`, `SUBMISSION_FAILED`, `CONTRACT_ERROR`, `NETWORK_ERROR`.
- Use `get*Tx()` builders if your app signs externally (EOA or Safe).
