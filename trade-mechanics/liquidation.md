# Liquidation

Anyone can liquidate an underwater short via `liquidate(mktId, user, percent)`.

## Standard flow

1. **Cancel orders** — locked collateral in resting LOB orders is released.
2. **Accrue funding** — crystallises floating payments into `base` (may trigger bad debt).
3. **Verify underwater** — reverts if position is actually solvent (anti-griefing).
4. **Close via vAMM** — short notional is bought back using the user's collateral.
5. **Penalty** — 5% of collateral consumed, split 90% liquidator / 10% protocol.

Partial closes use `percent` in `[10%, 100%]` (or `0` for full close). Penalty and bad-debt logic scale proportionally.

---

## Where the money goes

| Amount | Recipient | How |
|---|---|---|
| Collateral used to close vAMM position | **Vault** | `vault.accrueBase(baseConsumed)` |
| Uncoverable shortfall | **Vault** (loss) | `vault.accrueBadDebt(shortfall)` |
| 5% penalty × 90% | **Liquidator** | Clearing-house credit |
| 5% penalty × 10% | **Protocol** | Manager clearing-house balance |

**Net to vault:** `baseConsumed − badDebt`. Healthy liquidations are positive; bad-debt events absorb the gap.

If collateral is insufficient to close, clearing-house credit is used first; any residual shortfall becomes vault bad debt.

---

## Foreclosure (alternative)

Foreclosure bypasses the vAMM — no price impact, no cascade risk.

```
new_caller.base  = caller.base + capitalInfusion + user.base
new_caller.quote = caller.quote + user.quote
user wiped       → base = 0, quote = 0
```

The caller injects fresh capital from their clearing-house balance and absorbs the position. The **combined** book must pass the strict 67.5% safe check or the transaction reverts.

Governance can toggle foreclosure via `setIsForeclosureAllowed`. Prefer it in volatile, low-liquidity conditions where a vAMM close would move price sharply.
