# AMM (Automated Market Maker)

AMMs allow for swapping between different tokens. Different AMMs have different designs and can involve different risks during integration.

Other resources:

- None currently

## 1. Is there a user defined slippage value when a swap happens?

**Incorrect**

No slippage protection during swaps, or the slippage amount is not user defined. Even if a slippage check exists, the implementation should be examined carefully.

**Correct**

The slippage amount is user defined, similar to the `amountOutMin` argument in [the swapExactTokensForTokens function of UniswapV2Router02.sol](https://github.com/Uniswap/v2-periphery/blob/0335e8f7e1bd1e8d8329fd300aea2ef2f36dd19f/contracts/UniswapV2Router02.sol#LL226C14-L226C26).

**Explanation**

Slippage is a side-effect of how AMMs operate, meaning slippage is always present in a swap. If there is no slippage protection and the swap can proceed at any price, the swap can be sandwiched or arbitraged by MEV bots, resulting in the user receiving fewer tokens than expected. If there is slippage protection that is hardcoded by the protocol, the swap may revert when the market volatility is high or liquidity is low, resulting in a temporary denial of service for the user who is unable to perform the swap.

**Links**

- Code4rena has many findings [related to missing or inadequate slippage settings](https://github.com/search?q=org%3Acode-423n4+slippage&type=issues), including [this one](https://github.com/code-423n4/2022-01-notional-findings/issues/181) and [this one](https://github.com/code-423n4/2022-06-badger-findings/issues/155).

## 2. If a single-sided deposit into a multi-token pool is allowed, does the protocol calculate the token swap amount properly?

**Incorrect**

No, the protocol naively swaps half the tokens received from the user and then deposits the tokens into the same pool that was used for the swapping process.

**Correct**

Yes, the protocol performs the proper math to prevent leftover dust or returns any leftover dust to the user after the deposit process.

**Explanation**

If a protocol takes the naive approach of swapping half of the token, this first swap will change the pool balances and exchange rate between the tokens in the pool. It is important to account for the impact of this swap when calculating the amount of tokens that should be swapped for a dust-free deposit into the liquidity pool. The precise math to allow a one-sided deposit into a Uniswap-like pool is [detailed by Alpha Finance](https://blog.alphaventuredao.io/onesideduniswap/).

**Links**

- [Alpha Finance single token deposit derivation](https://blog.alphaventuredao.io/onesideduniswap/)
- [One-sided swap finding](https://reports.yaudit.dev/reports/05-2022-OpenMEVRouter/#1-high---the-swap-and-stake-mechanisms-in-openmevzapper-leave-funds-in-the-contract-jackson)