### Gas Optimizations List
| Number | Optimization Details | Instances |
|:--:|:-------| :-----:|
| [G-01] | Do not calculate constant variables | 3 |
| [G-02] | Use assembly for address(0) comparison |4 |
| [G-03] | Avoid updating storage when the value hasn't changed | 3 |
| [G-04] | Use custom errors rather than require() strings to save gas |__|
| [G-05] | Function calls can be cached rather than re calling | 1 |
| [G-06] | Write for loops in most gas efficient way | 3 |
| [G-07] | Reading from calldata/memory array multiple times should be cached | 2 |
| [G-08] | State variables should be cached in stack variables rather than re-reading them from storage | 16 |
| [G-09] | Using storage instead of memory for structs/arrays saves gas | 2 |
| [G-10] | Use unchecked{} whenever underflow/overflow not possible saves 130 gas each time | 10 |

Total 7 issues

## [G-01]  Do not calculate constant variables
Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a 
constant variable is recomputed each time that the variable is used, which wastes some gas each time of use.

*Total 3 instances - 1 files:*

### Instance#1-3 :
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

*4 instances - 3 files:*

### Instance#1-2:
```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol

   99:  if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);
   100:  if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L99-L100

Use assembly like this below it saves 130 gas
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
### Instance#3 : Use assembly like instance#1-2 suggestion of [G-02] above but use  if(!isAddressZero) {}for it
```solidity
File :/contracts/lybra/miner/EUSDMiningIncentives.sol

//@audit gas use assembly to check address(0)
 76:       if (_account != address(0)) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76

### Instance#4 : Use assembly like instance#1-2 suggestion of [G-02] above but use  if(!isAddressZero) {}for it
```solidity
File :/contracts/lybra/miner/stakerewardV2pool.sol

//@audit gas use assembly to check address(0)
 60:       if (_account != address(0)) {
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60
# [G-03] Avoid updating storage when the value hasn't changed
If the old value is equal to the new value, not re-storing the value will avoid a Gsreset (2900 gas), potentially at the expense of a Gcoldsload (2100 gas) or a Gwarmaccess (100 gas)

*3 instances - 1 files:*
### Instance#1:
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
### Instance#2:
```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
158 :  vaultBurnPaused[pool] = isActive;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L158
### Instance#3:
```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
270 :   redemptionProvider[msg.sender] = _bool;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L270

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

## [G-05] Function calls can be cached rather than re calling from same function with same value will save gas

 *1 instances - 1 files:*
### Instance#1 :  getPreUnlockableAmount(msg.sender); can be cached to save gas instead of re calling in line 116, it will return same value here
```solidity
File :contracts/lybra/miner/ProtocolRewardsPool.sol

         // @audit gas  getPreUnlockableAmount(msg.sender) can be cached to save gas
115:      uint256 burnAmount = getReservedLBRForVesting(msg.sender) - getPreUnlockableAmount(msg.sender);
116:      uint256 amount = getPreUnlockableAmount(msg.sender) + getClaimAbleLBR(msg.sender);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L115-L116


## [G-06] Write for loops in most gas efficient way 
Loop can be written in more gas efficient way, by
1. taking length in stack variable,
2. using unchecked{++i} with pre increment
3. not re-initializing to 0 since it is default
4. Using <= is cheaper than <


*3 instances - 2 files:*
### Instance#1:
```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol

236 :   for (uint256 i = 0; i < _contracts.length; i++) {
       ...
           }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236

Convert above for loop to this for making gas efficient saves more than 130 Gas per iteration
```solidity
   uint256 contractsLength = _contracts.length - 1;
    for (uint256 i; i <= contractsLength; ) {
             ...
            unchecked {
                ++i;
            }
        }

```
### Instance#2: Write for loop gas efficient like above
```solidity
File :/contracts/lybra/miner/EUSDMiningIncentives.sol
94:   for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
154: 
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L94
### Instance#3: Write for loop gas efficient like first instance suggestion
```solidity
File :/contracts/lybra/miner/EUSDMiningIncentives.sol
//@audit gas make loop proper with gas efficient
154:   for (uint i = 0; i < pools.length; i++) {
            ILybra pool = ILybra(pools[i]);
            uint borrowed = pool.getBorrowedOf(user);
            if (pool.getVaultType() == 1) {
                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
            }
            amount += borrowed; //@audit gas x=x+y is cheaper than x+=y
        }

```

## [G-07] Reading from calldata/memory array multiple times should be cached to stack variables

*2 instances - 1 files:*
### Instance#1-2:
```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
          //@audit _contracts[i] and _bools[i] can be cached instead of rereading  can save 600 gas per iteration
           function setTokenMiner(address[] calldata _contracts, bool[] calldata _bools) external checkRole(TIMELOCK) {
        for (uint256 i = 0; i < _contracts.length; i++) {
    237:       tokenMiner[_contracts[i]] = _bools[i];
    238:       emit tokenMinerChanges(_contracts[i], _bools[i]);
   
        }
    }


```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L237-L238

