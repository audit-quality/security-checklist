Compound Finance v2 Forks
==========================

1. What is the process for adding new tokens?
-----------------------------------------------
  
  **Incorrect**
  
  No checks in place to prevent the addition of a ERC777 token
  
  **Correct**
  
  Validate that any token that will be added to the protocol has sufficient liquidity, a secure price oracle, is a standard ERC20 token without any callback hooks
  
  **Explanation**
  
  With a proper process to review new token markets that will be added (for example, `this OpenZeppelin checklist <https://github.com/OpenZeppelin/compound-assets-listing>`_), a token that risks a reentrancy attack or bad debt may be added (`Explanation: CREAM hack with AMP token <https://medium.com/cream-finance/c-r-e-a-m-finance-post-mortem-amp-exploit-6ceb20a630c5>`_)
  
  **Links**
  

2. What oracle is used for price data?
-----------------------------------------------
  
  **Incorrect**
  
  Spot price of a Uniswap pool or any price data that can be manipulated with a flashloan

  **Correct**
  
  A decentralized oracle (`Chainlink, Band, UMA, etc. <https://www.coingecko.com/en/categories/oracle>`_), Uniswap v3 TWAP or similar
  
  **Explanation**
  
  Oracle price manipulation changes the health of each borrowed position and can lead to bad debt, especially when combined with flashloans (`Explanation: Compound DAI liquidation <https://www.comp.xyz/t/dai-liquidation-event/642>`_)
  
  **Links**
  

3. What are the collateral factors and borrowing curves used for different tokens?
---------------------------------------------------------------------------------------

  **Incorrect**
  
  Much higher borrowing factor permitted compared to Compound (or Aave), much lower interest rate curve to Compound (or Aave)

  **Correct**
  
  Similar borrowing factor and interest rate curves to Compound (or Aave)

  **Explanation**
  
  The risk of bad debt is higher in adverse market conditions when there is a greater fraction of assets that is borrowed (`Explanation: Compound's regular risk modeling with Gauntlet <https://risk.gauntlet.network/protocols/compound>`_)
  
  **Links**
  
