# Compound Finance v2 Forks

Compound Finance is a popular lending protocol that has been forked many times. Despite this popularity, there have been many hacks of Compound Forks due to common pitfalls.

Other resources:

- [List of Compound Finance fork hacks](https://github.com/YAcademy-Residents/defi-fork-bugs#compound)

## 1. What is the process for adding new tokens?

**Incorrect**

No checks in place to prevent the addition of a ERC777 token

**Correct**

Validate that any token that will be added to the protocol has sufficient liquidity, a secure price oracle, is a standard ERC20 token without any callback hooks

**Explanation**

The Compound Finance codebase is [vulnerable to reentrancy](https://www.comp.xyz/t/reentrancy-protection-currently-broken/2573). This means that without a process to review new token markets (for example, [this OpenZeppelin checklist](https://github.com/OpenZeppelin/compound-assets-listing)), a token that risks a reentrancy attack or bad debt may be added

**Links**

- [CREAM hack with AMP token](https://medium.com/cream-finance/c-r-e-a-m-finance-post-mortem-amp-exploit-6ceb20a630c5)
- [OpenZeppelin asset listing checklist](https://github.com/OpenZeppelin/compound-assets-listing)

## 2. What oracle is used for price data?

**Incorrect**

Spot price of a Uniswap pool or any price data that can be manipulated with a flashloan

**Correct**

A decentralized oracle ([Chainlink, Band, UMA, etc.](https://www.coingecko.com/en/categories/oracle)), Uniswap v3 TWAP or similar

**Explanation**

Oracle price manipulation changes the health of each borrowed position and can lead to bad debt, especially when combined with flashloans

**Links**

- [Compound DAI liquidation](https://www.comp.xyz/t/dai-liquidation-event/642)

## 3. What are the collateral factors and borrowing curves used for different tokens?

**Incorrect**

Much higher borrowing factor permitted compared to Compound (or Aave), much lower interest rate curve to Compound (or Aave)

**Correct**

Similar borrowing factor and interest rate curves to Compound (or Aave)

**Explanation**

The risk of bad debt is higher in adverse market conditions when there is a greater fraction of assets that is borrowed

**Links**

- [Compound's regular risk modelling with Gauntlet](https://risk.gauntlet.network/protocols/compound)

## 4. What mitigations exist to mitigate the risk of a toxic liquidation spiral?

**Incorrect**

None, the protocol did not consider this edge case in detail

**Correct**

Many options exist with varying degrees of impact:
- Apply a dynamic incentive or closing factor during the liquidation process, which reduces the reward for liquidators as the loan nears the toxic liquidation threshold
- Allow markets to be quickly (or automatically) frozen in certain market conditions, in line with [this research](https://arxiv.org/abs/2212.07306)
- Choosing borrow caps and and collateral factors on assets strategically to minimize the risk of bad debt for each asset depending on current market liquidity and recent volatility

**Explanation**

Any DeFi protocol that allows borrowing must enforce liquidations to avoid the accumulation of bad debt. The most common incentive design is to give the liquidator a fraction of the liquidated amount as a reward. If there is a fast enough price movement, the reward amount given to the liquidator could create bad debt in the protocol, which can result in a liquidation spiral. This spiral is a result of one liquidation causing the borrower position to become less healthy (higher ratio of borrowed assets to collateral) after a liquidation happens, triggering more liquidations.

**Links**

- [CRV bad debt in Aave](https://governance.aave.com/t/arc-repay-excess-debt-in-crv-market-for-aave-v2-eth/10779)
- [OpenZeppelin 2019 finding about "Counterproductive Incentives"](https://blog.openzeppelin.com/compound-audit)
- [Toxic liquidations whitepaper](https://arxiv.org/abs/2212.07306)
- [OVIX hack](https://0vixprotocol.medium.com/0vix-exploit-post-mortem-15c882dcf479)
