# DeFi Projects

## Compound
* cToken: represents assets supplied; equals increasing quantity of underlying assets over time; earning interest is as simple as holding cToken
* Dapps and exchanges can use Compound as well
* No terms to negotiate, maturity dates, or funding periods
* Collateral factor (from 0 to 1): portion of underlying asset value that can be borrowed (e.g. liquid, high-cap has high collateral factor)
* cToken as collateral
* Borrowing capacity = account's underlying token balance * collateral factor
* Liquidation discount: can redeem collateral at a cheaper price when borrowing outstanding > borrowing capacity (can happen when collateral's value falls or borrowed asset's value increases)
* Close factor (from 0 to 1): proportion eligible to be closed (related to liquidation discount?)
* Allows ability to seamlessly hold new assets without(!) selling or rearranging a portfolio (e.g. purchase computing power on Golem)
* How does funding different "houses" work?
* Utilization ratio U = Borrows / (Cash + Borrows)
* Example: Borrowing Interest Rate = 2.5% + U * 20%
* Implicitly: Supplying Interest Rate = Borrowing Interest Rate * U
* Liquidity incentive structure: extreme demand for an asset -> liquidity (supply?) will decline -> interest rate rises -> incentivizes supply while disincentivizing demand
* Price between cToken and underlying asset: ![Compound](/images/defi_compound.png)
* ![Compound2](/images/defi_compound2.png)
* Interest Rate Index: captures history of each interest rate
* ![Compound3](/images/defi_compound3.png)
* Price feed: committee(?) that pools from top 10 exchanges
* Markets must be whitelisted (via admin function)
* Comptroller: each function call is validated via (centralized?) policy layer
* ![Compound4](/images/defi_compound4.png)