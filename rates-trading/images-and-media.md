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

# Open Position

**Fixed rates are settled upfront at position opening, while floating rates accrues over time during the position’s holding period.**&#x20;



**Longs** pay the fixed rate at entry, fully satisfying their liabilities and eliminating liquidation risk.&#x20;



**Shorts** receive the fixed rate upfront and pay the floating rate over time. Since the floating rate can rise unpredictably, shorts face potential liquidation if their margin becomes insufficient to cover mark-to-market losses.  &#x20;



**Initial Margin is calculated by:**&#x20;

```
Initial Margin = Notional Size * Implied Rate * Time Duration
```



