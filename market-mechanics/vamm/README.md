---
description: >-
  Rates Exchange virtual AMM lets users swap variable interest-rate exposure for
  fixed-rate exposure. This section is a technical overview of how Rates
  Exchange vAMM facilitates fixed for float swaps.
---

# vAMM

## Core Design

<table><thead><tr><th width="341">Component</th><th>Role</th></tr></thead><tbody><tr><td>ParRateVAMMSingleton</td><td>AMM controller: swaps, liquidations, TWAP tracking</td></tr><tr><td>ParRateSwapVAMM (lib)</td><td>Constant-product math, time decay, APR ↔ price conversions</td></tr><tr><td>Manager</td><td>Position registry, accrual, and Vault coordination</td></tr><tr><td><strong>Vault</strong></td><td>Counterparty for net imbalances; LP PnL aggregation</td></tr><tr><td>TrustedAccumulator (Oracle)</td><td>Provides cumulative floating index (Ray) for accrual</td></tr></tbody></table>

The vAMM uses a constant-product invariant to represent a rate market:

$$
k \;=\; x \times y
$$

where:

* x is the **base** (fixed-leg notional / collateral),
* y is the **quote** (floating exposure),
* the **implied price** is

$$
p \;=\; \frac{x}{y}
$$

The fixed leg **decays** toward par as maturity approaches, while floating accrues via the oracle index.
