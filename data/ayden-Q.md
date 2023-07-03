
1.The result of function getPreUnlockableAmount should be cached into local variable instead of double calculate.
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#128#L144

```solidity
    function unlockPrematurely() external {
        require(
            block.timestamp + exitCycle - 3 days >
                time2fullRedemption[msg.sender],
            "ENW"
        );
+       uint256 preUnlockableAmount = getPreUnlockableAmount(msg.sender);
-       uint256 burnAmount = getReservedLBRForVesting(msg.sender) -
-            getPreUnlockableAmount(msg.sender);
-       uint256 amount = getPreUnlockableAmount(msg.sender) +
            getClaimAbleLBR(msg.sender);
+       uint256 burnAmount = getReservedLBRForVesting(msg.sender) + preUnlockableAmount;
+       uint256 amount = preUnlockableAmount - getClaimAbleLBR(msg.sender);
        if (amount > 0) {
            LBR.mint(msg.sender, amount);
        }
        unstakeRatio[msg.sender] = 0;
        time2fullRedemption[msg.sender] = 0;
        grabableAmount += burnAmount;
    }
```

2.LBR.burn will lost precision when amount is small

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156

```solidity
    function testGrabEsLBR() public {
        vm.startPrank(Alice);
        assertEq(lbr.balanceOf(Alice), 1e18);

        for (uint256 i = 0; i < 10_000; i++) {
            pool.grabEsLBR(3);
        }

        assertEq(lbr.balanceOf(Alice), 1e18);
        assertEq(eslbr.balanceOf(Alice), 30_000);
    }
```

3.Change `x <= y ? x : y` to `x < y ? x : y` save 3 gas.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L147#L150

4.`unlockPrematurely` should ensure user `time2fullRedemption` > block.timestamp or this function will revert.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L129#L145

```solidity
    function unlockPrematurely() external {
+       require(time2fullRedemption[msg.sender]> block.timestamp);
        require(
            block.timestamp + exitCycle - 3 days >
                time2fullRedemption[msg.sender],
            "ENW"
        );
        uint256 burnAmount = getReservedLBRForVesting(msg.sender) -
            getPreUnlockableAmount(msg.sender);
        uint256 amount = getPreUnlockableAmount(msg.sender) +
            getClaimAbleLBR(msg.sender);
        if (amount > 0) {
            LBR.mint(msg.sender, amount);
        }
        unstakeRatio[msg.sender] = 0;
        time2fullRedemption[msg.sender] = 0;
        grabableAmount += burnAmount;
    }
```

5.The "unlockPrematurely" method and "grabEsLBR" should include event logging of the user, amount, and timestamp
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L129#L145

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L153#L158