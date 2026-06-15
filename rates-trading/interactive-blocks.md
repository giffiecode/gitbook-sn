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

# Liquidation Threshold

Liquidation closes an underwater **short** position before losses exceed posted collateral. \
\
Currently, the system doesn't allow any explicit leverage. Long side settle all their fixed rate liabilities upfront and don't face any liquidation risk.&#x20;

Since short side's liability is the floating rate, short side's floating rate liability accruals continuously and might get liquidated when a) implied rate rises sharply or b) when floating rate stays high for a prolonged period of time.&#x20;

***

## Liquidation Threshold&#x20;

A short is liquidatable when its health ratio exceeds the market LTV (typically **75%**):

$$
\text{health} = \frac{|Q| \times \text{vAMM price} / 10^{18} \times 10^5}{\text{base}}
$$

Health is checked against a **TWAP** price (not spot) and after funding accrues into collateral. A high floating rate alone can push a short toward liquidation over time.

***

For full mechanics, money flows, and vault guardrails, see [Position Health](../market-mechanics/position-health.md), [Liquidation](../market-mechanics/liquidation.md), and [Vault Guardrails](../market-mechanics/vault-guardrails.md).
