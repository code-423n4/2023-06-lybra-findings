**Table of contents**

- [The result of a function call should be cached rather than re-calling the function](#The-result-of-a-function-call-should-be-cached-rather-than-re-calling-the-function)
    - [ProtocolRewardsPool.sol.notifyRewardAmount() : **Results of totalStaked() should be cached**](#ProtocolRewardsPool.sol.notifyRewardAmount()-:-**Results-of-totalStaked()-should-be-cached)
    - **[PeUSDMainnetStableVision.sol.convertToPeUSDAndCrossChain() : Results of _msgsender() should be cached](#PeUSDMainnetStableVision.sol.convertToPeUSDAndCrossChain()-:-Results-of-_msgsender()-should-be-cached)**

### The result of a function call should be cached rather than re-calling the function

External calls are expensive. Consider caching the following:

### ProtocolRewardsPool.sol.notifyRewardAmount() : **Results of totalStaked() should be cached**

In Solidity, caching repeated function calls can be an effective way to optimize gas usage, especially when the function is called frequently with the same arguments

- [ProtocolRewardsPool.sol#L230](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L230)

```solidity
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

### **PeUSDMainnetStableVision.sol.convertToPeUSDAndCrossChain() : Results of _msgsender() should be cached**

- [PeUSDMainnetStableVision.sol#L97C3-L105C6](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L97C3-L105C6)

```solidity
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