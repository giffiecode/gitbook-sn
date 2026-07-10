# How Boost Yield Math Works

Plain-language guide to the numbers in Yield Boost: what you earn on Aave, what you pay and receive on the Supernova short, and how the two offset into a boosted fixed yield. Also covers what **Effective Supply APY** means on your total Aave balance.

---

## The two legs

| Leg | Where | You receive | You pay |
|---|---|---|---|
| Lending | Aave | Floating supply yield | — |
| Short rate | Supernova | Fixed rate | Floating rate |

A **short** on Supernova means you **receive fixed** and **pay floating** for the chosen tenor. **Position Amount (L)** is how much of your Aave lend you choose to hedge.

The floating you earn from Aave and the floating you pay on Supernova are built to **offset each other**, leaving the fixed rate as your yield on the hedged slice. The sections below show the actual cashflows — and why utilization drift after the short opens is usually still favorable.

**When boost is not attractive:** if the implied fixed rate is at or below the floating **borrow** rate, shorting does not beat simply lending on Aave. Enter when fixed is **above** floating borrow, or set a higher Desired APR as a limit and wait.

---

## The cashflows

### Aave (ongoing)

You keep earning floating **supply** yield on the hedged lend. On Aave you earn:

$$
L \cdot r_{\text{supply}} = L \cdot (1 - rf) \cdot u_{\text{underlying, now}} \cdot r_{\text{borrow, now}}
$$

where \(rf\) is the Aave **reserve factor**, \(u\) is pool **utilization**, and \(r_{\text{borrow}}\) is the floating **borrow rate**. Utilization and borrow rate here are **live** — they move with the pool after you boost.

### Supernova short (locked at open)

To hedge your Position Amount \(L\), you take a **short position with notional**:

$$
N = L \cdot (1 - rf) \cdot u_{\text{open}}
$$

Holding that short leads to the following cashflows — you pay floating and receive fixed on \(N\):

$$
\begin{aligned}
\text{pay (floating)} &= N \cdot r_{\text{borrow, now}} = L \cdot (1 - rf) \cdot u_{\text{open}} \cdot r_{\text{borrow, now}} \\
\text{receive (fixed)} &= N \cdot r_{\text{fixed, open}} = L \cdot (1 - rf) \cdot u_{\text{open}} \cdot r_{\text{fixed, open}}
\end{aligned}
$$

Floating on Supernova uses **today’s** borrow rate, but utilization is **locked when the short opens**. Fixed is also locked at open.

\(u_{\text{open}}\) is the utilization locked in when your short opens, and depends on the order type:

| Order type | \(u_{\text{open}}\) |
|---|---|
| **Market short** | \(u_{\text{underlying, open}}\) — pool utilization when the short opens |
| **Limit short** | \(u_{\text{implied, open}}\) — utilization implied by your Desired APR / fixed rate at open |

For a limit short, that slice of lend stays on pure Aave floating until the order **executes**; the short legs above turn on at execution.

### Total P&L on the hedged slice

Adding the Aave leg and the two Supernova legs together:

$$
\text{Total P\&L} = L \cdot (1 - rf) \Big[
u_{\text{open}} \cdot r_{\text{fixed, open}}
+ r_{\text{borrow, now}} \cdot \big(u_{\text{underlying, now}} - u_{\text{open}}\big)
\Big]
$$

The first term is the **locked fixed** on your hedge notional. The second term is what changes when pool utilization moves away from \(u_{\text{open}}\). If utilization never moved (\(u_{\text{underlying, now}} = u_{\text{open}}\)), the floating legs cancel exactly and you earn the locked fixed.

---

## Why utilization changes are still favorable

The hedge is imperfect when utilization moves away from \(u_{\text{open}}\). But both directions still work in the lender's favor:

- **Float goes up** (\(u_{\text{underlying, now}} > u_{\text{open}}\)): the lender gets a higher payment from Aave than what they owe to the long — borrow income grows with utilization while the swap notional was fixed, so the lender earns **more** than the locked rate.
- **Float goes down** (\(u_{\text{underlying, now}} < u_{\text{open}}\)): the payment from the long is greater than what it would have gotten from the Aave pool — the lender earns **less** than the locked rate, but \(r_{\text{borrow, now}}\) is also lower, dampening the shortfall.

Because \(r_{\text{borrow}}\) and \(u\) move together on the interest rate curve, the payoff is **convex in utilization** — gains from rising utilization are amplified, losses from falling utilization are dampened.

---

## Effective Supply APY

**Effective Supply APY** is the blended yield on your **entire** Aave balance for that asset — not only the hedged slice.

### Why it is not equal to Desired APR

Suppose you have $10,000 on Aave and boost $5,000:

