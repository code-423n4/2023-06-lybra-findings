# 1. && gate should be replace with two if statement to save gas
       2 instances of the issue present

   PeUSD.sol - https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol?plain=1#L46-L50

   LBR.sol - https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol?plain=1#L64

# 2. _setting.duration should be cached in local variable to save gas

esLBRBoost.sol -  https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol?plain=1#L42-L45

# 3. add constant for gas saving
LBR.sol - https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol?plain=1#L15

# 4. function circulatingSupply() and token() function  should be done  external instead of public to save gas and to reduce security vulnerability

PeUSDMainnetStableVision.sol - https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol?plain=1#L171-L177

# 5. In LybraGovernance.sol , some functions like votingPeriod() , votingDelay(), CLOCK_MODE(), proposalThreshold() functions are made public but they are not used in any external contract so it should be made internal to save gas and to prevent security vulnerability.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol
