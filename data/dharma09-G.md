**Table of contents**

- [The result of a function call should be cached rather than re-calling the function](#The-result-of-a-function-call-should-be-cached-rather-than-re-calling-the-function)
    - [ProtocolRewardsPool.sol.notifyRewardAmount() : Results of `totalStaked()` should be cached](#protocolrewardspool.sol.notifyrewardamount()-:-results-of-`totalstaked()`-should-be-cached)
    - [EUSDMiningIncentives.sol.rewardPerToken() : Result of `totalStaled()` should be cached](#eusdminingincentives.sol.rewardpertoken()`-:-result-of-`totalstaled()`-should-be-cached)
    - [PeUSDMainnetStableVision.sol.convertToPeUSDAndCrossChain() : Results of _msgsender() should be cached](#peusdmainnetstablevision.sol.converttopeusdandcrosschain()-:-results-of-_msgsender()-should-be-cached)
    - [PeUSDMainnetStableVision.sol.convertToPeUSD : Results of `_msgsender()` should be cached](#peusdmainnetstablevision.sol.converttopeusd`-:-results-of-`_msgsender()`-should-be-cached)
- [State variables only set in the constructor should be declared `immutable`](#state-variables-only-set-in-the-constructor-should-be-declared `immutable)
- [Multiple accesses of a mapping/array should use a local variable cache](#multiple-accesses-of-a-mapping/array-should-use-a-local-variable-cache)
    - [LybraGovernance.sol.proposals()` :`proposalData[proposalId] should be cached in local storage](#lybragovernance.sol.proposals()`-:`proposaldata[proposalid]`-should-be-cached-in-local-storage)
    - [LybraGovernance.sol.getReceipt()` :`proposalData[proposalId] should be cached in local storage](#lybragovernance.sol.getreceipt()`-:`proposaldata[proposalid]`-should-be-cached-in-local-storage)
- [Internal or private functions only called once can be inlined to save gas Gas saved: 20 * 20= 400](#internal-or-private-functions-only-called-once-can-be-inlined-to-save-gas gas-saved:-20-*-20=-400)
- [Using storage instead of memory for structs/arrays saves gas](#using-storage-instead-of-memory-for-structs/arrays-saves-gas)
- [A modifier used only once and not being inherited should be inlined to save gas](#a-modifier-used-only-once-and-not-being-inherited-should-be-inlined-to-save-gas)

# The result of a function call should be cached rather than re-calling the function

---

External calls are expensive. Consider caching the following:

# `ProtocolRewardsPool.sol.notifyRewardAmount()` : Results of `totalStaked()` should be cached

---

In Solidity, caching repeated function calls can be an effective way to optimize gas usage, especially when the function is called frequently with the same arguments

- [ProtocolRewardsPool.sol#L230](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L230)

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
	227: function notifyRewardAmount(uint amount, uint tokenType) external {
	228:        require(msg.sender == address(configurator));
	229:        if (totalStaked() == 0) return; //@audit: Initial call
	230:        require(amount > 0, "amount = 0");
	231:        if(tokenType == 0) {
	232:            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
	233:            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked(); //@audit: 2nd call
	234        } else if(tokenType == 1) {
	235:            ERC20 token = ERC20(configurator.stableToken());
	236:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked(); //@audit  2nd call
	234        } else if(tokenType == 1) {
	237:        } else {
	238:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked(); //@audit  2nd call
	234        } else if(tokenType == 1) {
	239:        }
	240:    }
```

```diff
diff --git a/contracts/lybra/miner/ProtocolRewardsPool.sol b/contracts/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..7c70c15 100644
--- a/contracts/lybra/miner/ProtocolRewardsPool.sol
+++ b/contracts/lybra/miner/ProtocolRewardsPool.sol
@@ -225,17 +225,18 @@ contract ProtocolRewardsPool is Ownable {
      * @dev When receiving stablecoin tokens other than eUSD, the decimals of the token are converted to 18 for consistent calculations.
      */
     function notifyRewardAmount(uint amount, uint tokenType) external {
-        require(msg.sender == address(configurator));
-        if (totalStaked() == 0) return;
+        require(msg.sender == address(configurator)); //@audit cache function
+        uint256 _totalStaked = totalStaked();
+        if (_totalStaked == 0) return;
         require(amount > 0, "amount = 0");
         if(tokenType == 0) {
             uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
-            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
+            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / _totalStaked;
         } else if(tokenType == 1) {
             ERC20 token = ERC20(configurator.stableToken());
-            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
+            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / _totalStaked;
         } else {
-            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
+            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / _totalStaked;
         }
```

# `EUSDMiningIncentives.sol.rewardPerToken()` : Result of `totalStaled()` should be cached

---

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L163C4-L169C6

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol
163: function rewardPerToken() public view returns (uint256) {
164:        if (totalStaked() == 0) {
165:            return rewardPerTokenStored;
166:        }
167:
168:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();
169:    }
```

```solidity
diff --git a/contracts/lybra/miner/EUSDMiningIncentives.sol b/contracts/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..450e02a 100644
--- a/contracts/lybra/miner/EUSDMiningIncentives.sol
+++ b/contracts/lybra/miner/EUSDMiningIncentives.sol
@@ -161,11 +161,12 @@ contract EUSDMiningIncentives is Ownable {
     }
 
     function rewardPerToken() public view returns (uint256) {
-        if (totalStaked() == 0) {
+         uint256 totalStaked = totalStaked();
+        if (totalStaked == 0) {
             return rewardPerTokenStored;
         }
 
-        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked(
);
+        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked;
     }
```

# **`PeUSDMainnetStableVision.sol.convertToPeUSDAndCrossChain()` : Results of `_msgsender()` should be cached**

---

- [PeUSDMainnetStableVision.sol#L97C3-L105C6](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L97C3-L105C6)

```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
	97: function convertToPeUSDAndCrossChain(
	98:        uint256 eusdAmount,
	99:        uint16 dstChainId,
	100:        bytes32 toAddress,
	101:        LzCallParams calldata callParams
	102:    ) external payable {
	103:        convertToPeUSD(_msgSender(), eusdAmount); //@audit initial call
	104:        sendFrom(_msgSender(), dstChainId, toAddress, eusdAmount, callParams); //@audit 2nd call
	105:    }
```

```diff
diff --git a/contracts/lybra/token/PeUSDMainnetStableVision.sol b/contracts/lybra/token/PeUSDMainnetStableVision.sol
index a1b2c72..ef7f474 100644
--- a/contracts/lybra/token/PeUSDMainnetStableVision.sol
+++ b/contracts/lybra/token/PeUSDMainnetStableVision.sol
@@ -100,8 +100,9 @@ contract PeUSDMainnet is BaseOFTV2, ERC20 {
         bytes32 toAddress,
         LzCallParams calldata callParams
     ) external payable {
-        convertToPeUSD(_msgSender(), eusdAmount);
-        sendFrom(_msgSender(), dstChainId, toAddress, eusdAmount, callParams);
+         address spender = _msgSender();
+        convertToPeUSD(spender, eusdAmount);
+        sendFrom(spender, dstChainId, toAddress, eusdAmount, callParams);
     }
```

# `PeUSDMainnetStableVision.sol.convertToPeUSD` : Results of `_msgsender()` should be cached

---

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L79C5-L89C1

```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
79: function convertToPeUSD(address user, uint256 eusdAmount) public {
80:        require(_msgSender() == user || _msgSender() == address(this), "MDM");
81:        require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
82:        bool success = EUSD.transferFrom(user, address(this), eusdAmount);83:
84:        require(success, "TF");
85:        uint256 share = EUSD.getSharesByMintedEUSD(eusdAmount);
86:        userConvertInfo[user].depositedEUSDShares += share;
87:        userConvertInfo[user].mintedPeUSD += eusdAmount;
88:        _mint(user, eusdAmount);
89:    }
```

```solidity
diff --git a/contracts/lybra/token/PeUSDMainnetStableVision.sol b/contracts/lybra/token/PeUSDMainnetStableVision.sol
index a1b2c72..ebd4f65 100644
--- a/contracts/lybra/token/PeUSDMainnetStableVision.sol
+++ b/contracts/lybra/token/PeUSDMainnetStableVision.sol
@@ -77,7 +77,8 @@ contract PeUSDMainnet is BaseOFTV2, ERC20 {
      * @param eusdAmount The amount of eUSD to deposit and mint PeUSD tokens.
      */
     function convertToPeUSD(address user, uint256 eusdAmount) public {
-        require(_msgSender() == user || _msgSender() == address(this), "MDM");
+      address spender = _msgSender();
+        require(spender == user || spender == address(this), "MDM");
         require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
         bool success = EUSD.transferFrom(user, address(this), eusdAmount);
         require(success, "TF");
          uint256 share = EUSD.getSharesByMintedEUSD(eusdAmount);
      userConvertInfo[user].depositedEUSDShares += share;
        userConvertInfo[user].mintedPeUSD += eusdAmount;
        _mint(user, eusdAmount);
    }
```

# State variables only set in the constructor should be declared `immutable`

---

Avoids a Gsset (**20000 gas**) in the constructor, and replaces the first access in each transaction (Gcoldsload - **2100 gas**) and each access thereafter (Gwarmacces - **100 gas**) with a `PUSH32` (**3 gas**).

While `string`s are not value types, and therefore cannot be `immutable`/`constant` if not hard-coded outside of the constructor, the same behavior can be achieved by making the current contract `abstract` with `virtual` functions for the `string` accessors, and having a child contract override the functions with the hard-coded implementation-specific values.

> **2.1k per var**
> 
- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L52

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol
	52: esLBRBoost = IesLBRBoost(_boost);  //@audit esLRBoost (constructor)
```

# Multiple accesses of a mapping/array should use a local variable cache

---

Caching a mapping's value in a local storage or calldata variable when the value is accessed multiple times saves ~42 gas per access due to not having to perform the same offset calculation every time. **Help the Optimizer by saving a storage variable's reference instead of repeatedly fetching it**

To help the optimizer,declare a storage type variable and use it instead of repeatedly fetching the reference in a map or an array. As an example, instead of repeatedly calling `someMap[someIndex]`, save its reference like this: `SomeStruct storage someStruct = someMap[someIndex]` and use it.

# `LybraGovernance.sol.proposals()` :`proposalData[proposalId] `should be cached in local storage

---

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L120C1-L122C66

```solidity
File: /contracts/lybra/governance/LybraGovernance.sol
	112: function proposals(uint256 proposalId) external view returns (uint256 id, address proposer, uint256 eta, uint256 startBlock, uint256 endBlock, uint256 forVotes, uint256 againstVotes, uint256 abstainVotes, bool canceled, bool executed) {
	113:        id = proposalId;
	114:        eta = proposalEta(proposalId);
	115:        startBlock = proposalSnapshot(proposalId);
	116:        endBlock = proposalDeadline(proposalId);
	117:
	118:        proposer = proposalProposer(proposalId);
	119:        
	120:        forVotes =  proposalData[proposalId].supportVotes[0];
	121:        againstVotes =  proposalData[proposalId].supportVotes[1];
	122:        abstainVotes =  proposalData[proposalId].supportVotes[2];
	123:
	124:        ProposalState currentState = state(proposalId);
	125:        canceled = currentState == ProposalState.Canceled;
	126:        executed = currentState == ProposalState.Executed;
	127:    }
```

```diff
diff --git a/contracts/lybra/governance/LybraGovernance.sol b/contracts/lybra/governance/LybraGovernance.sol
index 7b2d4ad..2230c45 100644
--- a/contracts/lybra/governance/LybraGovernance.sol
+++ b/contracts/lybra/governance/LybraGovernance.sol
@@ -57,6 +57,7 @@ contract LybraGovernance is GovernorTimelockControl {
      * @dev Amount of votes already cast passes the threshold limit.
      */
     function _quorumReached(uint256 proposalId) internal view override returns (bool){
+        
         return proposalData[proposalId].supportVotes[1] + proposalData[proposalId].supportVotes[2] >= quorum(proposalSnapshot(proposalId));
     }
 
@@ -116,10 +117,10 @@ contract LybraGovernance is GovernorTimelockControl {
         endBlock = proposalDeadline(proposalId);
 
         proposer = proposalProposer(proposalId);
-        
-        forVotes =  proposalData[proposalId].supportVotes[0];
-        againstVotes =  proposalData[proposalId].supportVotes[1];
-        abstainVotes =  proposalData[proposalId].supportVotes[2];
+        ProposalExtraData storage proposalExtraData = proposalData[proposalId];
+        forVotes =  proposalExtraData.supportVotes[0];
+        againstVotes =  proposalExtraData.supportVotes[1];
+        abstainVotes =  proposalExtraData.supportVotes[2];
 
         ProposalState currentState = state(proposalId);
         canceled = currentState == ProposalState.Canceled;
```

# `LybraGovernance.sol.getReceipt()` :`proposalData[proposalId] `should be cached in local storage

---

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L129C3-L133C6

```solidity
File: /contracts/lybra/governance/LybraGovernance.sol
	129: function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint256 votes){  
	130:        voted = proposalData[proposalId].receipts[account].hasVoted;
	131:        support = proposalData[proposalId].receipts[account].support;
	132:        votes = proposalData[proposalId].receipts[account].votes;
	133:	    }
```

```diff
@@ -127,9 +128,10 @@ contract LybraGovernance is GovernorTimelockControl {
     }
 
     function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint2
56 votes){  
-        voted = proposalData[proposalId].receipts[account].hasVoted;
-        support = proposalData[proposalId].receipts[account].support;
-        votes = proposalData[proposalId].receipts[account].votes;
+       ProposalExtraData storage proposalExtraData = proposalData[proposalId];
+        voted = proposalExtraData.receipts[account].hasVoted;
+        support = proposalExtraData.receipts[account].support;
+        votes = proposalExtraData.receipts[account].votes;
     }
```

# Internal/Private functions only called once can be inlined to save gas `Gas saved: 20 * 20= 400`

---

Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L234

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
300: function _newFee() internal view returns (uint256) {
301:        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;
302:    }
```

# Using storage instead of memory for structs/arrays saves gas

---

When fetching data from a storage location, assigning the data to a memory variable causes all fields of the struct/array to be read from storage, which incurs a Gcoldsload (2100 gas) for each field of the struct/array. If the fields are read from the new memory variable, they incur an additional MLOAD rather than a cheap stack read. Instead of declearing the variable with the memory keyword, declaring the variable with the storage keyword and caching any fields that need to be re-read in stack variables, will be much cheaper, only incuring the Gcoldsload for the fields actually read. The only time it makes sense to read the whole struct/array into a memory variable, is if the full struct/array is being returned by the function, is being passed to a function that requires memory, or if the array/struct is being read from another memory array/struct

- https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/miner/esLBRBoost.sol#L39C1-L39C1

 **Save 4k gas** 

```solidity
File: /contracts/lybra/miner/esLBRBoost.sol
	39: esLBRLockSetting memory _setting = esLBRLockSettings[id];
```

```diff
-  esLBRLockSetting memory _setting = esLBRLockSettings[id];
+  esLBRLockSetting storage _setting = esLBRLockSettings[id];
```

# A modifier used only once and not being inherited should be inlined to save gas

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L50C14-L50C24

```solidity
File: /contracts/lybra/token/PeUSDMainnetStableVision.sol
50: modifier BurnPaused() {
51:        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
52:        _;
53:    }
```

The above modifier is only being called on [L69](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L69)

- https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83C28-L83C28

```solidity
File: /contracts/lybra/token/EUSD.sol
83: modifier MintPaused() {
84:        require(!configurator.vaultMintPaused(msg.sender), "MPP");
85:        _;
86:    }
```

The above modifier is only being called on [L411](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L411)