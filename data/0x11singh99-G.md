### Gas Optimizations List
| Number | Optimization Details | Context |
|:--:|:-------| :-----:|


Total 7 issues

## [G-01]  Do not calculate constant variables
Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a 
constant variable is recomputed each time that the variable is used, which wastes some gas each time of use.

3 results - 1 files:

```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol

  76:  bytes32 public constant DAO = keccak256("DAO");
  77:  bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
  78:  bytes32 public constant ADMIN = keccak256("ADMIN");
```
 https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76-L78
Make it commented and directly save hash of these strings by calculating off chain .It will save 30 gas for each time constant is used.30 Gas for calculating keccak256() each time.

## [G-02]  Use assembly for address(0) comparison 
### saves 65 gas each time 

2 results - 1 files:

```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol

   99:  if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
   100:  if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99-L100

Use this instead it saves 130 gas
```solidity
      bool isAddressZero;
        assembly {
            isAddressZero := eq(address(EUSD), 0)
        }
        if (isAddressZero) {
            EUSD = IEUSD(_eusd);
        }
        assembly {
            isAddressZero := eq(address(peUSD), 0)
        }
        if (isAddressZero) {
            peUSD = IEUSD(_peusd);
        }
```
# [G-03] Avoid updating storage when the value hasn't changed
If the old value is equal to the new value, not re-storing the value will avoid a Gsreset (2900 gas), potentially at the expense of a Gcoldsload (2100 gas) or a Gwarmaccess (100 gas)

2 results - 1 files:

```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
110:  mintVault[pool] = isActive;
  
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L110

Convert above to this below code
```solidity
   if (mintVault[pool] = !isActive) {
            mintVault[pool] = isActive;
        }
```

```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
158 :  vaultBurnPaused[pool] = isActive;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L158

## [G-04]  Use custom errors rather than require() strings to save gas
Custom errors save ~50 gas each time they're hit by avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas.

This is all over the protocol files so update wherever require() used
eg:

```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
  function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {
187:    require(newFee <= 500, "Max Redemption Fee is 5%");
       ...
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L187
Use custom errors like this 
```solidity
//Define it in starting of contract
 error MAX_REDEMPTION_FEE_5_PERCENT();
  ...
function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {
  if (newFee <= 500) {
            revert MAX_REDEMPTION_FEE_5_PERCENT();
        }

```
Like this custom errors can be used across the protocol 

## [G-05] Splitting require() statements that use && saves gas

## [G-06] Write for loops in most gas efficient way 
Loop can be written in more gas efficient way, by
1. taking length in stack variable,
2. using unchecked{++i}
3. not re- initializing to 0 since it is default

1 results - 1 files:

```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol

236 :   for (uint256 i = 0; i < _contracts.length; i++) {
       ...
           }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236

Convert above for loop to this for making gas efficient saves more than 130 Gas per iteration
```solidity
   uint256 contractsLength=_contracts.length;
    for (uint256 i; i < contractsLength; ) {
             ...
            unchecked {
                ++i;
            }
        }

```
