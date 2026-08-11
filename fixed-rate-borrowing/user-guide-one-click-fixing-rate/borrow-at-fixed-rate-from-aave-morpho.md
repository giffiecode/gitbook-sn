---
description: >-
  Rates Exchange enables borrowers to secure fixed-rate borrowing from Aave /
  Morpho with one-click / API call
---

# Borrow at Fixed Rate From Aave / Morpho

{% hint style="info" %}
**Borrow (The "Credit" Product)**<br>

* **Certainty:** Converts Aave/Morpho’s variable liquidity into fixed-rate loans for a bank-like UX<br>
* **Predictability:** Locks in cost-of-capital for institutional borrowers and card's borrow-spend rails.<br>
* **Non-Custodial:** 99.6% of collateral stays within the Aave/Morpho protocol.
{% endhint %}

## Fixed Rate Borrowing from Aave / Morpho

{% stepper %}
{% step %}
### Borrow on Aave / Morpho

Initiate a standard floating-rate loan on Aave, Morpho or any money markets that Rates Exchange supports. Collateralization, LTV parameters, and counterparty risk remain unchanged. All collateral stay with lending protocol you select.
{% endstep %}

{% step %}
### Select Tenor for Fixing Rate

The system identifies active Aave debt exposures across supported markets and presents available tenors. A market-implied fixed rate is quoted based on on-chain long and short interest.
{% endstep %}

{% step %}
### Enter Long Rate Position

**Upon confirmation, the user enters a long rate position on Rates Exchange, paying fixed and receiving floating.** The floating-rate payments offset the variable borrow cost on Aave, Morpho, or the selected money market, effectively locking the borrowing cost for the chosen tenor until maturity.

To open the rates long position, a small amount of incremental collateral, typically **around 30 bps of the loan size, is bridged via Across to the Rates Exchange rate account to establish the position**. For borrowers taking the long-rate side, the fixed-rate obligation is settled upfront, eliminating liquidation risk on the long rate position itself. This doesn't eliminate the liquidation risk on the underlying money market protocol.
{% endstep %}

{% step %}
### Auto-Rolling (Optional)

If enabled, the expiring long-rate position is automatically closed and a new one is opened at the next available tenor, maintaining uninterrupted fixed-rate exposure.
{% endstep %}

{% step %}
### Manage or Exit Early (Optional)

Maintain flexibility by closing the long-rate position at any time. This reverts the loan to floating and allows full or partial repayment through Aave/Morpho’s standard process.
{% endstep %}

{% step %}
### Settlement at Expiry

At term end, the position settles, funds are bridged back, and the Aave / Morpho loan continues at floating if not repaid.
{% endstep %}
{% endstepper %}
