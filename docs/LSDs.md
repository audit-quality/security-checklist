# LSDs (Liquid Staking Derivatives)

LSDs have become a popular way of earning interest on Proof of Stake (PoS) native token staking without locking up the staked tokens. A LSD protocol issues a new liquid token in return for staking the native tokens, which allows users to use their new token in other dApps.

Other resources:

- [LSD checklist](https://blog.decurity.io/typical-vulnerabilities-in-lsd-protocols-e52ffe4ee175)

## 1. Are withdraw credentials securely handled to prevent a malicious node operator from frontrunning withdrawals with their own credentials?

**Incorrect**

No

**Correct**

Yes

**Explanation**

The process of setting `withdrawal_credentials`, a variable used in the beacon chain withdrawal process, is a key part in determining who receives the ETH upon withdrawal. The `withdrawal_credentials` value must match the key of a depositor. But if a malicious party frontruns a valid deposit from the staking pool with their own deposit of 32 ETH, the withdraw request can include the malicious party's own `withdrawal_credentials` and the malicious party will receive the ETH from the staking pool. This is made possible by the fact that the beacon chain deposit and withdrawal process allows multiple `withdrawal_credentials` based on the addresses of depositors.

**Links**

- [Lido and Rocketpool frontrunning bounty writeup](https://medium.com/immunefi/rocketpool-lido-frontrunning-bug-fix-postmortem-e701f26d7971)
- [Tranchess frontrunning bounty writeup](https://tranchess.medium.com/recap-deposit-front-run-vulnerability-mitigation-cfc66ef8c50d)