---
name: Manage and close an Ostium position
description: Adjust TP/SL and collateral on an open Ostium position, then fully or partially close it.
api: https://docs.ostium.com/developer/sdk/overview
method: generated
source: https://docs.ostium.com/developer/reference
operations: [getOpenPositions, modifyOrder, updateCollateral, closeTrade, cancelOrder, getFills]
---

# Manage and close an Ostium position

Manage a live position with the Ostium Builder SDK in a write-enabled client mode.

## Steps

1. **Read the position.** Call `getOpenPositions` for the trader to get margin summary, liquidation context and withdrawable collateral.
2. **Adjust risk.** Call `modifyOrder` to update take-profit, stop-loss or a limit-order price. Call `updateCollateral` to add or remove collateral and change effective leverage / liquidation distance.
3. **Cancel pending orders** if needed with `cancelOrder` (pending limit, market-open or market-close orders).
4. **Close.** Call `closeTrade` to fully or partially close the position.
5. **Reconcile.** Call `getFills` (or `getFillsByTime`) to confirm the executed close and realized amounts.

## Rules

- Verify current state with `getOpenPositions` before every mutation; on-chain price moves change liquidation thresholds continuously.
- Errors surface as typed `OstiumError` codes (see errors/ostium-labs-error-codes.yml).
- For automated bots, subscribe to `streamAccountUpdates` / `streamPositionUpdates` instead of polling.
