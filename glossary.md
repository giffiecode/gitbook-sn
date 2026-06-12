# Glossary

Each Interest Rate Market is defined by two key parameters: the _**Underlying Market**_ and the _**Expiry**_. Each market trades its _**implied rate**_, which is used to price movements in the underlying rate. While trading occurs on the implied rate, _**continuous cash settlement**_ is based on the actual _**underlying rate.**_

**Underlying Market**\
The base lending market that determines the floating rate benchmark used for the swap. Examples include specific Aave or Morpho markets (e.g., Aave USDC, Morpho USDC/cbBTC).

**Expiry**

The maturity date of the Interest Rate Market. It specifies the settlement point when all open rate positions in that market are closed and PnL is realized.<br>

**Debt Unit (DU)**\
Each DU represents the borrow rate on **1 unit of notional debt** in the underlying money market. For example, in an Aave USDC market, each DU represents the variable borrow rate accrued on **1 USDC of debt**.

\
**Notional Size**

The reference amount that defines the rate exposure of a position. It represents how much of the underlying market’s rate the user is long or short. Notional Size is measured in _**Debt Units (DU)**_, where **1 DU** equals the rate exposure of 1 unit of notional debt in the underlying market.\
\
\
**Implied APR (Fixed APR)**

The market-derived fixed rate representing the consensus expectation of the average future rate for the selected underlying market and tenor. When a position is opened, this becomes the fixed rate paid or received, depending on long or short exposure.

**Underlying Rate (Floating APR)**\
The current variable rate of the underlying market (e.g., Aave or Morpho), reflecting real-time utilization conditions from the Underlying Market.<br>

**Long Implied APR**\
A position that fixes the borrowing rate by paying the implied APR **up-front** in exchange for receiving the floating underlying rate over time. The fixed-rate liability is settled at entry.<br>

**Short Implied APR**\
A position that receives the implied APR and pays the floating underlying rate over time. This exposure requires **margin** to cover mark-to-market fluctuations and ongoing settlement.\
\
**Margin**\
Capital posted to open and maintain a Short Implied APR rate position. Margin absorbs mark-to-market PnL and ongoing settlement flows, ensuring the position remains solvent while supporting the selected notional exposure.

**Settlement**

The continuous transfer of floating-rate payments to the long (pay-fixed) side based upon the underlying rate. It reflects how the floating rate accrues over time and is credited to the long’s account, representing the realized return from the underlying market relative to the fixed rate paid upfront.

**My Fixed Rate**\
The weighted-average fixed rate of the user’s open position(s), determined by the implied rate(s) at the time of entry.

&#x20;
