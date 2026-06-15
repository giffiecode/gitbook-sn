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

# Short Rate

**Short Rate = Earn Fixed Rate, Pay Floating Rate**

_(Directly benefits from falling implied rates. Continuously benefits from falling floating rate as less settlement payment accruals)_\
\
In essence, a Short Rate position profits from a decrease in the implied cost of capital over the selected tenor.

Entering a Short Rate position means **receiving the fixed rate** and **paying the floating rate** from the underlying market. This position benefits when floating rates decline relative to the fixed rate. Short Rate positions express the view that market rates will fall or that fixed rates are currently overpriced. They are often used for **boosting lending yield or directional rate exposure**, not for hedging. \
\
Given implied rate is expected to trade above floating rate, **lenders can short the implied rate**, paying floating and receiving fixed, **to convert a floating-rate yield into a boosted fixed-rate yield.**&#x20;

Since the yield curve is typically upward-sloping, implied forward rates embedded in the swap curve usually trade above the prevailing spot floating rate. Locking in a fixed rate derived from this forward curve, rather than the current floating rate, allows lenders to capture a yield pickup, effectively monetizing the spread between spot and forward rates as a fixed-rate boost.

