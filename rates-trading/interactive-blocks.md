---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# Liquidation

Liquidation closes an underwater **short** position before losses exceed posted collateral. Longs cannot be liquidated — they receive floating funding inflows, not liabilities.

---

## When it triggers

A short is liquidatable when its health ratio exceeds the market LTV (typically **75%**):

$$
\text{health} = \frac{|Q| \times \text{vAMM price} / 10^{18} \times 10^5}{\text{base}}
$$

Health is checked against a **TWAP** price (not spot) and after funding accrues into collateral. A high floating rate alone can push a short toward liquidation over time.

---

## What happens

1. Open orders are cancelled and collateral released.
2. Outstanding funding is settled into `base`.
3. The short is closed through the vAMM using the user's collateral.
4. A **5% penalty** is charged — **90%** to the liquidator, **10%** to the protocol.

If collateral cannot cover the close cost, the vault absorbs the shortfall as bad debt.

---

## Alternatives

**Partial liquidation** closes 10–100% of the position to restore health without a full unwind.

**Foreclosure** lets another party inject capital and take over the position with no vAMM swap — useful when pool liquidity is thin and a standard close would move price sharply.

---

For full mechanics, money flows, and vault guardrails, see [Position Health](../trade-mechanics/position-health.md), [Liquidation](../trade-mechanics/liquidation.md), and [Vault Guardrails](../trade-mechanics/vault-guardrails.md).
