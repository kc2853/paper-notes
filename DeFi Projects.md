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

## Balancer
* You collect fees from traders (who rebalance your portfolio per arbitrage opportunities) instead of paying fees to portfolio managers to rebalance
* Equations (value function V, spot price SP, effective price EP): ![Balancer](/images/defi_balancer.png)
* Can think of n tokens' (in the pool, token < pool) value all in terms of (say) token t => each token's normalized weight (i.e. the exponent in the generalized constant product formula) = share in the pool
* The constant product invariant will actually increase in practice b/c trading fees?!
* Trading formulas: out given in (how many will I get if I put in 100?), in given out (how many do I need to put in to get 100?), in given price (how many do I need to put in to change the price from 8 to 12?)
* Pool tokens(?): &lt;Pools can aggregate the liquidity provided by several different users. In order for them to be able to freely deposit and withdraw assets from the pool, Balancer Protocol has the concept of pool tokens. Pool tokens represent ownership of the assets contained in the pool. The outstanding supply of pool tokens is directly proportional to the Value Function of the pool. If a deposit of assets increases the pool Value Function by 10%, then the outstanding supply of pool tokens also increases by 10%. This happens because the depositor is issued 10% of new pool tokens in return for the deposit.&gt;
* 3 planned releases: Bronze, Silver, Gold
* Controlled pools (only specific address can add/remove liquidity to the pool) vs finalized pools (fixed configurations, trustless)
* Exists 1000+ pools!
* Swap (traders exchanging tokens; all go to liquidity providers) and exit (liquidity providers withdrawing; most go to liquidity providers but some go to Balancer Labs) fees
* BAL: governance token; liquidity mining such that distributed to LPs (liquidity providers)

## Rari Capital
* Mint RFT when lock in funds -> burn them to retrieve funds (plus yield) back
* Charges 20% performance fee
* Mentions Ken Deeter's guarded launch concept: $350 user deposit limit; limited asset types of DAI, USDC, USDT; 3-out-of-5 multisig for smart contract upgrades; rebalancer remains centralized however
* DAI, USDC to dYdX; DAI, USDC, USDT to Compound
* Deposits (same when withdrawing) exchanged (to stablecoins?) via 0x
* Will be implementing smart-contract-based slippage limit?

## UMA
* Basic idea (Hart Lambur's tweets): https://archive.vn/U5iXS
* TRS: total return swap (e.g. betting on gold's price without gold)
* Cool examples: investing in US stocks from the developing world, decentralized life insurance, shorting altcoins, leverage trading
* UMA contains 5 core components: public addresses of maker and taker; margin subaccounts; logic to calculate the economic terms of the agreement; choice of oracle; contract functions to add/withdraw margin, remargin, settle, etc.
* Example (taker, not maker, is gold buyer): ![UMA](/images/defi_uma.png)
* The function remargin(): transfers amount from maker's margin account to taker's (if reference asset's price goes up); can happen in short time intervals (e.g. every day); can be exploited by anyone?! => incentivizes taker/maker to run it as much as possible (b/c it's ideal to remargin continuously even if it's hard on ETH)
* Keeper: third party margin custodian who can always monitor and remargin for a fee
* Taker (not just maker) can be on margin!
* Whitepaper has detailed examples of UMA contract life cycle with/without default
* Can be configured such that ETH is converted to USD per remargin transfer (!= beginning/final price of ETH)?!
* Trustless tokenization: Alice (taker) can remove leverage from her position -> put in $10 mil worth of ETH to "purchase" $10 mil worth of gold -> mint ERC20 tokens correspondingly -> new market
* Future work: trade negotiation protocol (automating the process of matchmaking taker with best maker); margin netting (offsetting margin based on multiple positions)
* Exists a separate whitepaper just for oracle
* Priceless token: has 2 states (collateralized, undercollateralized); liquidator (using off-chain oracle) vs disputor (can dispute and win reward if liquidator is wrong)
* ETHBTC: first priceless synthetic asset
* Can use ETH or DAI as collateral rather than SNX
* DVM (Data Verification Mechanism): oracle service provided by UMA; does not provide an on-chain price feed; only called to resolve liquidation disputes
* Token sponsor: initial taker who deposits collateral into smart contract and withdraws synthetic tokens to sell to token holders (liquidator/disputor)
* Basically: token sponsor vs token holder (liquidator vs disputor) -> DVM only called when liquidator disputed (a la court system)
* ![UMA2](/images/defi_uma2.png)