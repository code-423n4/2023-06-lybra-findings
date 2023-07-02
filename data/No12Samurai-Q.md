# QA Report

## Summary

This QA report highlights two issues related to event parameter inconsistency and incorrect order in the Lybra protocol contracts. The first issue involves the `ClaimReward` event emission in the `ProtocolRewardsPool.sol` contract, where incorrect parameters are used. The second issue is about the `WithdrawAsset` event emission in the `LybraPeUSDVaultBase.sol` contract, where the parameter order is inconsistent between the event definition and its usage. Both issues can lead to confusion and misinterpretation of the event data.



## Issue 1: Incorrect Parameters in `ProtocolRewardsPool.getReward()` Function Event Emission

In the `ProtocolRewardsPool.sol` contract, the `getReward()` function at [L190-L218](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L190-L218) emits the `ClaimReward` event at [L214](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L214) with incorrect parameters. The event emission lacks essential reward information, such as the address of `token` and the `tokenAmount`. The following event is emitted at [L214](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L214):

```solidity
ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);
```

The event is defined at [L46](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L46) with the following parameters:

```solidity
event ClaimReward(address indexed user, uint256 eUSDAmount, address token, uint256 tokenAmount, uint256 time);
```

To resolve this issue, the event emission line in the `getReward()` function should be modified to include the correct parameters. The recommended modification is as follows:

```solidity
ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(EUSD), EUSD.getMintedEUSDByShares(eUSDShare, block.timestamp);
```


## Issue 2: Inconsistent Parameter Order in `LybraPeUSDVaultBase._withdraw` Event Emission

This issue pertains to the incorrect parameter order in the `WithdrawAsset` event emission at [L219](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L219C9-L219C104) within the `LybraPeUSDVaultBase.sol` contract. The discrepancy arises between the event definition and its usage, leading to inconsistent parameter positions.

The issue occurs when emitting the `WithdrawAsset` event in the `LybraPeUSDVaultBase.sol` contract. The event is defined with the following parameter order at [L29](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L29):

```solidity
event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);
```

However, when the event is emitted at [L219](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L219C9-L219C104), the parameters are passed in a different order:

```solidity
emit WithdrawAsset(_provider, address(collateralAsset), _onBehalfOf, _amount, block.timestamp);
```

To address this issue, it is recommended to modify the event definition at [LybraPeUSDVaultBase.sol#L29](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L29) to match the parameter order used when emitting the event at [LybraPeUSDVaultBase.sol#L219](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L219), and also, to be consistent with [LybraEUSDVaultBase.sol#L38](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L38C5-L38C120). The modified event definition would be as follows:

```solidity
event WithdrawAsset(address sponsor, address asset, address indexed onBehalfOf, uint256 amount, uint256 timestamp);
```