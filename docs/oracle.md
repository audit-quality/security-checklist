# Oracle price data

Oracles are a common choice to receive price data for assets. The price data from oracles can be harder to manipulate than price data from a DeFi protocol's swap pool, but there are common oracle integration issues that should be kept in mind.

Other resources:

- [Market Manipulation vs. Oracle Exploits](https://chain.link/education-hub/market-manipulation-vs-oracle-exploits)

## 1. Is the spot price of a pool used?

**Incorrect**

Yes, spot price is used and it can be manipulated by flashloans

**Correct**

A decentralized oracle ([Chainlink, Band, UMA, etc.](https://www.coingecko.com/en/categories/oracle)), Uniswap v3 TWAP or similar

**Explanation**

Flashloan price manipulation has caused many protocol hacks, making price manipulation hacks one of the most common attack vectors

**Links**

- [samczsun's oracle price guide](https://shouldiusespotpriceasmyoracle.com/)

## 2. Are variable decimals for different token pair price feeds accounted for?

**Incorrect**

No, a constant decimals value is hardcoded

**Correct**

Yes

**Explanation**

Incorrect decimals can lead to accounting errors

**Links**

- [USD/AMPL Chainlink price feed has 18 decimals, not 8 decimals](https://etherscan.io/address/0xe20ca8d7546932360e37e9d72c1a47334af57706#readContract#F3)

## 3. If Uniswap v3 TWAP is used for price data, is post-merge PoS manipulation accounted for?

**Incorrect**

No, TWAP is fully trusted

**Correct**

Yes

**Explanation**

Incorrect decimals can lead to accounting errors

**Links**

- [Euler oracle manipulation tool](https://oracle.euler.finance/) and [usage instructions](https://medium.com/eulerfinance/uniswap-oracle-attack-simulator-42d18adf65af)
- [Uniswap analysis of TWAP manipulation in PoS](https://blog.uniswap.org/uniswap-v3-oracles)

# Chainlink Oracle

## 1. Is a deprecated Chainlink function used, such as `latestAnswer()`, `latestRound()`, or `getTimestamp()`?

**Incorrect**

Yes

**Correct**

No

**Explanation**

Deprecated functions may not be supported in the future, which could cause a denial of service

**Links**

- [Chainlink deprecated functions](https://docs.chain.link/data-feeds/api-reference)

## 2. Is there proper validation of `latestRoundData()`?

**Incorrect**

No

**Correct**

Yes, price is confirmed to be [in the range of minAnswer and maxAnswer limits](https://docs.chain.link/data-feeds#check-the-latest-answer-against-reasonable-limits), the [timestamp of the latest answer is checked](https://docs.chain.link/data-feeds#check-the-timestamp-of-the-latest-answer) against zero and a stale feed threshold that depends on the update frequency of the oracle for each specific token

**Explanation**

Insufficient validation of oracle can lead to the acceptance of bad data

**Links**

- [Compound fork bad debt accumulation during Luna crash](https://rekt.news/venus-blizz-rekt/)

## 3. Is the price query in a try/catch?

**Incorrect**

No

**Correct**

Yes, `latestRoundData` call is in a try/catch block

**Explanation**

Access to price feed data may be removed due to the multisig ownership of Chainlink's EACAggregatorProxy contract which is queried for price data. If uptime is a key part of a protocol's design and the Chainlink multisig is not considered a trusted entity, a backup mode of operation should exist in the `catch` block of the try/catch price query to handle the edge case where the primary Chainlink price feed is not available.

**Links**

- ["ChainLink Price Feeds" section of OpenZeppelin post](https://blog.openzeppelin.com/secure-smart-contract-guidelines-the-dangers-of-price-oracles)