---
description: >-
  Supernova enables lenders to secure boosted fixed-rate on Aave / Morpho with
  one-click / API call
---

# Lend at Boosted Fixed Rate on Aave / Morpho

{% hint style="info" %}
**Lend (The "Savings" Product)**<br>

* **Yield Boost:** Delivers a 200–300 bps premium on top of standard Aave/Morpho lending rates.
* **Security:** 99.6% of assets remain settled directly within Aave /Morpho.&#x20;
* **Flexibility:** Users maintain the ability to withdraw at any time while retaining the fixed-rate yield profile.
{% endhint %}

## Lend at Boosted Fixed Rate on Aave / Morpho&#x20;

{% stepper %}
{% step %}
### Lend on Aave / Morpho&#x20;

Initiate a standard floating-rate lending position on Aave, Morpho, or any money markets that Supernova supports. Deposits in the underlying lending protocol, with collateral structure, counterparty risk, and yield source unchanged.
{% endstep %}

{% step %}
### Select Tenor for Fixing Rate&#x20;

The system identifies active lending positions across supported markets and presents available fixed-yield tenors. A market-determined fixed rate is quoted based on on-chain long and short interest, and is typically 200–250 bps above the prevailing floating lending rate.
{% endstep %}

{% step %}
### Enter Short Rate Position&#x20;

**Upon confirmation, the user enters a short rate position on Supernova, receiving fixed and paying floating.** The floating-rate payments owed on the short-rate position are offset by the floating lending yield earned from Aave, Morpho, or the selected money market, effectively converting the position into a fixed-rate lending position for the chosen tenor until maturity. The market-determined **fixed rate is typically 200–250 bps above the prevailing floating lending rate, providing lenders with a fixed-rate yield boost.**

To establish the position, a small amount of incremental collateral, typically around **30 bps of the lending position size, is bridged via Across to the Supernova rate account as margin.** Approximately **99.7% of lending capital remains deposited in the underlying money market** and **can be withdrawn at any time.**
{% endstep %}

{% step %}
### Auto-Rolling (Optional)&#x20;

If enabled, ParRate automatically closes the expiring long-rate position and opens a new one at the next available tenor, maintaining uninterrupted fixed-rate exposure.
{% endstep %}

{% step %}
### Manage or Exit Early (Optional)&#x20;

Maintain flexibility by closing the long-rate position at any time. This reverts the loan to floating and allows full or partial repayment through Morpho’s standard process.
{% endstep %}

{% step %}
### Settlement at Expiry&#x20;

At term end, the position settles, margin is bridged back, and your Aave / Morpho loan continues at floating&#x20;
{% endstep %}
{% endstepper %}
