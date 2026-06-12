# Vault Guardrails

Circuit breakers and loss-sharing rules that bound vault risk.

***

## Exposure cap (flow control)

Default `exposureCap = 4` → threshold is **25% of TVL** (`totalAssets / exposureCap`).

Checked on every position write via `modifyTotalTally`. Liquidations and maturity settlement **bypass** this check.

**Vault net-long** (more shorts than longs — vault receives float):

$$
|\text{accruedFunding}| > \frac{\text{totalAssets}}{\text{exposureCap}} \;\Rightarrow\; \text{revert}
$$

**Vault net-short** (more longs than shorts — vault pays float):

$$
\text{impliedObligation} = |\text{netExposure}| \times \frac{\text{vAMM price}}{10^{18}}
$$

$$
\text{diff} = \text{vaultPnL} - \text{impliedObligation}
$$

Revert if `|diff| > totalAssets / exposureCap` **and** `diff < 0`.

| Protected                                          | Not protected                                      |
| -------------------------------------------------- | -------------------------------------------------- |
| New trade that would push vault exposure > 25% TVL | Existing exposure as vAMM price drifts             |
| Deposits/withdrawals that breach the cap           | Losses crystallised during liquidation or maturity |

Guard rails stop the **next** trade from worsening stress — they are not a P\&L floor.

***

## Phi (φ) — partial funding

When the vault cannot fully cover short's floating obligation during `settleFunding`:

$$
\phi = \min\!\left(1,\; \frac{\text{vaultHeadroom}}{\text{vaultFlow}}\right)
$$

Longs receive `φ × payment` instead of the full amount. The shortfall hits `vaultPnL`, reducing PPS (Price per share) for all LPs proportionally — a real-time haircut rather than a binary default.

***

## Price per share

$$
\text{PPS} = \frac{\text{vaultHeadroomLessAccrued}}{\text{totalShares}}
$$

`vaultHeadroomLessAccrued` simulates closing net vAMM exposure at current price, then adds the mark. PPS falls as `vaultPnL` goes negative.

If `vaultHeadroomLessAccrued = 0`, new deposits revert (`VaultInsolvent`). The vault is exit-only until maturity or PnL recovery.

***

## Vault cap

Per-market absolute TVL limit. New deposits revert with `VaultCapExceeded` when `currentTVL + deposit > vaultCap`.

***

## Maturity settlement

At maturity, `vaultPnL` crystallises into `totalAssets`:

$$
\text{totalAssets} \leftarrow
\begin{cases}
\text{totalAssets} + \text{vaultPnL} & \text{if vaultPnL} \geq 0 \\
\max(0,\; \text{totalAssets} - |\text{vaultPnL}|) & \text{if vaultPnL} < 0
\end{cases}
$$

`vaultPnL` is zeroed. LPs redeem at post-settlement PPS.