## [G‑08] State variables should be cached in stack variables rather than re-reading them from storage
The instances below point to the second+ access of a state variable within a function. Caching of a state variable replaces each Gwarmaccess (100 gas) with a much cheaper stack read
Note: view functions also added since they are used in state changing functions

*16 instances - 3 files:*
### Instance#1-3: EUSD peUSD curvepool can be cached
```solidity
 File : contracts/lybra/configuration/LybraConfigurator.sol
   290: uint256 peUSDBalance = peUSD.balanceOf(address(this));
        if(peUSDBalance >= 1e21) {
            peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);
            lybraProtocolRewardsPool.notifyRewardAmount(peUSDBalance, 2);
        }
        uint256 balance = EUSD.balanceOf(address(this));
        if (balance > 1e21) {
            uint256 price = curvePool.get_dy_underlying(0, 2, 1e18);
            if(!premiumTradingEnabled || price <= 1005000) {
                EUSD.transfer(address(lybraProtocolRewardsPool), balance);
                lybraProtocolRewardsPool.notifyRewardAmount(balance, 0);
            } else {
                EUSD.approve(address(curvePool), balance);
  303:          uint256 amount = curvePool.exchange_underlying(0, 2, balance, balance * price * 998 / 1e21);
                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);
                lybraProtocolRewardsPool.notifyRewardAmount(amount, 1);
            }
        }
         
 
```
 https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L290-L303
### Instance#4-5 : vaultSafeCollateralRatio[pool]  and vaultBadCollateralRatio[pool] can be cached 
```solidity
File : contracts/lybra/configuration/LybraConfigurator.sol
   334:     if (vaultSafeCollateralRatio[pool] == 0) return 160 * 1e18;
   335:     return vaultSafeCollateralRatio[pool];
          ...
   339:  if(vaultBadCollateralRatio[pool] == 0) return vaultSafeCollateralRatio[pool] - 1e19;
   340:    return vaultBadCollateralRatio[pool];
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L334-L335
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L339-L340
### Instance#6 : userLockStatus[user] can be cached below
```solidity
File: contracts/lybra/miner/esLBRBoost.sol
  //@audit userLockStatus[user] can be cached 
 58:      uint256 boostEndTime = userLockStatus[user].unlockTime;
 59:       uint256 maxBoost = userLockStatus[user].miningBoost;
```
### Instance#7-8 : finishAt and duration state var. can be cached here instead of re-reading in line 230,231,233,234,239
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
    //@audit finishAt can be cached
230: if (block.timestamp >= finishAt) {
231:            rewardRatio = amount / duration;
       } else {
233:           uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
234:           rewardRatio = (amount + remainingRewards) / duration;  //@audit duration can be cached
        }

        require(rewardRatio > 0, "reward ratio = 0");

239:      finishAt = block.timestamp + duration;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L230-L239

### Instance#9-11 : finishAt, duration and rewardRatio state variables can be cached here instead of re-reading 
```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol
           //@audit finishAt can be cached
133:        if (block.timestamp >= finishAt) {
134:            rewardRatio = _amount / duration; //@audit duration can be cached
        } else {
              //@audit finishAt can be cached above instead of re reading here after if() check
136:         uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
137:            rewardRatio = (_amount + remainingRewards) / duration;
        }

140        require(rewardRatio > 0, "reward ratio = 0");
         
142:        finishAt = block.timestamp + duration;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L133-L142


### Instance#12 : time2fullRedemption[msg.sender] can be cached instead of re reading
```solidity
File :contracts/lybra/miner/ProtocolRewardsPool.sol

      //  @audit gas time2fullRedemption[msg.sender] can be cached
92:  if (time2fullRedemption[msg.sender] > block.timestamp) {
93:          total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
        }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L92C8-L94C10

### Instance#13-15 :state variables time2fullRedemption[msg.sender], lastWithdrawTime[user] and unstakeRatio[user] can be cached instead of re reading to save gas 
```solidity
File :contracts/lybra/miner/ProtocolRewardsPool.sol
     //@audit gas state var. can be cached instead of re-reading
156:  if (time2fullRedemption[user] > lastWithdrawTime[user]) {
157:           amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * 
        (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - 
            lastWithdrawTime[user]);
       }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156-L157
### Instance#16 : time2fullRedemption[msg.sender] can be cached instead of re reading
```solidity
File :contracts/lybra/miner/ProtocolRewardsPool.sol

      //  @audit gas time2fullRedemption[msg.sender] can be cached
162: if (time2fullRedemption[user] > block.timestamp) {
163:            amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp);
        }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L162C9-L164C10

## [G-09] Using storage instead of memory for structs/arrays saves gas
When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read.
 *2 instances - 1 files:*
### Instance#1-2 :
```solidity
File: contracts/lybra/miner/esLBRBoost.sol
 39:      esLBRLockSetting memory _setting = esLBRLockSettings[id];
 40:      LockStatus memory userStatus = userLockStatus[msg.sender];
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L39C6-L40C67

## [G-10] Use unchecked{} whenever underflow/overflow not possible saves 130 gas each time
Because of above if condition check, it can be decided that underflow/overflow not possible.

 *10 instances - 4 files:*
