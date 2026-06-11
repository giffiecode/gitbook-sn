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

# Close Position

Closing a position means exiting at the **current implied rate**, not the floating rate.

At close:

* The position is offset by taking the opposite side at the new implied rate for the same remaining tenor.
* The **difference between the entry implied rate and exit implied rate** determines realized PnL.
* All **floating-rate accruals** up to that point are already settled continuously as cashflow, so closing only realizes the change in implied rate, not past accruals.

Effectively, closing crystallizes the mark-to-market value of the rate view while leaving prior floating settlements untouched.
