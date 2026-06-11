# Key Parameters

Quick reference for liquidation, vault safety, and market bootstrap settings.

| Parameter | Location | Example | Effect |
|---|---|---|---|
| `ltvForMarket` | Manager | 75,000 (75%) | LTV at which a short is liquidatable |
| `liquidationPenaltyE5` | Manager | 5,000 (5%) | Penalty on collateral used in liquidation |
| `liquidatorShareE5` | Manager | 90,000 (90%) | Liquidator's cut of the penalty |
| `exposureCap` | Vault | 4 | Guard-rail threshold = 1/cap of TVL (25%) |
| `vaultCap` | Vault | per market | Max TVL per market |
| `isForeclosureAllowed` | Manager | `true` | Enable peer-takeover liquidations |
| `defaultFundingFeeBps` | Vault | set at deploy | Annual fee credited to vault accrual |
