Compound Finance v2 Forks
=======================

.. _checklist:

Checklist
------------

1. What is the process for adding new tokens?
  a. Incorrect: No checks in place to prevent the addition of a ERC777 token
  b. Correct: Validate that any token that will be added to the protocol has sufficient liquidity, a secure price oracle, is a standard ERC20 token without any callback hooks
  c. Attack vector: With a proper process to review new token markets that will be added (for example, `this OpenZeppelin checklist <https://github.com/OpenZeppelin/compound-assets-listing>`_), a token that risks a reentrancy attack or bad debt may be added (`Explanation: CREAM hack with AMP token <https://medium.com/cream-finance/c-r-e-a-m-finance-post-mortem-amp-exploit-6ceb20a630c5>`_)
2. What oracle is used for price data?
  a. Incorrect: spot price of a Uniswap pool or any price data that can be manipulated with a flashloan
  b. Correct: A decentralized oracle (`Chainlink, Band, UMA, etc. <https://www.coingecko.com/en/categories/oracle>`_), Uniswap v3 TWAP or similar
  c. Attack vector: oracle price manipulation changes the health of each borrowed position and can lead to bad debt, especially when combined with flashloans (`Explanation: Compound DAI liquidation <https://www.comp.xyz/t/dai-liquidation-event/642>`_)
3. What are the collateral factors and borrowing curves used for different tokens?
  a. Incorrect: much higher borrowing factor permitted compared to Compound (or Aave), much lower interest rate curve to Compound (or Aave)
  b. Correct: similar borrowing factor and interest rate curves to Compound (or Aave)
  c. Attack vector: The risk of bad debt is higher in adverse market conditions when there is a greater fraction of assets that is borrowed (`Explanation: Compound's regular risk modeling with Gauntlet <https://risk.gauntlet.network/protocols/compound>`_)
