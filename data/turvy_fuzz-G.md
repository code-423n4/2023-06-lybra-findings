## cache storage variable peUSDExtraRatio before looping to avoid SLOAD(100) opcode in every loop
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L142

## store value of proposalData[proposalId] to memory in multiple calls
cache proposalData[proposalId] e.g ProposalExtraData memory proposalExtraData = proposalData[proposalId]
i.e unoptimized 
```solidity
 forVotes =  proposalData[proposalId].supportVotes[0];
        againstVotes =  proposalData[proposalId].supportVotes[1];
        abstainVotes =  proposalData[proposalId].supportVotes[2];
```
optimized
```solidity
ProposalExtraData memory proposalExtraData = proposalData[proposalId]
 forVotes =  proposalExtraData.supportVotes[0];
        againstVotes =  proposalExtraData.supportVotes[1];
        abstainVotes =  proposalExtraData.supportVotes[2];
```
Multiple Instance:
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L60
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L67
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L120
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L121
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L122

## To save repeated SLOAD(100) gas cost, cache result of time2fullRedemption[user] and lastWithdrawTime[user]
3 instance of this:
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L92
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L156
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L162

## save result of reward - eUSDShare rather than recalculating repeatedly
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L201

## rewardPerToken() is executed twice in the same single transaction
In the modifier *updateReward()* , rewardPerToken() is called twice
first here:
```solidity
rewardPerTokenStored = rewardPerToken();
```
and still called in the *earned()* function
```solidity
modifier updateReward(address _account) {
        rewardPerTokenStored = rewardPerToken();
        updatedAt = lastTimeRewardApplicable();

        if (_account != address(0)) {
            rewards[_account] = earned(_account);
            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
            userUpdatedAt[_account] = block.timestamp;
        }
        _;
    }
```
```solidity
function earned(address _account) public view returns (uint256) {
        return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
    }
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L56

## cache storage variable totalsupply
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L75