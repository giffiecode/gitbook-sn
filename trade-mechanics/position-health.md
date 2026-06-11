# Position Health

Every participant holds a two-field position per market:

| Field   | Meaning                                                                 |
| ------- | ----------------------------------------------------------------------- |
| `base`  | Collateral posted (stablecoin, WAD-scaled)                              |
| `quote` | Signed notional — positive = long (borrower), negative = short (lender) |

Only **shorts** (`quote < 0`) can become insolvent. Longs receive funding inflows and stay solvent under normal conditions.

***

## Health ratio

Debt is the cost to close the short at the current vAMM price (TWAP, not spot):

$$
\text{debt} = |Q| \times \frac{\text{vAMM price}}{10^{18}}
$$

$$
\text{health} = \frac{\text{debt} \times 10^5}{\text{base}} \quad \leftarrow \text{lower is healthier}
$$

***

## Two LTV thresholds

| Check           | LTV                               | Used when                              |
| --------------- | --------------------------------- | -------------------------------------- |
| **Liquidation** | `ltvForMarket` (e.g. 75%)         | Triggering a liquidation               |
| **Safe**        | `ltvForMarket × 90%` (e.g. 67.5%) | After trades, withdrawals, foreclosure |

A short is liquidatable at 75% but must clear 67.5% after any action. The 7.5-point buffer is standard maintenance margin.&#x20;

***

## Funding erodes collateral first

Before any solvency check, outstanding funding settles into `base`:

$$
\text{payment} = |Q| \times \frac{\text{accPerNotional}_{\text{now}} - \text{poolDebtAcc}_{\text{entry}}}{10^{18}}
$$

| Side            | Effect            |
| --------------- | ----------------- |
| Long (`Q > 0`)  | `base += payment` |
| Short (`Q < 0`) | `base -= payment` |

If a short cannot pay, the deficit is written to the vault as bad debt and collateral is zeroed. A persistently high floating rate (e.g. 40–60% APR) can make a position liquidatable over hours or days — no sudden price spike required.
