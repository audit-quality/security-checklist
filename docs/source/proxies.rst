Proxies and Upgradeable Code
=================================

Proxies are a common tool to allow for smart contract code to receive upgrades. A proxy contract acts as a pointer to a logic contract using delegatecall, and the logic contract contains the implementation logic. If the proxy contract is modified to point to a different logic contract address, the implementation logic is changed.

Other resources:
* `OpenZeppelin Proxy Pattern <https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies>`_
* `yAcademy Proxies Research <https://proxies.yacademy.dev/>`_

1. Is the UUPS proxy initialized?
-----------------------------------

  **Incorrect**

  No

  **Correct**

  Yes

  **Explanation**

  An uninitialized proxy can be initialized by anyone. The user that calls the initialize function is granted special privileges for the proxy contract.

  **Links**

  * `Wormhole, largest bug bounty <https://medium.com/immunefi/wormhole-uninitialized-proxy-bugfix-review-90250c41a43a>`_
  * `Aave v2 <https://medium.com/aave/aave-security-newsletter-546bf964689d>`_
  * `Agave v2 (Aave fork) <https://medium.com/@hacxyk/forked-protocols-are-not-battle-tested-agave-uninitialized-proxy-vulnerability-6b5d587b3a07>`_
  * `Teller <https://medium.com/immunefi/teller-bug-fix-postmorten-and-bug-bounty-launch-b3f67a65c5ac>`_
  * `Harvest Finance <https://medium.com/immunefi/harvest-finance-uninitialized-proxies-bug-fix-postmortem-ea5c0f7af96b>`_

2. Does the logic contract behind the proxy contain selfdestruct?
--------------------------------------------------------------------------------------------------------------------

  **Incorrect**

  Yes

  **Correct**

  No, `selfdestruct` is not found in the contracts or the contract is not a logic contract behind a proxy contract

  **Explanation**

  If the logic contract behind a proxy contains `selfdestruct`, the proxy can be deleted, which can cause a denial of service. If a metamorphic contract created with CREATE2 contains `selfdestruct`, then the contract code can be replaced in a similar way to upgrading code behind a proxy.

  **Links**

  * `Parity wallet hack <https://www.parity.io/blog/a-postmortem-on-the-parity-multi-sig-library-self-destruct/>`_ and the related `OpenZeppelin writeup <https://blog.openzeppelin.com/on-the-parity-wallet-multisig-hack-405a8c12e8f7>`_
  * `Ethernaut Level 25 “Motorbike” <https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/contracts/levels/Motorbike.sol>`_
  * `Paradigm CTF 2021 “Vault” <https://github.com/paradigmxyz/paradigm-ctf-2021/tree/master/vault>`_

3. Is the contract (not necessarily a proxy contract) created with CREATE2 or CREATE3 and the contract contains selfdestruct?
---------------------------------------------------------------------------------------------------------------------------------

  **Incorrect**

  Yes

  **Correct**

  No, `selfdestruct` is not found in the contracts or the contract was not created with CREATE2 or CREATE3

  **Explanation**

  A CREATE2 or CREATE3 contract that contains selfdestruct can be destroyed and replaced with a new contract at the same address. If a metamorphic contract created with CREATE2 contains `selfdestruct`, then the contract code can be replaced in a similar way to upgrading code behind a proxy.

  **Links**

  * `Rajeev forum post about CREATE2 security implications <https://ethereum-magicians.org/t/potential-security-implications-of-create2-eip-1014/2614>`_
  * `CertiK metamorphic contract detector tool <https://medium.com/certik/introducing-certiks-create2-audit-tool-2c75f0b53f54>`_
  * `a16z metamorphic contract detector tool <https://a16zcrypto.com/posts/article/metamorphic-smart-contract-detector-tool/>`_
  * `PoC metamorphic contract detector tool <https://gist.github.com/engn33r/ec2d8f176bff962064afdadedb2d6faf>`_


4. Does the upgraded logic contract have the same state variable layout as the contract it replaces?
---------------------------------------------------------------------------------------------------------

  **Incorrect**

  No

  **Correct**

  Yes, new state variables are only appended to the list of existing state variables. Even better, the new state variables are filling slots that were already reserved using a pattern like `OpenZeppelin contact's storage gap <https://docs.openzeppelin.com/contracts/4.x/upgradeable#storage_gaps>`_.

  **Explanation**

  If the storage variable layout is changed, a storage collision can happen where the variable names in the upgraded contract to not match the variables that are stored in the corresponding storage slots. The storage layout should always consider inheritance, so flattening the contracts to more easily view the storage variables can sometimes be a useful manual approach. Automated tools can also reveal the storage layout of a contract and help verify that no storage collision happens as a result of a contract upgrade.

  **Links**

  * `Furucombo hack <https://medium.com/furucombo/furucombo-post-mortem-march-2021-ad19afd415e>`_
  * `Audius hack <https://blog.audius.co/article/audius-governance-takeover-post-mortem-7-23-22>`_
  * `Ethernaut Level 6 “Delegation” <https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/contracts/levels/Delegation.sol>`_
  * `Ethernaut Level 16 “Preservation” <https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/contracts/levels/Preservation.sol>`_
  * `Ethernaut Level 24 “Puzzle Wallet” <https://github.com/OpenZeppelin/ethernaut/blob/master/contracts/contracts/levels/PuzzleWallet.sol>`_
  * `OpenZeppelin storage collision explanation <https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies#storage-collisions-between-implementation-versions>`_