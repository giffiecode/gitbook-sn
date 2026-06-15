# SLP Vault

#### Core Functions

\
The Vault acts as the protocol’s counterparty for each market. It absorbs net imbalances between longs and shorts and captures protocol fees to incentivize its depositors.

1.  **Deposit or Withdraw liquidity at any time**

    Deposits mint Vault shares that represent proportional ownership of the Vault’s assets and earnings, while withdrawals burn shares to redeem the underlying value. This design enables flexible participation and continuous capital availability without lockups.
2. **Counterparty Management**\
   The Vault passively takes the opposite side of net open interest. When one side of the market is larger, the Vault provides offsetting exposure, ensuring continuous price discovery and execution without requiring a perfectly balanced order book.
3. **Fee Accrual**\
   Trading and settlement fees flow directly to the Vault. In the early phase, 100% of these fees are redirected to incentivize LPs and bootstrap Vault liquidity. As the protocol matures, a portion may be routed to protocol treasury or insurance reserves.

#### Vault LP Payout

Liquidity Providers earn returns from the Vault based on market activity and funding dynamics. LP yield consists of:

1. **Fee Income** — LPs receive a proportional share of all trading and settlement fees generated in their market. Currently, 100% fees are directed to the vault.
2. **Seeding Premium** — In markets expecting stronger natural long-rate demand, the Vault offers an additional yield premium, initially set at 100 bps, to incentivize early liquidity. A 100 bps fixed rate premium a 5% floating rate equates to approximately 20% APR on LP equity if rates remain stable.
3. **Position PnL** — The Vault’s net position PnL contributes directly to LP returns. When the Vault’s aggregate exposure earns positive carry against traders, LPs gain; when traders or excess market liquidity outperform, LP returns can be negative.

LP payouts are periodically realized in the Vault’s base asset, reflecting each LP’s share of total Vault liquidity after accounting for accrued PnL, fees, and funding.
