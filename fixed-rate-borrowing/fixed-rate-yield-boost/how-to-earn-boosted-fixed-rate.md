# How to Earn Boosted Fixed Rate

A step-by-step guide to boosting your Aave / Morpho lending yield with a short fixed-rate position on Supernova.

---

## Step 1 — Lend on Aave / Morpho

Deposit into the underlying money market as you normally would — same collateral, same counterparty risk, same floating yield.

Your capital stays in the protocol's liquid variable-rate pool. Nothing changes about how you lend or withdraw.

---

## Step 2 — Short fixed rate when fixed > float

Open a **short fixed-rate** position on Supernova: you **receive fixed** and **pay floating**.

Enter only when the market's implied fixed rate is **above** the current floating lending rate — that spread is your boost.

$$
\text{boost} \approx \text{fixed rate} - \text{floating rate}
$$

Typical boost: **200–250 bps**.

---

## Step 3 — Choose your execution

Set your Desired APR — this determines the fixed rate you want and whether your boost enters as a market or limit short.

| Method | When to use |
|---|---|
| **Market order** | Execute immediately at the best available implied rate |
| **Limit order** | Wait for your target fixed rate or better before filling |

Both route to the order book or vAMM, whichever offers the better price.

---

## Step 4 — Earn boosted fixed rate

Once the short is open, floating payments on Supernova are offset by floating yield from your Aave / Morpho deposit:

$$
\text{net yield} \approx \underbrace{\text{floating from Aave}}_{\text{earn}} - \underbrace{\text{floating on Supernova}}_{\text{pay}} + \underbrace{\text{fixed from Supernova}}_{\text{earn}} \;\approx\; \text{boosted fixed rate}
$$

You keep ~**99.8%** of capital on Aave / Morpho and can withdraw anytime liquidity permits. Close the Supernova short separately when you want to return to pure floating yield.

---

## Summary

1. **Lend** on Aave / Morpho (floating yield, full liquidity)
2. **Short fixed rate** on Supernova when fixed > float
3. **Market or limit order** — your choice on timing and price
4. **Earn** boosted fixed rate while staying flexible on withdrawals

To see more about how boost yield works and how Effective Supply APY is calculated, see [How Boost Yield Math Works](how-boost-yield-math-works.md).
