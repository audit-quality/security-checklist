# Cryptography

Many cryptographic primitives are used by protocols. These can often have implementation mistakes when a common library is not used or when key values are improperly set.

Other resources:

- None currently

## 1. Is the Merkle proof validation done properly?

**Incorrect**

No

**Correct**

Yes

**Explanation**

TODO and improve the focus of the question

**Links**

- [OpenSea Merkle Audit Finding](https://github.com/code-423n4/2022-05-opensea-seaport-findings/issues/168)
- [Binance bridge vulnerability](https://medium.com/immunefi/hack-analysis-binance-bridge-october-2022-2876d39247c1)
- [OpenZeppelin merkle multiproof vulnerability in OZ >=4.7.0 <4.9.2](https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-wprv-93r4-jj2p)

## 2. Is the Merkle root set to a non-zero value?

**Incorrect**

No

**Correct**

Yes

**Explanation**

The default value of an integer in solidity is zero. So if valid proofs are stored in a mapping that maps the proof to the root, then invalid proofs map to a value of zero, which is the same as the valid root if the root is set to zero. This allows invalid proofs to be accepted because there is inadequate differentiation between valid and invalid proofs.

**Links**

- [Nomad bridge hack](https://medium.com/nomad-xyz-blog/nomad-bridge-hack-root-cause-analysis-875ad2e5aacd)

## 3. Is ECDSA `recover` malleable?

**Incorrect**

No

**Correct**

Yes

**Explanation**

TODO and improve the focus of the question

**Links**

- [Deeper dive into ECDSA signature malleability](https://0xsomeone.medium.com/b002-solidity-ec-signature-pitfalls-b24a0f91aef4)
- [Polygon $2 million bug bounty](https://medium.com/immunefi/polygon-lack-of-balance-check-bugfix-postmortem-2-2m-bounty-64ec66c24c7d)
- [OpenZeppelin ECDSA recover vulnerability in OZ >= 4.1.0 < 4.7.3](https://github.com/OpenZeppelin/openzeppelin-contracts/security/advisories/GHSA-4h98-2769-gh6h)
- [Signature malleability PoC](https://github.com/pcaversaccio/malleable-signatures)

## 4. Is the result of solidity's `ecrecover` checked for a return value of 0?

**Incorrect**

No, the return value from `ecrecover` is not checked for a value of 0.

**Correct**

Yes, the code will revert if `ecrecover` returns a value of 0.

**Explanation**

[The solidity docs](https://docs.soliditylang.org/en/v0.8.20/units-and-global-variables.html#mathematical-and-cryptographic-functions) explain that `ecrecover` will return zero on error. If there is an error, the code should revert instead of continuing with the assumption that the recovered value is zero.

**Links**

- [Solidity docs describing ecrecover](https://docs.soliditylang.org/en/v0.8.20/units-and-global-variables.html#mathematical-and-cryptographic-functions)
- OpenZeppelin's [ECDSA.sol `tryRecover()` avoids this issue as the comments explain](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/1a77a508f93e2df058cb082def4753a060aefa8f/contracts/utils/cryptography/ECDSA.sol#L147-L155)
- Code4rena has many findings [related to a missing zero check for ecrecover](https://github.com/search?q=org%3Acode-423n4+ecrecover+check&type=issues), including [this one](https://github.com/code-423n4/2021-08-gravitybridge-findings/issues/61) and [this one](https://github.com/code-423n4/2022-07-golom-findings/issues/357).