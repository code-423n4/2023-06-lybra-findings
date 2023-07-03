## GAS-1: <X> * 2 costs more gas then <X> << 1

### Description

Using bitwize operator instead of '*' or '/' saves gas.

### Affected file

* LybraEUSDVaultBase.sol (Line: 159)
* LybraPeUSDVaultBase.sol (Line: 130)

## GAS-2: Add unchecked{} for subtractions where the operands cannot underflow because of a previous require() or if statement

### Description

require(a <= b); x = b - a => require(a <= b); unchecked { x = b - a }
 if(a <= b); x = b - a => if(a <= b); unchecked { x = b - a }
 This will stop the check for overflow and underflow so it will save gas.

### Affected file

* LBR.sol (Line: 22)
* LybraPeUSDVaultBase.sol (Line: 200)
* LybraStETHVault.sol (Line: 75, 78)
* PeUSD.sol (Line: 14)
* PeUSDMainnetStableVision.sol (Line: 60)
* ProtocolRewardsPool.sol (Line: 201, 202, 203, 209, 211)

## GAS-3: Caching global variables is more expensive than using the actual variable

### Description

 It’s cheaper to use global variable as compared to caching.

### Affected file

* EUSDMiningIncentives.sol (Line: 240)
* LybraEUSDVaultBase.sol (Line: 297)
* LybraStETHVault.sol (Line: 43, 92)
* stakerewardV2pool.sol (Line: 143)

## GAS-4: Do not calculate constants

### Description

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.

### Affected file

* GovernanceTimelock.sol (Line: 10, 11, 12, 13)
* LybraConfigurator.sol (Line: 76, 77, 78)

## GAS-5: Internal functions not called by the contract should be removed to save deployment gas

### Description

If the functions are required by an interface, the contract should inherit from that interface and use the override keyword.

### Affected file

* LBR.sol (Line: 49, 56, 61, 70)
* LybraEUSDVaultBase.sol (Line: 307)
* LybraGovernance.sol (Line: 59, 66, 76, 98)
* LybraPeUSDVaultBase.sol (Line: 244)
* PeUSD.sol (Line: 31, 38, 43, 52)
* PeUSDMainnetStableVision.sol (Line: 184, 191, 196, 205)
* esLBR.sol (Line: 26)

## GAS-6: Length calculated at each iteration in For-loop

### Description

Caching the array length outside a loop saves reading it on each iteration, as long as the array’s length is not changed during the loop.

### Affected file

* EUSDMiningIncentives.sol (Line: 94, 138)
* LybraConfigurator.sol (Line: 236)

## GAS-7: Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate

### Description

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally, if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) and that calculation’s associated stack operations.

### Affected file

* EUSD.sol (Line: 51, 56)
* EUSDMiningIncentives.sol (Line: 46, 48, 49)
* LybraConfigurator.sol (Line: 38, 39, 40, 41, 42, 43, 44, 45, 46, 47)
* LybraEUSDVaultBase.sol (Line: 28, 29, 32)
* LybraPeUSDVaultBase.sol (Line: 21, 22, 23, 24)
* ProtocolRewardsPool.sol (Line: 33, 35, 36, 37, 38)
* stakerewardV2pool.sol (Line: 33, 35, 36, 41)

## GAS-8: Non-usage of specific imports

### Description

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace. Instead, the Solidity docs recommend specifying imported symbols explicitly.

### Affected file

* AdminTimelock.sol (Line: 5)
* EUSD.sol (Line: 5, 6, 7, 8)
* EUSDMiningIncentives.sol (Line: 12, 13, 14, 15, 16, 17)
* GovernanceTimelock.sol (Line: 5)
* LBR.sol (Line: 9, 10, 11)
* LybraConfigurator.sol (Line: 17, 18)
* LybraEUSDVaultBase.sol (Line: 5, 6, 7)
* LybraGovernance.sol (Line: 5, 6, 7)
* LybraPeUSDVaultBase.sol (Line: 5, 6, 7)
* LybraProxy.sol (Line: 5)
* LybraProxyAdmin.sol (Line: 4)
* LybraRETHVault.sol (Line: 5, 6, 7)
* LybraStETHVault.sol (Line: 5, 6)
* LybraWbETHVault.sol (Line: 5, 6, 7)
* LybraWstETHVault.sol (Line: 5, 6, 7)
* PeUSD.sol (Line: 5, 6)
* PeUSDMainnetStableVision.sol (Line: 16, 17, 18, 19)
* ProtocolRewardsPool.sol (Line: 12, 13, 14, 15, 16)
* esLBR.sol (Line: 10, 11)
* esLBRBoost.sol (Line: 5)
* stakerewardV2pool.sol (Line: 4, 5, 6)