| Slice | Size | Earns roughly |
|---|---|---|
| Hedged | $5,000 | Boosted profile from the short (formulas above) |
| Unhedged | $5,000 | Aave floating |

The headline Effective Supply APY is a **blend**. Boosting half your balance cannot make the whole $10,000 earn Desired APR.

In simple terms (one clip):

$$
\text{Effective Supply APY} \approx \frac{L_{\text{hedged}}}{L_{\text{total}}} \times r_{\text{boost}} + \frac{L_{\text{unhedged}}}{L_{\text{total}}} \times r_{\text{float}}
$$

(The app also weights time to each clip’s maturity when you have several boosts. The idea stays the same: blend hedged and floating pieces over your total lend.)

### Current vs projected

| Label | Meaning |
|---|---|
| **Current** Effective Supply APY | Blended APY from boosts you already have open |
| **Projected** Effective Supply APY | Estimate **after** the new clip you are about to confirm |

In the modal, the arrow compares current → projected for the quoted size.

### Market vs limit

| Order type | What the projection assumes |
|---|---|
| **Market Short** | Fills now at **current** pool utilization (\(u_{\text{underlying, open}}\)) |
| **Limit Short** | Fills later at your Desired APR; notional uses \(u_{\text{implied, open}}\). Until fill, that slice stays on **floating** |

**Pending hedged lend:** open limit shorts reserve Position Amount before fill. That slice is not yet on the boosted profile.

---

## Worked example

**Setup**

- Aave USDC lend: **$10,000**
- You boost: **$5,000** (half your balance)
- Reserve factor \(rf\) ≈ **10%**
- Utilization at open \(u_{\text{underlying, open}}\) ≈ **80%**
- Fixed rate at open \(r_{\text{fixed, open}}\) ≈ **6.5%**
- Current floating supply APY on the unhedged slice ≈ **4.0%**

**1. Notional**

$$
N = \$5{,}000 \cdot (1 - 0.10) \cdot 0.80 = \$3{,}600
$$

**2. At open, if utilization has not moved yet**

Floating from Aave and floating paid on Supernova cancel on that notional. You are left receiving fixed on \(N\):

$$
\text{receive fixed} \approx \$3{,}600 \cdot 6.5\%
$$

**3. If utilization rises after open**

Aave pays on \(u_{\text{now}} > u_{\text{open}}\) while you still only pay floating on \(u_{\text{open}}\) — so you keep a positive float residual **on top of** the locked fixed.

**4. Effective Supply APY (simple blend on total lend)**

| Slice | Size | Rate (illustrative) |
|---|---|---|
| Hedged | $5,000 | ~boosted fixed profile |
| Unhedged | $5,000 | ~4.0% floating |

$$
\text{Effective Supply APY} \approx 0.5 \times 6.5\% + 0.5 \times 4.0\% = 5.25\%
$$

| Number | Value |
|---|---|
| Fixed locked on the hedge notional | **~6.5%** |
| Effective Supply APY on the **whole** $10k | **~5.25%** |

That gap is expected whenever you only boost part of your lend.

**5. Margin (order of magnitude)**

Around **~0.2%** of the lending position is posted as Supernova margin (separate USDC/USDT — not taken from your aTokens).

---

## Quick reference

| Term | Meaning |
|---|---|
| **Position Amount (L)** | Aave lend you choose to hedge |
| **Notional (N)** | \(L \cdot (1-rf) \cdot u_{\text{open}}\) (market: \(u_{\text{underlying, open}}\); limit: \(u_{\text{implied, open}}\)) |
| **Desired APR** | Target fixed rate; also chooses market vs limit |
| **Supply rate** | \(r_{\text{supply}} = (1-rf)\,u_{\text{underlying, now}}\,r_{\text{borrow, now}}\) |
| **Aave receive** | \(L \cdot r_{\text{supply}} = L(1-rf)\,u_{\text{underlying, now}}\,r_{\text{borrow, now}}\) |
| **Short pay / receive** | Floating / fixed on open (or implied) utilization × \(L(1-rf)\) |
| **Effective Supply APY** | Blended APY on **total** Aave lend |
| **Projected APY** | Estimate after the new clip (limit: after assumed fill) |
| **Pending hedged** | L reserved by open limit shorts (not boosted until fill) |

---

## Related

- [How to Earn Boosted Fixed Rate](how-to-earn-boosted-fixed-rate.md) — step-by-step flow
- [Lend at Boosted Fixed Rate on Aave / Morpho](../user-guide-one-click-fixing-rate/lend-at-boosted-fixed-rate-on-aave-morpho.md) — one-click product guide
- [Fixed Rate Yield Boost](README.md) — product overview
