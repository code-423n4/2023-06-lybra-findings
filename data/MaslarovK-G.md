https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/AdminTimelock.sol#L8

proposers and executors may be switched from memory to callData in order to save gas.
__________________________________________________________________________________________________________________________________________________________________________
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L15

proposers and executors may be switched from memory to callData in order to save gas.
______________________________________________________________________________________________________
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L106

targets, values and callDatas may be switched from memory to callData  in order to save gas.
_____________________________________________________________________
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L43
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L47
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L51

Modifiers can be changed to internal functions in order to save gas.
_____________________________________________________________________
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L33

Mappings can be put into a struct in order to save gas.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L41

Can be made immutable in order to save gas.
_____________________________________________________________________
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L88
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L84
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L80

Modifiers can be changed to internal functions in order to save gas.
_____________________________________________________________________