## GAS-9: Not using the named return variables when a function returns, wastes deployment gas

### Description

It is not necessary to have both a named return and a return statement.

### Affected file

* PeUSDMainnetStableVision.sol (Line: 167)

## GAS-10: Replace modifier with function

### Description

Modifiers make code more elegant, but cost more than normal functions.

### Affected file

* EUSD.sol (Line: 79, 83, 87)
* EUSDMiningIncentives.sol (Line: 72)
* LybraConfigurator.sol (Line: 85, 90)
* PeUSDMainnetStableVision.sol (Line: 42, 46, 50)
* ProtocolRewardsPool.sol (Line: 178)
* stakerewardV2pool.sol (Line: 56)

## GAS-11: Use ```assembly``` to write address storage values

### Affected file

* EUSD.sol (Line: 93, 421, 471)
* EUSDMiningIncentives.sol (Line: 65, 66, 67, 68, 69, 73, 74, 85, 86, 90, 97, 102, 107, 112, 116, 121, 125, 126, 129, 231, 234, 239, 240)
* LBR.sol (Line: 19, 22)
* LybraConfigurator.sol (Line: 81, 82, 138, 148, 168, 188, 248, 256, 262)
* LybraEUSDVaultBase.sol (Line: 47, 48, 49, 50, 297)
* LybraGovernance.sol (Line: 40, 41, 80)
* LybraPeUSDVaultBase.sol (Line: 38, 39, 40, 41)
* LybraRETHVault.sol (Line: 24, 43)
* LybraStETHVault.sol (Line: 27)
* LybraWstETHVault.sol (Line: 26)
* PeUSD.sol (Line: 14)
* PeUSDMainnetStableVision.sol (Line: 56, 57, 60)
* ProtocolRewardsPool.sol (Line: 49, 53, 54, 55, 60, 233, 236, 238)
* esLBR.sol (Line: 23)
* stakerewardV2pool.sol (Line: 50, 51, 52, 57, 58, 123, 128, 134, 137, 142, 143)

## GAS-12: Use assembly to check for address(0)

### Description

Saves 6 gas per instance if using assembly to check for address(0).

### Remediation

```
assembly {
 if iszero(_addr) {
  mstore(0x00, "zero address")
  revert(0x00, 0x20)
 }
}
```

### Affected file

* EUSD.sol (Line: 367, 368, 392, 393, 412, 441, 460)
* LybraEUSDVaultBase.sol (Line: 99, 127, 141)
* LybraPeUSDVaultBase.sol (Line: 83, 97, 111)
* PeUSDMainnetStableVision.sol (Line: 64)

## GAS-13: Use constants instead of type(uintx).max

### Description

It uses more gas in the distribution process and also for each transaction than constant usage.

### Affected file

* EUSD.sol (Line: 179)

## GAS-14: Use hardcoded address instead address(this)

### Description

Instead of using ```address(this)```, it is more gas-efficient to pre-calculate and use the hardcoded ```address```

### Affected file

* GovernanceTimelock.sol (Line: 20)
* LBR.sol (Line: 43, 64)
* LybraConfigurator.sol (Line: 290, 295)
* LybraEUSDVaultBase.sol (Line: 75, 160, 171, 197, 204, 205, 260, 292, 301)
* LybraPeUSDVaultBase.sol (Line: 45, 60, 61, 62, 128, 131, 141, 174, 226, 238)
* LybraRETHVault.sol (Line: 29, 31)
* LybraStETHVault.sol (Line: 63)
* LybraWbETHVault.sol (Line: 22, 24)
* PeUSD.sol (Line: 25, 46)
* PeUSDMainnetStableVision.sol (Line: 80, 81, 82, 133, 176, 199)
* ProtocolRewardsPool.sol (Line: 195, 200)
* stakerewardV2pool.sol (Line: 85)

## GAS-15: Use nested if and avoid multiple check combinations

### Description

Using nested is cheaper than using && multiple check combinations. There are more advantages, such as easier to read code and better coverage reports.

### Affected file

* LBR.sol (Line: 64)
* LybraEUSDVaultBase.sol (Line: 204)
* PeUSD.sol (Line: 46)
* PeUSDMainnetStableVision.sol (Line: 199)