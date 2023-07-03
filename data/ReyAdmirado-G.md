| | issue |
| ----------- | ----------- |
| 1 | [state variables can be packed into fewer storage slots](#1-state-variables-can-be-packed-into-fewer-storage-slots) |
| 2 | [expressions for constant values such as a call to keccak256(), should use immutable rather than constant](#2-expressions-for-constant-values-such-as-a-call-to-keccak256-should-use-immutable-rather-than-constant) |
| 3 | [state variables should be cached in stack variables rather than re-reading them from storage](#3-state-variables-should-be-cached-in-stack-variables-rather-than-re-reading-them-from-storage-ones-not-found-in-bot-audit) |
| 4 | [can make the variable outside the loop to save gas](#4-can-make-the-variable-outside-the-loop-to-save-gas) |
| 5 | [Avoid a SLOAD optimistically](#5-avoid-a-sload-optimistically) |
| 6 | [using `calldata` instead of `memory` for read-only arguments in external functions saves gas](#6-using-calldata-instead-of-memory-for-read-only-arguments-in-external-functions-saves-gas) |
| 7 | [Ternary over if ... else](#7-ternary-over-if--else) |
| 8 | [Use assembly to check for address(0)](#8-use-assembly-to-check-for-address0) |
| 9 | [state variable should be declared as constant which saves lots of gas](#9-state-variable-should-be-declared-as-constant-which-saves-lots-of-gas) |
| 10 | [part of the code can be pre calculated](#10-part-of-the-code-can-be-pre-calculated) |
| 11 | [cache parts of code for second use](#11-cache-parts-of-code-for-second-use) |
| 12 | [Non-usage of specific imports](#12-non-usage-of-specific-imports) |



## 1. state variables can be packed into fewer storage slots

If variables occupying the same slot are both written the same function or by the constructor, avoids a separate Gsset (20000 gas). Reads of the variables are also cheaper.

`stableToken` and `premiumTradingEnabled` can be put together into one slot reducing slots used by 1 and also they are accessed in one function.
- [LybraConfigurator.sol#L59-L61](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L59-L61)

`isEUSDBuyoutAllowed` and `esLBR` can be put together into one slot reducing slots used by 1 and also they are accessed in one function.
- [EUSDMiningIncentives.sol#L57](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L57)


## 2. expressions for constant values such as a call to keccak256(), should use immutable rather than constant

- [GovernanceTimelock.sol#L10](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L10)
- [GovernanceTimelock.sol#L11](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L11)
- [GovernanceTimelock.sol#L12](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L12)
- [GovernanceTimelock.sol#L13](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L13)

- [LybraConfigurator.sol#L76](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76)
- [LybraConfigurator.sol#L77](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L77)
- [LybraConfigurator.sol#L78](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L78)


## 3. state variables should be cached in stack variables rather than re-reading them from storage (ones not found in bot audit)

The instances below point to the second+ access of a state variable within a function. Caching of a state variable replace each Gwarmaccess (100 gas) with a much cheaper stack read. Other less obvious fixes/optimizations include having local memory caches of state variable structs, or having local caches of state variable contracts/addresses. 


in the modifier here `rewardPerToken()` is being called to get `rewardPerTokenStored` but first we can cache what the function returns and use that cached version to assign `rewardPerTokenStored` #L57 and `userRewardPerTokenPaid[_account] ` #L62. saves 100 gas per call of modifier --> 400 gas save as it is called 4 times
- [stakerewardV2pool.sol#L56-L64](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L56-L64)

in the modifier here `rewardPerToken()` is being called to get `rewardPerTokenStored` but first we can cache what the function returns and use that cached version to assign `rewardPerTokenStored` #L73 and `userRewardPerTokenPaid[_account] ` #L78. saves 100 gas per call of modifier --> 400 gas save as it is called 4 times
- [EUSDMiningIncentives.sol#L73-L78](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L73-L78)

`duration` is being read twice(once in one of if or else then in #L239) cache it before the if statement to save 100 gas
- [EUSDMiningIncentives.sol#L231-L239](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L231-L239)

`depositedAsset[onBehalfOf]` is being read twice, once in #L156 and #L159 so it should be cached before to reduce one complex SLOAD
- [LybraEUSDVaultBase.sol#L156-L159](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156-L159)

`depositedAsset[onBehalfOf]` is being read twice, once in #L190 and #L192 so it should be cached before to reduce one complex SLOAD
- [LybraEUSDVaultBase.sol#L190-L192](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L190-L192)

`borrowed[provider]` will be read twice if the require in #L234 doesnt revert so we can cache before the require to reduce one complex SLOAD 
- [LybraEUSDVaultBase.sol#L234-L236](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L234-L236)

`depositedAsset[onBehalfOf]` is being read twice, once in #L127 and #L130 so it should be cached before to reduce one complex SLOAD
- [LybraPeUSDVaultBase.sol#L127-L130](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127-L130)

`borrowed[provider]` will be read twice if the require in #L159 doesnt revert so we can cache before the require to reduce one complex SLOAD 
- [LybraPeUSDVaultBase.sol#L159-L161](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L159-L161)

if `block.timestamp >= finishAt` isnt true `finishAt` will be read twice and we can save 97 gas if we cache it before the if statement with just risking 3 gas loss if the condition turns out true
- [stakerewardV2pool.sol#L133-L136](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L133-L136)

if `time2fullRedemption[msg.sender] > block.timestamp` condition be true `time2fullRedemption[msg.sender]` is gonna be read twice so we can cache if before the if statement and use that instead to reduce one complex SLOAD. saves lots of gas by risking losing 3 if the condition is false.
- [ProtocolRewardsPool.sol#L92-L93](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L92-L93)

if `time2fullRedemption[user] > lastWithdrawTime[user]` condition be true `lastWithdrawTime[user]` will be read twice(one in condition and one in the inside of if statement) so we can cache it before the if statement to reduce one complex SLOAD by just risking losing 3 gas if the condition is false.
- [ProtocolRewardsPool.sol#L156-L157](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156-L157)

if `(time2fullRedemption[user] > block.timestamp` condition be true `(time2fullRedemption[user]` will be read twice from storage so we can cache it before the if statement to reduce one complex SLOAD with just risking losing 3 gas if the condition be false
- [ProtocolRewardsPool.sol#L162-L163](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L162-L163)

if `_totalShares == 0` is false `_totalShares` will be read twice and we can save 97 gas if we cache it before the if statement with just risking 3 gas loss if the condition turns out true
- [EUSD.sol#L312-L315](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L312-L315)

if `vaultSafeCollateralRatio[pool] == 0` is false `vaultSafeCollateralRatio[pool]` will be read twice and we can reduce one complex SLOAD if we cache it before the if statement with just risking 3 gas loss if the condition turns out true
- [LybraConfigurator.sol#L334](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L334)

if `vaultBadCollateralRatio[pool] == 0` is false `vaultBadCollateralRatio[pool]` will be read twice and we can reduce one complex SLOAD if we cache it before the if statement with just risking 3 gas loss if the condition turns out true
- [LybraConfigurator.sol#L339](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L339)


## 4. can make the variable outside the loop to save gas

make the variable outside the loop and only give the value to variable inside

`pool`
- [EUSDMiningIncentives.sol#L139](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L139)

`borrowed`
- [EUSDMiningIncentives.sol#L140](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L140)


## 5. Avoid a SLOAD optimistically

There is a chance that the first part will be true so the second part doesn’t need to be checked, so it is better to use the part that we have first instead of the part that needs to be called.

swap the 2 sides for the chance of `price <= 1005000` being true and not using a SLOAD on `premiumTradingEnabled`
- [LybraConfigurator.sol#L298](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L298)


## 6. using `calldata` instead of `memory` for read-only arguments in external functions saves gas

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 * <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution. 

- [esLBRBoost.sol#L33](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33)

- [EUSDMiningIncentives.sol#L93](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93)


## 7. Ternary over if ... else

Using ternary operator instead of the if else statement saves gas.

- [stakerewardV2pool.sol#L133-L136](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L133-L136)

- [EUSD.sol#L301-L305](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L301-L305)
- [EUSD.sol#L312-L315](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L312-L315)

- [LybraConfigurator.sol#L199-L203](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L199-L203)

- [EUSDMiningIncentives.sol#L230-L235](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L230-L235)


## 8. Use assembly to check for address(0)

saves 6 gas per instance

- [stakerewardV2pool.sol#L60](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60)

- [PeUSDMainnetStableVision.sol#L64](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64)

- [EUSD.sol#L367](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L367)
- [EUSD.sol#L368](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L368)
- [EUSD.sol#L392](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L392)
- [EUSD.sol#L393](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L393)
- [EUSD.sol#L412](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L412)
- [EUSD.sol#L441](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L441)
- [EUSD.sol#L460](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L460)

- [LybraConfigurator.sol#L99](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99)
- [LybraConfigurator.sol#L100](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L100)

- [EUSDMiningIncentives.sol#L76](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76)

- [LybraEUSDVaultBase.sol#L99](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99)
- [LybraEUSDVaultBase.sol#L127](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L127)
- [LybraEUSDVaultBase.sol#L141](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L141)

- [LybraPeUSDVaultBase.sol#L83](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83)
- [LybraPeUSDVaultBase.sol#L97](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97)
- [LybraPeUSDVaultBase.sol#L111](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111)


## 9. state variable should be declared as constant which saves lots of gas

`maxSupply` is not being updated in this contract and it always has a constant value so it should be declared as a constant saving Gsset (20000 gas) and 100 gas per call
- [esLBR.sol#L20](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20)
- [LBR.sol#L15](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15)


## 10. part of the code can be pre calculated

these parts of the code can be pre calculated and given to the contract as constants this will stop the use of extra operations
even if its for code readability consider putting comments instead.

instead of `100 * 1e18` put the answer to it in the code to reduce one operation use 
- [stakerewardV2pool.sol#L102](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L102)
- [LybraPeUSDVaultBase.sol#L162](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L162)
- [LybraEUSDVaultBase.sol#L237](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L237)


`(86400 * 365) / 10000` can be pre calculated to reduce two operation uses
- [LybraEUSDVaultBase.sol#L301](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L301)
- [LybraPeUSDVaultBase.sol#L238](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L238)


## 11. cache parts of code for second use

instead of doing some operations again answer can be cached at the first time and be used every where.

`balance - preBalance` is calculated in #L25 so if we cache it then we can use the cached version in #L31
- [LybraWbETHVault.sol#L25-L31](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L25-L31)

same here
- [LybraRETHVault.sol#L32-L38](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L32-L38)

`reward - eUSDShare` 
- [ProtocolRewardsPool.sol#L202-L203](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L202-L203)


## 12. Non-usage of specific imports

The current form of relative path import is not recommended for use because it can unpredictably pollute the namespace. Instead, the Solidity docs recommend specifying imported symbols explicitly.
- [more here](https://docs.soliditylang.org/en/v0.8.15/layout-of-source-files.html#importing-other-source-files)

A good example:
```solidity
import {OwnableUpgradeable} from "openzeppelin-contracts-upgradeable/contracts/access/OwnableUpgradeable.sol";
import {SafeTransferLib} from "solmate/utils/SafeTransferLib.sol";
import {SafeCastLib} from "solmate/utils/SafeCastLib.sol";
import {ERC20} from "solmate/tokens/ERC20.sol";
import {IProducer} from "src/interfaces/IProducer.sol";
import {GlobalState, UserState} from "src/Common.sol";
```

- [LybraProxy.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/Proxy/LybraProxy.sol)
- [AdminTimelock.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol)
- [GovernanceTimelock.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol)
- [LybraWbETHVault.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol)
- [esLBR.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol)
- [LybraWstETHVault.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol)
- [LybraRETHVault.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol)
- [PeUSD.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol)
- [esLBRBoost.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol)
- [LBR.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol)
- [LybraStETHVault.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol)
- [stakerewardV2pool.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol)
- [LybraGovernance.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol)
- [PeUSDMainnetStableVision.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol)
- [ProtocolRewardsPool.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol)
- [EUSD.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol)
- [LybraConfigurator.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol)
- [EUSDMiningIncentives.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol)
- [LybraEUSDVaultBase.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol)
- [LybraPeUSDVaultBase.sol](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol)
