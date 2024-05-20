# Deploying on other EVM-compatible (Arbitrum, Optimism, Gnosis, etc.)

Protocols that will be deployed on multiple chains may require custom modifications for each individual chain because each chain is slightly different in key ways. One common multichain deployment option is to deploy on mainnet Ethereum and an L2 like Arbitrum or Optimism.

Other resources:

- [EVM Diff, differences between EVM compatible chains](https://www.evmdiff.com/)

## 1. If Chainlink is used, is there a check for sequencer uptime?

**Incorrect**

No

**Correct**

Sequencer uptime is verified

**Explanation**

[Old price data may be used](https://twitter.com/bytes032/status/1653943092427325448) if the protocol is not aware that the sequencer is down.

**Links**

- [Chainlink recommended sequencer check code](https://docs.chain.link/data-feeds/l2-sequencer-feeds#example-code)

## 2. Is block.number used anywhere?

**Incorrect**

Yes, block.number is used on Optimism, Arbitrum, or a similar L2

**Correct**

No, `block.timestamp` is used instead of `block.number`

**Explanation**

Block production is not constant on L2 chains, meaning `block.timestamp` is a better choice than `block.number` in most cases.

**Links**

- [block.timestamp recommended by Optimism docs](https://community.optimism.io/docs/developers/build/differences/#added-opcodes)
- [block.timestamp recommended by Arbitrum docs](https://developer.arbitrum.io/time#block-numbers-arbitrum-vs-ethereum)

## 3. Is CREATE2 used in contracts deployed to zkEVMs (like zkSync Era)?

**Incorrect**

Yes

**Correct**

No

**Explanation**

zkEVMs can compute CREATE2 addresses differently than on ETH mainnet. Code copied from ETH mainnet to calculate the address of the contract deployed with CREATE2 may not work as expected.

**Links**

- [Tweet from pcaversaccio describing the difference](https://twitter.com/pcaversaccio/status/1701938364696363103)

## 4. If deployed on Gnosis chain (xDAI), are reentrant tokens considered?

**Incorrect**

No

**Correct**

Yes

**Explanation**

Gnosis Chain cross-chain tokens have a `onTokenTransfer()` hook that enabled reentrancy. The Gnosis bridge creates a ERC677 tokens on Gnosis Chain for these tokens, which means protocols like Compound Finance cannot be used on xDAI safely unless modified to protect against reentrancy.

**Links**

- [Gnosis bridge details](https://docs.gnosischain.com/bridges/tutorials/using-omnibridge/)
- [Hundred Finance and Agave Finance hacks](https://medium.com/immunefi/a-poc-of-the-hundred-finance-heist-4121f23a098)

## 5. Do any token addresses consider if a token is bridged or native?

**Incorrect**

A difference between bridged or native tokens was not considered.

**Correct**

Yes, the different types of tokens were considered and the correct choice is made.

**Explanation**

Some tokens, like USDC, have a bridged and native form. For example, USDC.e on Arbitrum, Avalanche, and Optimism is the bridged form of USDC. Circle launched a native USDC on these chains after the bridged tokens already existed. If the protocol involves any tokens that may be bridged, it is important to consider which token address to use.

**Links**

- [Circle announcement of Optimism native USDC](https://www.circle.com/blog/now-available-usdc-on-op-mainnet)
- [Circle announcement of Arbitrum native USDC](https://www.circle.com/blog/arbitrum-usdc-now-available)

## 6. If deployed on Blast chain, is it claiming fees?

**Incorrect**

No.

**Correct**

Yes, deployed contracts are configured to receive and claim gas from Blast.

**Explanation**

Blast redirects sequencer fees to the DApps that induced them, allowing smart contract developers to have an additional source of revenue. The contracts have to be configured to receive and claim gas fees.

**Links**

- [Blas docs for receiving and claiming fees](https://docs.blast.io/building/guides/gas-fees#overview)
