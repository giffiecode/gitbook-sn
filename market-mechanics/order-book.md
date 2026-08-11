# Order Book

Orders on Rates Exchange Interest Rate Market are quoted and executed based on the **Implied APR**, representing the yield-derived price of the underlying rate exposure.

**Order Types**

* **Market Order** — Executes instantly at the best available implied rate, automatically routing to whichever venue—order book or vAMM—offers the most favorable price.
* **Limit Order** — Executes only at the trader’s chosen implied rate or a better one. Limit orders may also route to either venue depending on pricing conditions.

All limit orders remain active until filled or manually cancelled (Good-Til-Cancelled). When a market reaches maturity, its order book closes automatically and any remaining open orders are cancelled.
