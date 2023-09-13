# Common ERC Tokens (ERC20, ERC4626)

ERC20 and other token standards, the different implementations of this standard do not all play nicely together. In addition, some ERC20 tokens do not follow the standard exactly.

Other resources:

- https://ercx.runtimeverification.com/

## 1. Are incompatible versions of this ERC considered?

**Incorrect**

No

**Correct**

Yes

**Explanation**

This is an extremely broad category, but it is hard to enumerate every possible issue related to incompatibility. The best approach is to review the corresponding link below outlining unexpected behavior for each token standard. 

**Links**

- [Unexpected or non-compliant ERC20 behavior](https://github.com/d-xo/weird-erc20)
- [Unexpected or non-compliant ERC4626 behavior](https://banteg.mirror.xyz/xMsPLpgsv88NFspah0v1SyJHBk0Yp3vSyzCSznr6ZaM)

## 2. Are any newly created ERC tokens compliant with the standard?

**Incorrect**

No

**Correct**

Yes

**Explanation**

Every ERC standard contains detailed information about what the contract code must do to remain compliant with the standard. If the contract is importing an ERC implementation from a well-known library, such a OpenZeppelin, there is less likelihood of unusual implementation details when compared to a fully custom ERC implementation.

**Links**

- [Slither ERC conformance check for ERC20, ERC4626, and many other token standards](https://github.com/crytic/slither/wiki/ERC-Conformance)

## 3. If tokens with OpenZeppelin and solmate ERC imports involved, are incompatibilities considered?

**Incorrect**

No

**Correct**

Yes, the known compatibility issues have been considered

**Explanation**

Solmate's gas optimizations have led to some differences compared to tokens importing OpenZeppelin ERC libraries. Examples of this are linked to below.

**Links**

- [IERC20 incompatibility between OZ and solmate](https://github.com/transmissions11/solmate/issues/307)
- [`safeTransfer()` reverts on flashloan for certain tokens in solmate](https://github.com/transmissions11/solmate/issues/346)
- [Missing maxwithdraw check in solmate](https://github.com/transmissions11/solmate/issues/362)
- [`totalAssets()` implementation inconsistency in solmate](https://github.com/transmissions11/solmate/issues/348)

## 4. If solmate is used, is there an existence check before any safeTransfer?

**Incorrect**

No, and the lack of this check may cause problems

**Correct**

Yes

**Explanation**

Solmate delegates the responsibility of checking that a token has code to the caller, as stated in the [SafeTransferLib NatSpec](https://github.com/transmissions11/solmate/blob/fadb2e2778adbf01c80275bfb99e5c14969d964b/src/utils/SafeTransferLib.sol#L9).

**Links**

- Past Code4rena issues ([1](https://github.com/code-423n4/2022-08-olympus-findings/issues/117) and [2](https://github.com/code-423n4/2023-01-astaria-findings/issues/158))
