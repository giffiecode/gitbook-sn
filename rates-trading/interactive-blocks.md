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

Liquidation occurs when a trader’s margin becomes insufficient to cover mark-to-market losses from adverse movements in the **implied rate** or **underlying rate**. **Longs cannot be liquidated**, since they pay the fixed rate upfront and have no ongoing liabilities—floating settlements are inflows, not obligations.&#x20;



**Shorts** receive the fixed rate upfront and pay the floating rate over time. If either the implied rate or the underlying rate rises, their position value deteriorates while floating liabilities accrue. When margin falls below maintenance requirements, the system automatically liquidates the position at the current implied rate, using remaining margin to settle losses and return any excess collateral.



**For Shorts, Maintenance Margin is calculated by:**&#x20;

```
Maintenance Margin = Notional Size * Implied Rate * Time Duration * 0.75
```
