Oracle price data
=====================

.. _oracle_price_data:

1. Is the spot price of a pool used?
--------------------------------------

  **Incorrect**
  
  Yes, spot price is used and it can be manipulated by flashloans
  
  **Correct**
  
  A decentralized oracle (`Chainlink, Band, UMA, etc. <https://www.coingecko.com/en/categories/oracle>`_), Uniswap v3 TWAP or similar
  
  **Explanation**
  
  Flashloan price manipulation has caused many attacks on protocols (`Explanation: samczsun price oracle advice <https://shouldiusespotpriceasmyoracle.com/>`_)
  
  **Links**
  
2. Are variable decimals for different token pair price feeds accounted for?
--------------------------------------------------------------------------------

  **Incorrect**
  
  No, a constant decimals value is hardcoded

  **Correct**
  
  Yes
  
  **Explanation**
  
  Incorrect decimals can lead to accounting errors (`Explanation: USD/AMPL Chainlink price feed has 18 decimals, not 8 decimals <https://etherscan.io/address/0xe20ca8d7546932360e37e9d72c1a47334af57706#readContract#F3>`_)
  
  **Links**
  
Chainlink Oracle
=====================

.. _chainlink_oracle:

1. Is a deprecated Chainlink function used, such as ``latestAnswer()``, ``latestRound()``, or ``getTimestamp()``?
--------------------------------------------------------------------------------------------------------------------

  **Incorrect**
  
  Yes
  
  **Correct**
  
  No

  **Explanation**
  
  Deprecated functions may not be supported in the future, which could cause a denial of service (`Explanation: Chainlink deprecated functions <https://docs.chain.link/data-feeds/api-reference>`_)
  
  **Links**
  
2. Is there proper validation of ``latestRoundData()``?
-------------------------------------------------------------

  **Incorrect**
  
  No
  
  **Correct**
  
  Yes, price is confirmed to be `in the range of minAnswer and maxAnswer limits <https://docs.chain.link/data-feeds#check-the-latest-answer-against-reasonable-limits>`_, the `timestamp of the latest answer is checked <https://docs.chain.link/data-feeds#check-the-timestamp-of-the-latest-answer>`_ against zero and a stale feed threshold that depends on the update frequency of the oracle for each specific token
  
  **Explanation**
  
  Insufficient validation of oracle can lead to the acceptance of bad data (`Explanation: Compound fork bad debt accumulation during Luna crash <https://rekt.news/venus-blizz-rekt/>`_)
  
  **Links**
  