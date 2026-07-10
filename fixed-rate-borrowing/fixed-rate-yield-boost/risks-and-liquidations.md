# Risks & Liquidations

Boosting does not touch your Aave deposit — but the short position backing your boost lives on Supernova and is collateralized by the margin you posted there. If the market's implied rate rises sharply against your short, the position can be **liquidated**, consuming the margin backing it. Liquidated positions will also no longer boost the underlying Aave lending. For how position health, margin thresholds, and liquidations are calculated for shorts, see [Position Health](../../market-mechanics/position-health.md) and [Liquidation](../../market-mechanics/liquidation.md).

## Max rate coverage

To protect against liquidations, you can choose to deposit margin up to the **max rate coverage** when you open or manage a boost. Depositing this amount protects against liquidations as long as the Supernova implied rate and the underlying borrow rate remain at or below the max borrow rate on Aave.
