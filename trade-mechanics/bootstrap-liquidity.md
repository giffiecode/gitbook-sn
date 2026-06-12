# Market Liquidity

Markets launch with zero open interest. Two actors seed liquidity together.

***

## Vault — vAMM counterparty

* Deposits initialise `totalAssets` and seed the vAMM with `base = quote = √k` reserves (typically sized to vault TVL).
* The vault is the **silent counterparty** to every vAMM swap. Trade flow that reaches the pool lands on the vault's book via `accrueBase` / `vaultPnL`.
* The vault does not post orders. Its exposure is the aggregate net of all vAMM flows.

***

## MM — order book limit orders

* Posts two-sided limit-orders around fair value of the fixed rate&#x20;
* Runs **vAMM↔oracle arbitrage** when implied rate deviates from float, re-anchoring the pool and limiting vault counterparty swings.

***

## Why both are needed

| Without vault                                            | Without MM bot                                   |
| -------------------------------------------------------- | ------------------------------------------------ |
| No vAMM fallback when LOB has no match                   | All flow hits thin vAMM; vault takes every trade |
| Lenders/borrowers with no LOB match have nowhere to fill | vAMM rate decouples; vault marks down sharply    |

***

## How flow is routed

1. **Most flow → LOB.** The router sends each order to the cheaper venue. With a tight MM ladder (±2 bps), the LOB wins most price comparisons. Vault is not involved.
2. **Residual flow → vAMM.** Large orders or one-sided books spill to the pool. Vault takes counterparty PnL.
3. **Arb loop anchors vAMM.** After large vAMM swaps, the MM corrects rate divergence&#x20;

At baseline launch throughput, the split is roughly **55% LOB / 45% vAMM**. LOB share rises as depth grows. At very high throughput the vAMM share climbs again (throughput ceiling).
