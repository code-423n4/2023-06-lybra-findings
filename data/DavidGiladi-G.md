# Report: Identified 12 Distinct Gas Optimization Opportunities

 - [Multiplication and Division by 2 Should use in Bit Shifting](#multiplication-and-division-by-2-should-use-in-bit-shifting) (2 results) (Gas Optimization)
 - [Modulus operations that could be unchecked](#modulus-operations-that-could-be-unchecked) (1 results) (Gas Optimization)
 - [State variables that could be declared constant](#state-variables-that-could-be-declared-constant) (2 results) (Gas Optimization)
 - [Inefficient assignments to state variables](#inefficient-assignments-to-state-variables) (1 results) (Gas Optimization)
 - [Inefficient Parameter Storage](#inefficient-parameter-storage) (4 results) (Gas Optimization)
 - [Using Storage Instead of Memory for structs/arrays Saves Gas](#using-storage-instead-of-memory-for-structsarrays-saves-gas) (1 results) (Gas Optimization)
 - [Internal or Private Function that Called Once Should Be Inlined to Save Gas](#internal-or-private-function-that-called-once-should-be-inlined-to-save-gas) (4 results) (Gas Optimization)
 - [Optimal State Variable Order](#optimal-state-variable-order) (1 results) (Gas Optimization)
 - [Use Small Integer For Efficiency](#use-small-integer-for-efficiency) (2 results) (Gas Optimization)
 - [Avoid Unnecessary Computation Inside Loops](#avoid-unnecessary-computation-inside-loops) (1 results) (Gas Optimization)
 - [Redundant Contract Existence Check in Consecutive External Calls](#redundant-contract-existence-check-in-consecutive-external-calls) (1 results) (Gas Optimization)
 - [Usage of Custom Errors for Gas Efficiency](#usage-of-custom-errors-for-gas-efficiency) (4 results) (Gas Optimization)

# 
## Optimal State Variable Order
- Severity: Gas Optimization
- Confidence: High

### Description
Detect optimal variable order in contract storage layouts to decrease the number of slots used.

<details>

<summary>
There are 1 instances of this issue:

</summary>

###
- Optimization opportunity in storage variable layout of contract File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
```
 
Line: 17          abstract contract LybraEUSDVaultBase 
```

- original variable order (count: 13 slots)
    - IEUSD EUSD
    - IERC20 collateralAsset
    - Iconfigurator configurator
    - uint256 badCollateralRatio
    - IPriceFeed etherOracle
    - uint256 totalDepositedAsset
    - uint256 lastReportTime
    - uint256 poolTotalEUSDCirculation
    - mapping(address => uint256) depositedAsset
    - mapping(address => uint256) borrowed
    - uint8 vaultType
    - uint256 feeStored
    - mapping(address => uint256) depositedTime


 - optimized variable order (count: 12 slots)
    - IEUSD EUSD
    - uint8 vaultType
    - IERC20 collateralAsset
    - Iconfigurator configurator
    - IPriceFeed etherOracle
    - uint256 badCollateralRatio
    - uint256 totalDepositedAsset
    - uint256 lastReportTime
    - uint256 poolTotalEUSDCirculation
    - mapping(address => uint256) depositedAsset
    - mapping(address => uint256) borrowed
    - uint256 feeStored
    - mapping(address => uint256) depositedTime


[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L17-L328](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L17-L328)


</details>

# 



## Multiplication and Division by 2 Should use in Bit Shifting
- Severity: Gas Optimization
- Confidence: High
- Total Gas Saved: 40


### Description
The expressions 'x * 2' and 'x / 2' can be optimized for gas efficiency by utilizing bitwise operations. In Solidity, you can achieve the same results by using bitwise left shift (x << 1) for multiplication and bitwise right shift (x >> 1) for division.

Using bitwise shift operations (SHL and SHR) instead of multiplication (MUL) and division (DIV) opcodes can lead to significant gas savings. The MUL and DIV opcodes cost 5 gas, while the SHL and SHR opcodes incur a lower cost of only 3 gas.

By leveraging these more efficient bitwise operations, you can reduce the gas consumption of your smart contracts and enhance their overall performance.

<details>

<summary>
There are 2 instances of this issue:

</summary>

###
- File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
```
 
Line: 159          require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
 instead `2` use bit shifting `1` 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159)


- File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
```
 
Line: 130          require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");

```
 instead `2` use bit shifting `1` 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130)


</details>

# 


## Redundant Contract Existence Check in Consecutive External Calls
- Severity: Gas Optimization
- Confidence: Medium
- Total Gas Saved: 100

### Description
This detector identifies instances where there are consecutive external calls to the same contract, where the subsequent calls could use a low-level call to save gas.

Note: This detector only triggers if the function call does not return any value. 

Prior to 0.8.10, the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent Solidity versions the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low-level calls never check for contract existence. 

<details>

<summary>
There are 1 instances of this issue:

</summary>

###
- File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
```
 
Line: 205          try  configurator.distributeRewards() {} catch {}


```
This call could be replaced with a low-level call because the contract `Iconfigurator` has already been checked in <br>`Line: 193    try  configurator.refreshMintReward(_onBehalfOf) {} catch {}
`<br>
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L205](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L205)


</details>

# 


## Modulus operations that could be unchecked
- Severity: Gas Optimization
- Confidence: High
- Total Gas Saved: 30

### Description
Modulus operations should be unchecked to save gas since they cannot overflow or underflow. Execution of modulus operations outside `unchecked` blocks adds nothing but overhead. Saves about 30 gas.

<details>

<summary>
There are 1 instances of this issue:

</summary>

###
- File: contracts/lybra/pools/LybraStETHVault.sol
```
 
Line: 102          (block.timestamp - lidoRebaseTime) % 1 days
```
 should be unchecked

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L102](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L102)


</details>

# 

## Avoid Unnecessary Computation Inside Loops
- Severity: Gas Optimization
- Confidence: High

### Description
This detector identifies instances of computations being performed inside loops that could be performed outside the loop to save gas.

Unnecessary computation inside loops:

Any computation performed inside a loop that doesn't change with each iteration can be moved outside the loop to save gas.This detector identifies instances where the following unnecessary computations are performed inside loops:

- 1 Local variables are read inside loops but never modified within the same loop, indicating they could be cached outside the loop.

- 2 Computations involving constant expressions (literals) which result in the same output in every loop iteration.

By addressing these issues, gas usage can be optimized.

<details>

<summary>
There are 1 instances of this issue:

</summary>

###
- File: contracts/lybra/miner/EUSDMiningIncentives.sol
```
 
Line: 141          pool.getVaultType() == 1
```
The following computations can be cached outside of loops for gas optimization `pool.getVaultType() == 1`<br>.
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L141](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L141)


</details>

# 
## Using Storage Instead of Memory for structs/arrays Saves Gas
- Severity: Gas Optimization
- Confidence: Medium
- Total Gas Saved: 4200

### Description
When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct / array to be read from storage. This incurs a Gcoldsload (2100 gas) for each field of the struct / array. If the fields are read from the new memory variable, they incur an additional MLOAD, which is more expensive than a simple stack read.

Instead of declaring the variable with the memory keyword, a more cost-effective approach is to declare the variable with the storage keyword. In this case, you can cache any fields that need to be re-read in stack variables. This approach significantly reduces gas costs, as you only incur the Gcoldsload for the fields actually read.

The only scenario where it makes sense to read the whole struct / array into a memory variable is if the full struct / array is being returned by the function or passed to a function that requires memory. Additionally, if the array / struct is being read from another memory array / struct, using a memory variable may be appropriate.

By carefully considering the storage and memory usage in your contract, you can optimize gas costs and improve the efficiency of your smart contract operations.

<details>

<summary>
There are 1 instances of this issue:

</summary>

###
- File: contracts/lybra/miner/esLBRBoost.sol
```
 
Line: 39          esLBRLockSetting memory _setting = esLBRLockSettings[id]
```
 should be declared as `storage` 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L39](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L39)


</details>

# 
## State variables that could be declared constant
- Severity: Gas Optimization
- Confidence: High
- Total Gas Saved: 4194
### Description
State variables that are not updated following deployment should be declared constant to save gas.

<details>

<summary>
There are 2 instances of this issue:

</summary>

###
- File: contracts/lybra/token/LBR.sol
```
 
Line: 15          uint256 maxSupply = 100_000_000 * 1e18
```
 should be constant 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15)


- File: contracts/lybra/token/esLBR.sol
```
 
Line: 20          uint256 maxSupply = 100_000_000 * 1e18
```
 should be constant 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20)


</details>

# 


## Internal or Private Function that Called Once Should Be Inlined to Save Gas
- Severity: Gas Optimization
- Confidence: High
- Total Gas Saved: 80

## Note:
I reported issues that were overlooked by the winning bot. I reported only on instances that were missed

### Description
Setting the internal/private function that called once to inline would save gas

<details>

<summary>
There are 4 instances of this issue:

</summary>

###
- File: contracts/lybra/miner/EUSDMiningIncentives.sol
```
 
Line: 244          function _min(uint256 x, uint256 y) private pure returns (uint256) 
```
 should be inline to save gas:

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L244-L246](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L244-L246)



- File: contracts/lybra/miner/stakerewardV2pool.sol
```
 
Line: 147          function _min(uint256 x, uint256 y) private pure returns (uint256) 
```
 should be inline to save gas:

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L147-L149](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L147-L149)


- File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
```
 
Line: 307          nction _etherPrice() internal returns (uint256) 
```
 should be inline to save gas:

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L307-L309](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L307-L309)


- File: contracts/lybra/token/EUSD.sol
```
 
Line: 177          function _spendAllowance(address owner, address spender, uint256 amount) internal virtual 
```
 should be inline to save gas:

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L177-L185](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L177-L185)


</details>

# 

## Use Small Integer For Efficiency
- Severity: Gas Optimization
- Confidence: High

## Note:
I reported issues that were overlooked by the winning bot. I reported only on instances that were missed

### Description
This detector flags contracts that inefficiently use `uint` or `int` of sizes smaller than 32 bytes. The EVM operates on 32 bytes at a time, thus using elements smaller than this may cause your contract's gas usage to be higher. Refer to the Solidity documentation for more details: https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html.

<details>

<summary>
There are 2 instances of this issue:

</summary>

###
- File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
```
 
Line: 30          uint8 immutable vaultType = 0
```
 `vaultType` use 256 bites instead of 8

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30)


- File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
```
 
Line: 18          uint8 immutable vaultType = 1
```
 `vaultType` use 256 bites instead of 8

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18)


</details>

# 


## Inefficient Parameter Storage
- Severity: Gas Optimization
- Confidence: Medium

### Description
When passing function parameters, using the `calldata` area instead of `memory` can improve gas efficiency. Calldata is a read-only area where function arguments and external function calls' parameters are stored.

By using `calldata` for function parameters, you avoid unnecessary gas costs associated with copying data from `calldata` to memory. This is particularly beneficial when the parameter is read-only and doesn't require modification within the contract.

Using `calldata` for function parameters can help optimize gas usage, especially when making external function calls or when the parameter values are provided externally and don't need to be stored persistently within the contract.

<details>

<summary>
There are 4 instances of this issue:

</summary>

###
- File: contracts/lybra/governance/GovernanceTimelock.sol
```
 
Line: 15          address[] memory proposers
```
 should be declared as `calldata` instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L15](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L15)


- File: contracts/lybra/governance/GovernanceTimelock.sol
```
 
Line: 15          address[] memory executors
```
 should be declared as `calldata` instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L15](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L15)


- File: contracts/lybra/miner/EUSDMiningIncentives.sol
```
 
Line: 93          address[] memory _pools
```
 should be declared as `calldata` instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93)


- File: contracts/lybra/miner/esLBRBoost.sol
```
 
Line: 33          esLBRLockSetting memory setting
```
 should be declared as `calldata` instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33)


</details>

# 

## Inefficient assignments to state variables
- Severity: Gas Optimization
- Confidence: High
- Total Gas Saved: 113

## Note:
I reported issues that were overlooked by the winning bot. I reported only on instances that were missed

### Description
Assignment operations that involve calculations, like add-assigment (`+=`), for state variables, should be replaced with the appropriate calculation operation, like add (`+`), followed by an assignment operation (`=`), to save gas. Avoids a `Gwarmaccess` (100 gas).

<details>

<summary>
There are 1 instances of this issue:

</summary>

###


- File: contracts/lybra/pools/LybraStETHVault.sol
```
 
Line: 43          totalDepositedAsset += msg.value
```
 should use the appropriate calculation operation (`+`), followed by an assignment operation (`=`)

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L43](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L43)



</details>

# 



## Usage of Custom Errors for Gas Efficiency
- Severity: Gas Optimization
- Confidence: High

## Note:
I have identified issues that were overlooked by the winning bot, specifically focusing on instances where revert was missed.

### Description
This detector flags functions that use revert()/require() strings, which are less gas efficient than custom errors. Custom errors, available from Solidity version 0.8.4, save approximately 50 gas each time they're used by avoiding the need to allocate and store the revert string.

<details>

<summary>
There are 4 instances of this issue:

</summary>

###

- File: contracts/lybra/configuration/LybraConfigurator.sol
```
 
Line: 254          revert('EL')
```
 use custom error instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L254)



- File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
```
 
Line: 292          revert("collateralRatio is Below safeCollateralRatio");

```
 use custom error instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L292](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L292)



- File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
```
 
Line: 227          revert("collateralRatio is Below safeCollateralRatio");

```
 use custom error instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L227](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L227)


- File: contracts/lybra/token/esLBR.sol
```
 
Line: 27          revert("not authorized")
```
 use custom error instead 

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L27](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L27)



</details>

# 
