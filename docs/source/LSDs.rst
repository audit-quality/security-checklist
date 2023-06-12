LSDs (Liquid Staking Derivatives)
==================================

LSDs have become a popular way of earning interest on Proof of Stake (PoS) native token staking without locking up the staked tokens. A LSD protocol issues a new liquid token in return for staking the native tokens, which allows users to use their new token in other dApps.

Other resources:
* `LSD checklist <https://blog.decurity.io/typical-vulnerabilities-in-lsd-protocols-e52ffe4ee175>`_

1. Are withdraw credentials securely handled to prevent a malicious node operator from frontrunning withdrawals with their own credentials?
-----------------------------------------------------------------------------------------------------------------------------------------------

  **Incorrect**

  No

  **Correct**

  Yes

  **Explanation**

  TODO

  **Links**

  * `Lido and Rocketpool frontrunning bounty writeup <https://medium.com/immunefi/rocketpool-lido-frontrunning-bug-fix-postmortem-e701f26d7971>`_
  * `Tranchess frontrunning bounty writeup <https://tranchess.medium.com/recap-deposit-front-run-vulnerability-mitigation-cfc66ef8c50d>`_