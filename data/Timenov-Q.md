# Lybra Finance report by Timenov

## Summary
I-01 Empty lines should be removed for better code readability.
I-02 Incorrect naming of interfaces.
I-03 Incorrect naming of modifiers.
I-04 Incorrect naming of event.
I-05 Incorrect naming of function.
I-06 Wrong contract address in comment.
I-07 Use functions instead of modifiers.

### [I-01] Empty lines should be removed for better code readability.
*There are 7 instances of this issue.*

In the `LybraGovernance` contract there are some places where unnecessary empty lines are left. They should be removed for better code readability.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L25C5-L25C5
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L34-L36
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L54C1-L54C1
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L77C7-L77C7
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L85
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L91C9-L91C9
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L204-L205

### [I-02] Incorrect naming of interfaces.
*There is 1 instance of this issue.*

Some of the interfaces do not use to correct naming convention for interfaces. I have included only 1, because it is the only one is scope, however there is also one with wrong naming in `contracts/lybra/interfaces/Iconfigurator`

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

8: interface Ilido // should be ILido
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L8

### [I-03] Incorrect naming of modifiers.
*There are 4 instances of this issue.*

Some of the modifiers do not use the correct naming convention for modifiers.

```solidity
File: contracts/lybra/token/EUSD.sol

83: modifier MintPaused() // should be mintPaused()
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L83C28-L83C28

```solidity
File: contracts/lybra/token/EUSD.sol

87: modifier BurnPaused() // should be burnPaused()
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L87


```solidity
File: contracts/lybra/token/PeUSDCMainnetStableVision.sol

46: modifier MintPaused() // should be mintPaused()
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46

```solidity
File: contracts/lybra/token/PeUSDCMainnetStableVision.sol

50: modifier BurnPaused() // should be burnPaused()
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L50

### [I-04] Incorrect naming of event.
*There is 1 instance of this issue.*

One of the events does not use the correct naming convention for events.

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

70: event tokenMinerChanges(address indexed pool, bool status); // should be TokenMinerChanges
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L70

### [I-05] Incorrect naming of function.
*There is 1 instance of this issue.*

One of the functions does not use the correct naming convention for functions.

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

158: function setvaultBurnPaused(address pool, bool isActive) // should be setVaultBurnPaused
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L158

### [I-06] Wrong contract address in comment.
*There are 2 instances of this issue.*

In 2 comments the address of the contract does not match the name of the contract

```solidity
File: contracts/lybra/pools/LybraWbETHVault.sol

16: WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393 // This is the address of Rocket Pool ETH(rETH) not the address of WBETH
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L16C15-L16C57

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

24: Lido = 0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84; // This is the address of stETH not the address of Lido
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWstETHVault.sol#L24C14-L24C56

### [I-07] Use functions instead of modifiers.
*There are 3 instances of this issue.*

The purpose of a modifier is to check values and revert if condition is not matched. In this case modifiers are used to implement logic. This is wrong and functions should be used instead.

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L72
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L178
https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L56