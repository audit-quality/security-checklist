Deploying on Layer 2 (Arbitrum, Optimism, etc.)
================================================

Protocols that will be deployed on multiple chains may require custom modifications for each individual chain because each chain is slightly different in key ways. One common multichain deployment option is to deploy on mainnet Ethereum and an L2 like Arbitrum or Optimism.

Other resources:
* `EVM Diff, differences between EVM compatible chains <https://www.evmdiff.com/>`_

1. If Chainlink is used, is there a check for sequencer uptime?
------------------------------------------------------------------

  **Incorrect**

  No

  **Correct**
  
  Sequencer uptime is verified
  
  **Explanation**
  
  `Old price data may be used <https://twitter.com/bytes032/status/1653943092427325448>`_ if the protocol is not aware that the sequencer is down
  
  **Links**
  
  * `Chainlink recommended sequencer check code <https://docs.chain.link/data-feeds/l2-sequencer-feeds#example-code>`_

2. Is block.number used anywhere?
------------------------------------

  **Incorrect**
  
  Yes
  
  **Correct**
  
  No, ``block.timestamp`` is used instead of ``block.number``
  
  **Explanation**
  
  Block production is not constant on L2 chains, meaning ``block.timestamp`` is a better choice than ``block.number`` in most cases.

  **Links**

  * `block.timestamp recommended by Optimism docs <https://community.optimism.io/docs/developers/build/differences/#added-opcodes>`_
  * `block.timestamp recommended by Arbitrum docs <https://developer.arbitrum.io/time#block-numbers-arbitrum-vs-ethereum>`_