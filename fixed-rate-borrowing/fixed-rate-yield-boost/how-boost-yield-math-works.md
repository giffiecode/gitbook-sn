# How Boost Yield Math Works

## The two legs

| Leg | Venue | You receive | You pay |
|---|---|---|---|
| Lending | Aave | Floating supply yield | — |
| Short rate | Supernova | Fixed rate | Floating rate |

A **short** on Supernova means you **receive fixed** and **pay floating** for the chosen tenor.

The floating you earn from Aave and the floating you pay on Supernova are built to **offset each other**, leaving the fixed rate as your yield on the boosted slice. The sections below show the actual cashflows — and why utilization drift after the short opens is usually still favorable.

---

## The cashflows

### Aave Lending

You keep earning floating **supply** yield on the boosted lend. On Aave, you receive per unit time:

$$
L \cdot r_{\text{supply}} = L \cdot (1 - rf) \cdot u_{\text{underlying, now}} \cdot r_{\text{borrow, now}}
$$

where $rf$ is the Aave **reserve factor**, $u_{\text{underlying, now}}$ is pool **utilization**, and $r_{\text{borrow, now}}$ is the floating **borrow rate**. Utilization and borrow rate here are **live** — they move with the pool after you boost.

### Supernova short

To boost your Position Amount $L$, you take a **short position with notional**:

$$
N = L \cdot (1 - rf) \cdot u_{\text{open}}
$$

$u_{\text{open}}$ is the pool **utilization at open** — it sizes your short notional and pairs with the locked fixed rate $r_{\text{fixed, open}}$. For a **market short**, it is pool utilization when the order executes; for a **limit short**, it is the utilization implied by your Desired APR when the order fills. Unlike $u_{\text{underlying, now}}$, it does not change after the short opens.

Holding that short leads to the following cashflows — you pay floating and receive fixed on $N$ per unit time:

$$
\begin{aligned}
\text{pay (floating)} &= N \cdot r_{\text{borrow, now}} = L \cdot (1 - rf) \cdot u_{\text{open}} \cdot r_{\text{borrow, now}} \\
\text{receive (fixed)} &= N \cdot r_{\text{fixed, open}} = L \cdot (1 - rf) \cdot u_{\text{open}} \cdot r_{\text{fixed, open}}
\end{aligned}
$$

### Total P&L on the boosted slice

Adding the Aave leg and the two Supernova legs together:

$$
\text{Total PnL} = L \cdot (1 - rf) \Big[ u_{\text{open}} \cdot r_{\text{fixed, open}} + r_{\text{borrow, now}} \cdot \big(u_{\text{underlying, now}} - u_{\text{open}}\big) \Big]
$$

The first term is the **locked fixed** on your short notional. The second term is what changes when pool utilization moves away from $u_{\text{open}}$. If utilization never moved ($u_{\text{underlying, now}} = u_{\text{open}}$), the floating legs cancel exactly and you earn the locked fixed.

---

## Why utilization changes are still favorable

The boost is imperfect when utilization moves away from $u_{\text{open}}$. But both directions still work in the lender's favor:

- **Float goes up** ($u_{\text{underlying, now}} > u_{\text{open}}$): the lender earns more than the locked rate, since the lender gets a higher payment from Aave than what they owe on their short position floating payments.
- **Float goes down** ($u_{\text{underlying, now}} < u_{\text{open}}$): the lender earns less than the locked rate, but the payment from the long is greater than what it would have gotten from the Aave pool.

---

## Effective Supply APY

**Effective Supply APY** is the headline yield on your **entire** Aave balance for that asset — boosted and unboosted slices combined. This is the number shown in the app.

Each boost locks a **boost rate** on the slice you boosted:

$$
r_{\text{boost}} = (1 - rf) \cdot u_{\text{open}} \cdot r_{\text{fixed, open}}
$$

Effective Supply APY is the **size-weighted blend** of each boosted slice earning its locked $r_{\text{boost}}$ and any remaining unboosted lend earning the live supply rate $r_{\text{supply}}$. If only part of your lend is boosted, the headline sits between $r_{\text{supply}}$ and your locked boost rate(s).