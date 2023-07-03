## LBR on Arbitrum: unnecessary dependencies and code

The LBR token implementation refers to the configurator, which will, based on my current my understanding, only be deployed on Ethereum mainnet for V2. 

https://github.com/code-423n4/2023-06-lybra/blob/26915a826c90eeb829863ec3851c3c785800594b/contracts/lybra/token/LBR.sol#L19

With no configurator, `mint()` and `burn()` in the LBR contract on Arbitrum will always revert, as the `tokenMiner` check at the beginning of those functions cannot be done. 

Filing this as a QA finding as it's easy to work around and doesn't seem to cause any bigger issues, but still appears quite inelegant.

## Cleaner way to implement OFTV2

Instead of Copy-Pasting some OFTV2 implementation code into LBR.sol, PeUSD.sol and PeUSDMainnetStableVision.sol, it would be cleaner to base those contracts off OFTV2 given here: https://github.com/LayerZero-Labs/solidity-examples/blob/main/contracts/token/oft/v2/OFTV2.sol

## PeUSD mainnet: zero address check in mint() is redundant

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64

This check is performed in _mint() of the OZ ERC20 implementation (observe that burn() in the same contract doesn't check for the 0 address).

## Flashloan callback interface spec in PeUSDMainnetStableVision

Interface spec declares "amount of token":

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L23

But actual value given is "amount of shares":

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L132

## EUSD.mint() is supposed to return newTotalShares, but doesn't

Function doesn't contain a return statement: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L411