### Instance#1-2 :(boostEndTime - userUpdatedAt)  and (time - userUpdatedAt) can be unchecked due to line 60 and 63 if () condition
```diff
 File: contracts/lybra/miner/esLBRBoost.sol

60:  if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt) {
61:            return 0;
        }
63:        if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {
            return maxBoost;
        } else {
66:            uint256 time = block.timestamp > finishAt ? finishAt : block.timestamp;
            //@audit add unchecked{} in difference due to above if()
- 67:            return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);
+ 67:    return (unchecked{(boostEndTime - userUpdatedAt)}* maxBoost) / unchecked{(time - userUpdatedAt)};
 ```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L67

### Instance#3 : use unchecked{} in subtraction due to line 230 if() condition since subtraction in else{} block
```diff
File: contracts/lybra/miner/EUSDMiningIncentives.sol
230:   if (block.timestamp >= finishAt) {
            rewardRatio = amount / duration;
        } else {  //audit use unchecked{} in subtraction due to line 230 if() condition
- 233:           uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
+ 233:           uint256 remainingRewards = unchecked{(finishAt - block.timestamp)} * rewardRatio;
            rewardRatio = (amount + remainingRewards) / duration;
        }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L233

### Instance#4 : use unchecked{} in subtraction due to line 133 if() condition since subtraction in else{} block
```diff
File: contracts/lybra/miner/stakerewardV2pool.sol
133:    if (block.timestamp >= finishAt) {
            rewardRatio = _amount / duration;
        } else { //audit use unchecked{} in subtraction due to line 133 if() condition
- 136:        uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
+ 136:        uint256 remainingRewards = unchecked{(finishAt - block.timestamp)} * rewardRatio;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L136

### Instance#5 : use unchecked{} in subtraction in line 93 due to line 92 if() condition
```diff
File :contracts/lybra/miner/ProtocolRewardsPool.sol
92:    if (time2fullRedemption[msg.sender] > block.timestamp) {
- 93:          total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
+ 93:   total += unstakeRatio[msg.sender] * unchecked{(time2fullRedemption[msg.sender] - block.timestamp)};
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L93

### Instance#6-7 : use  unchecked{} in subtraction in line 157 due to line 156  if() condition check and ternary check for (block.timestamp - lastWithdrawTime[user])
```diff
File :contracts/lybra/miner/ProtocolRewardsPool.sol
156:   if (time2fullRedemption[user] > lastWithdrawTime[user]) {
- 157:           amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
+ 157:           amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * unchecked{(time2fullRedemption[user] - lastWithdrawTime[user])} : unstakeRatio[user] * unchecked{(block.timestamp - lastWithdrawTime[user])};
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156-L157

### Instance#8-10:  use  unchecked{} in subtraction due to their prior checks they can't underflow
```diff
File :contracts/lybra/miner/ProtocolRewardsPool.sol
//@audit gas  reward-balance can be unchecked due to ternary check it can't underflow
- 196 : uint256 eUSDShare = balance >= reward ? reward : reward - balance;
+ 196 : uint256 eUSDShare = balance >= reward ? reward : unchecked{reward - balance};
  197:            EUSD.transferShares(msg.sender, eUSDShare);
  198:           if(reward > eUSDShare) {
                ERC20 peUSD = ERC20(configurator.peUSD());
  200:                uint256 peUSDBalance = peUSD.balanceOf(address(this));
- 201:               if(peUSDBalance >= reward - eUSDShare) {
- 202:                    peUSD.transfer(msg.sender, reward - eUSDShare);
- 203:                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), reward - eUSDShare, block.timestamp);
       //@audit gas this can be unchecked due to line 198 if condition it can't underflow, and calculate 1 time instead of three,it saves more gas
               uint256 rewardeUsdShareDiff=unchecked{reward - eUSDShare};
+ 201:               if(peUSDBalance >= rewardeUsdShareDiff) {
+ 202:                    peUSD.transfer(msg.sender, rewardeUsdShareDiff);
+ 203:                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), rewardeUsdShareDiff, block.timestamp);
                } else {
                    if(peUSDBalance > 0) {
                        peUSD.transfer(msg.sender, peUSDBalance);
                    }
                    ERC20 token = ERC20(configurator.stableToken());
                    //@audit gas reward - eUSDShare - peUSDBalance can be unchecked due to line 201 if check and it is in else block so can't underflow
- 209:               uint256 tokenAmount = (reward - eUSDShare - peUSDBalance) * token.decimals() / 1e18;
+ 209:               uint256 tokenAmount = unchecked{(reward - eUSDShare - peUSDBalance)} * token.decimals() / 1e18;
                    token.transfer(msg.sender, tokenAmount);
- 211:                  emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), reward - eUSDShare, block.timestamp);
   //@audit gas reward-eUSDShare can be unchecked due to line 198 if check and it is in else block so can't underflow
+ 211:                  emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), unchecked{reward - eUSDShare}, block.timestamp);
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L196-L211

