1. In GovernanceTimelock.sol , The checkOnlyRole function is redundant, as it is already implemented by the checkRole function and it can be removed without affecting its functionality(in line 29 to 31 of contract).

here is the link - https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol




