# Fixed Rate Yield Boost

A product built for lenders who want **higher, predictable yield** without giving up the liquidity and security of Aave or Morpho.

Instead of locking capital into a fixed-term loan, you keep lending on-chain as usual and add a small short rate position on Supernova. The result is a **boosted fixed-rate return** — typically **200–250 bps above** the prevailing floating lending rate.

---

## Why lenders like it

**a) ~99.8% of capital stays on Aave / Morpho**

Nearly all of your deposit remains in the underlying money market's most liquid variable-rate pool. Only a thin margin (~30 bps of position size) is posted to Supernova to support the rates leg.

**b) Withdraw anytime**

You are not locked into a fixed-term loan. As long as Aave or Morpho has sufficient liquidity, you can withdraw your deposit on the usual terms. The Supernova position can be closed separately at any time.

**c) 200–250 bps fixed-rate premium, no term lock**

You earn a market-determined fixed rate on top of your floating lending yield — without committing to a maturity date on the lending side.

---

## How the economics work

You hold two legs that offset on the floating side:

| Leg | Venue | You receive | You pay |
|---|---|---|---|
| Lend | Aave / Morpho | Floating lending yield | — |
| Short fixed rate | Supernova | Fixed rate | Floating rate |

Floating payments roughly cancel, leaving:

$$
\text{net yield} \approx \text{fixed rate received on Supernova}
$$

When the market fixed rate trades **above** the underlying floating rate, that fixed leg delivers the **200–250 bps premium** lenders see in practice.

---

[How to earn boosted fixed rate →](how-to-earn-boosted-fixed-rate.md)

[How Boost Yield math works →](how-boost-yield-math-works.md)
