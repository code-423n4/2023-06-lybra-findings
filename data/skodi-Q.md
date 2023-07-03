# [01] missing require check

below `grabEsLBR` function dosent check if the amount inputted as parameter is greater than the available grabableAmount which might break the code at this part `grabableAmount -= amount` with a silent revert
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L131
```solidity
function grabEsLBR(uint256 amount) external { 
        require(amount > 0, "QMG");
        grabableAmount -= amount;
        LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);
        esLBR.mint(msg.sender, amount);
    }
```

## Recommendation

add a if/require statement
```solidity
- require(amount > 0, "QMG");
+ require(amount>0 && amount<=grabableAmount, "QMG");
``` 


# [02] Lacking Validation Of Oracle Queries
the function `stakedLBRValue` is missing additional validations to ensure that the round is complete and has returned a valid/expected price
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L149
```solidity
function stakedLBRLpValue(address user) public view returns (uint256) {
        uint256 totalLp = IEUSD(ethlbrLpToken).totalSupply();
        (, int etherPrice, , , ) = etherPriceFeed.latestRoundData();  //@audit no checks
        (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
        uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;
        uint256 lbrInLp = (IEUSD(LBR).balanceOf(ethlbrLpToken) * uint(lbrPrice)) / 1e8;
        uint256 userStaked = IEUSD(ethlbrStakePool).balanceOf(user);
        return (userStaked * (lbrInLp + etherInLp)) / totalLp;
    }
```
## Recommendation
Consider validating the other outputs of `latestRoundData()` such as 
```
(
        uint80 roundID,
        int256 price,
        ,
        uint256 updateTime,
        uint80 answeredInRound
      )
```


# [03] CENTRALIZATION RISK -- owner can delay the reward claiming duration
since the only possible check is that `require(amount > 0, "amount = 0");` owner can update the reward amount with very low as such as 1 gwei and still extend the duration making users unable to claim their full reward amount
```solidity
 function notifyRewardAmount(
        uint256 amount
    ) external onlyOwner updateReward(address(0)) {
        require(amount > 0, "amount = 0");
        if (block.timestamp >= finishAt) {
            rewardRatio = amount / duration;
        } else {
            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
            rewardRatio = (amount + remainingRewards) / duration;
        }

        require(rewardRatio > 0, "reward ratio = 0");

        finishAt = block.timestamp + duration;
        updatedAt = block.timestamp;
        emit NotifyRewardChanged(amount, block.timestamp);  
    }
```
## Recommendation
although considering the owner is a trusted party its better to use a conditional statement such that `notifyRewardAmount` cant be updated unless the amount provided is grater than certain threshold
```solidity
- require(amount > 0, "amount = 0");
+ require(amount > 0 && amount>= thresholdAmount, "amount = 0");
```