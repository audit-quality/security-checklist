Deploying on Layer 2 (Arbitrum, Optimism)
=========================================

.. _checklist:

Checklist
------------

1. If Chainlink is used, is there a check for sequencer uptime?
  a. Incorrect: No
  b. Correct: Sequencer uptime is verified
  c. Attack vector: `Old price data may be used <https://twitter.com/bytes032/status/1653943092427325448>`_ if the protocol is not aware that the sequencer is down (`Explanation: Apply Chainlink recommended code <https://docs.chain.link/data-feeds/l2-sequencer-feeds#example-code>`_)
2. Is block.number used anywhere?
  a. Incorrect: Yes
  b. Correct: No, ``block.timestamp`` is used instead of ``block.number``
  c. Attack vector: Block production is not constant (`Explanation: Use block.timestamp as recommended by Optimism docs <https://community.optimism.io/docs/developers/build/differences/#added-opcodes>`_  and `Arbitrum docs <https://developer.arbitrum.io/time#block-numbers-arbitrum-vs-ethereum>`_)
