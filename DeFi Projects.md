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

## Uniswap
### V1
* Factory (sets up an exchange system for a particular ERC20) and Exchange (can support any ERC20) smart contracts
* 2x+ cheaper gas price compared to other DEXs
* Supports ETH <-> ERC20 and ERC20 <-> ERC20
* Rewards (share of a 0.3% fee) are provided to users providing liquidity (e.g. ETH, DAI, etc.) -- i.e. to each "house"
* Adding liquidity to ERC20 requires the same amount of ETH to be deposited (such that 50% ETH, 50% ERC20)
* ERC20 <-> ERC20 requires 2 swaps (hence two fees)
* Uniswap tokens: ![Uniswap](/images/defi_uniswap.png)
* Constant product formula (x * y = k, based on quantity not price): ![Uniswap2](/images/defi_uniswap2.png)
* Relies on arbitrage trading (pertaining to constant product formula?) to keep exchange rates in check
### V2
* ERC20 <-> ERC20 possible without ETH bridging (hence saves on gas fees)
* Smart contracts: Factory, Router, Pair, Pair ERC20
* ![Uniswap3](/images/defi_uniswap3.png)
* ETH bridging still possible: ![Uniswap4](/images/defi_uniswap4.png)
* A more complicated routing possible (when direct swap not possible): ![Uniswap5](/images/defi_uniswap5.png)
* Price oracle: offered by Uniswap; uses TWAP (time weighted average price)
* Flash swapping (e.g. arbitrage trading such that guaranteed to make profit and pay back right away, settle a Maker Vault, etc.): ![Uniswap6](/images/defi_uniswap6.png)
* Possibility (not implemented right now) of 0.05% (as part of 0.3% fee) protocol fee for governance
* JavaScript SDK: e.g. Agent Wallet, Switcheo Exchange, Streamr Marketplace

## Curve
* StableSwap: previous name
* "Uniswap with leverage" for stablecoins (such that liquidity providers profit from fees)
* Also uses lending protocols (e.g. Compound, yEarn) for additional yield
* AMM: automated market maker
* ![Curve](/images/defi_curve.png)
* Can view (the absolute value of) dy / dx (i.e. slope of curve x * y = k) as price(!) of x in terms of y
* Amplification coefficient A (lower means closer to constant product invariant): e.g. A = 100 => comparable to Uniswap with 100x leverage?!
* Constant product invariant (x * y = k) vs constant price invariant (x + y = k)
* Has weird math (involving leverage variable χ and amplification coefficient A) to determine quantity/price/AMM curve
* Design philosophy of the curve: similar to constant price invariant if portfolio well-balanced (e.g. ∃ 100 DAI, 100 USDT, 100 USDC) vs constant product invariant if not well-balanced
* Price slippage: ![Curve2](/images/defi_curve2.png)
* Their simulation: ![Curve3](/images/defi_curve3.png)
* CRV (governance token a la COMP) and Curve DAO
* Ren, Synthetix, Balancer