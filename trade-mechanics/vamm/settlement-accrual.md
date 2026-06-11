# Settlement Accrual

#### Settlement Accrual <a href="#funding-accrual" id="funding-accrual"></a>

Long Positions continuously accrue Settlement based upon oracle's cumulative growth. At a high level:

* Let `growthRay` be the oracle growth (Ray).
* Convert to notional : $$\text{growth} = \text{scaleRayToNotional}(\text{growthRay})$$&#x20;
* Funding for a position of size Q (quote) is:&#x20;

$$
\text{funding} \;=\; \text{growth} \times |Q|
$$

Sign:

* $$Q > 0$$ (long float) -> **accrue** funding to base&#x20;
* $$Q < 0$$ (short float) -> **deduct** funding from base &#x20;
