## cache storage variable peUSDExtraRatio before looping to avoid SLOAD(100) opcode in every loop
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L142

## cache proposalData[proposalId] in multiple calls
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