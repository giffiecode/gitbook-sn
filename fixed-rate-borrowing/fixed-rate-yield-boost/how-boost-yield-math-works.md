# How Boost Yield Math Works

A plain-language guide to the numbers behind Yield Boost: why a short locks in fixed yield, how position size maps to a Supernova hedge, and what **Effective Supply APY** means on your total Aave balance.

---

## The two legs

Boost Yield combines:

| Leg | Where | You receive | You pay |
|---|---|---|---|
| Lending | Aave | Floating supply yield | — |
| Short rate | Supernova | Fixed rate | Floating rate |

On the short, **receive fixed / pay floating** is the standard rates-market short: you lock in a fixed rate and pay whatever the underlying floating rate is over the tenor.

Your Aave deposit still earns floating. That floating income roughly **offsets** the floating you pay on Supernova, so the net profile looks like a **boosted fixed** yield on the hedged slice:

$$
\text{net on hedged slice} \approx \underbrace{\text{Aave float}}_{\text{earn}} - \underbrace{\text{Supernova float}}_{\text{pay}} + \underbrace{\text{Supernova fixed}}_{\text{earn}}
$$

When the fixed rate is above the floating lending rate, the leftover is your boost:

$$
\text{boost} \approx \text{fixed rate} - \text{floating lending rate}
$$

**When boost is not attractive:** if fixed ≤ floating, shorting does not improve your yield versus simply lending on Aave. Enter only when fixed trades **above** float (or set a limit Desired APR and wait).

---

## Position Amount (L) vs Notional (N)

In the Boost modal:

- **Position Amount (L)** — how much of your Aave lend you want to hedge (e.g. $5,000 of a $10,000 deposit).
- **Notional (N)** — the size of the Supernova short. This is usually **smaller than L**.

### Why N is smaller than L

On Aave, only part of a lending pool’s deposits are “rate-sensitive” at any moment — roughly the share that is borrowed, after the protocol’s reserve factor. Boost sizes the short to that slice so the floating legs line up:

$$
N \approx L \times u \times (1 - rf)
$$

where:

- \(u\) = pool **utilization** (how much of the pool is borrowed)
- \(rf\) = **reserve factor** (share of interest kept by the protocol)

**Intuition:** if utilization is 80% and reserve factor is 10%, then about \(0.80 \times 0.90 = 72\%\) of each dollar of lend is rate-sensitive — so a $5,000 Position Amount maps to roughly **$3,600** of Supernova notional, not $5,000.

You do not need to compute this by hand; the quote shows notional for you. The important takeaway: **hedging $L of lend does not mean a $L short.**

---

## Effective Supply APY

**Effective Supply APY** is the blended yield on your **entire** Aave balance for that asset — not only the hedged slice.

### Why it is not equal to Desired APR

Suppose you have $10,000 on Aave and boost $5,000:

| Slice | Size | Earns roughly |
|---|---|---|
| Hedged | $5,000 | Boosted fixed (from your short) |
| Unhedged | $5,000 | Aave floating |

Your headline Effective Supply APY is a **blend** of those two. Boosting half the balance cannot make the whole $10,000 earn Desired APR.

In simple terms (same tenor, one clip):

$$
\text{Effective Supply APY} \approx \frac{L_{\text{hedged}}}{L_{\text{total}}} \times r_{\text{boost}} + \frac{L_{\text{unhedged}}}{L_{\text{total}}} \times r_{\text{float}}
$$

(The app also accounts for time to each clip’s maturity when you have multiple boosts; the idea is the same — blend hedged and floating pieces over your total lend.)

### Current vs projected

| Label | Meaning |
|---|---|
| **Current** Effective Supply APY | Blended APY from boosts you already have open |
| **Projected** Effective Supply APY | Estimate **after** the new clip you are about to confirm |

Projected assumes the new hedge is included. Until a **limit** order fills, you are still on floating for that pending slice — so realized APY stays closer to current until fill.

### Market vs limit projections

| Order type | Projection assumption |
|---|---|
| **Market Short** | Fills now at **current** pool utilization |
| **Limit Short** | Fills later when the market reaches your Desired APR; projection uses the utilization implied by that limit rate |

Until a limit fills, you keep earning Aave float on that slice. Realized APY can differ if fill happens at a different utilization than assumed.

---

## Worked example

**Setup**

- Aave USDC lend: **$10,000**
- Current Aave floating supply APY: **4.0%**
- You boost: **$5,000** (half your balance)
- Desired / fill fixed rate on Supernova: **6.5%**
- Pool utilization \(u\) ≈ **80%**, reserve factor \(rf\) ≈ **10%**

**1. Boost on the hedged slice**

$$
\text{boost} \approx 6.5\% - 4.0\% = 2.5\% \text{ (250 bps)}
$$

**2. Notional on Supernova**

$$
N \approx \$5{,}000 \times 0.80 \times (1 - 0.10) = \$3{,}600
$$

**3. Margin (order of magnitude)**

Around **~0.2%** of the lending position is posted as Supernova margin (separate USDC/USDT — not taken from your aTokens). For a $5,000 clip that is on the order of **~$10**, before any 2× market buffer the UI may recommend.

**4. Effective Supply APY (simple blend)**

- Hedged $5,000 ≈ **6.5%**
- Unhedged $5,000 ≈ **4.0%**

$$
\text{Effective Supply APY} \approx 0.5 \times 6.5\% + 0.5 \times 4.0\% = 5.25\%
$$

So:

- Desired APR / fixed on the hedge ≈ **6.5%**
- Effective Supply APY on the **whole** $10k ≈ **5.25%**

That gap is expected whenever you only boost part of your lend.

---

## Quick reference

| Term | Meaning |
|---|---|
| **Position Amount (L)** | Aave lend dollars you choose to hedge |
| **Notional (N)** | Supernova short size ≈ \(L \times u \times (1-rf)\) |
| **Desired APR** | Target fixed rate; also chooses market vs limit |
| **Boost** | ≈ fixed − floating (when fixed > float) |
| **Effective Supply APY** | Blended APY on **total** Aave lend |
| **Projected APY** | Estimate after the new clip (limit: after assumed fill) |

---

## Related

- [How to Earn Boosted Fixed Rate](how-to-earn-boosted-fixed-rate.md) — step-by-step flow
- [Lend at Boosted Fixed Rate on Aave / Morpho](../user-guide-one-click-fixing-rate/lend-at-boosted-fixed-rate-on-aave-morpho.md) — one-click product guide
- [Fixed Rate Yield Boost](README.md) — product overview
