### [QA-01] Lack of event emiting
**Summary**
The contract emits some events, but not all state-changing operations are covered. For example, there is no event emitted when LybraStETHDepositVault.sol#setLidoRebaseTime() lidoRebaseTime is changed, which would be useful for tracking changes.

**Instances**
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L25-L28
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L41-L44


### [QA-02] Possible reentrancy in StakingRewardsV2.sol#stake()
**Summary**
Possible reentrancy in StakingRewardsV2.sol#stake() if stakingToken is controlled by untrusted person (attacker). Use nonReentrant modifier.

**Instances**
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L83-L90

**Impact**
Reentrancy vulnerabilities can lead to various critical outcomes such as token stealing and burning. Adversaries may exploit the bug to mint tokens without any limitations or extract all the tokens out of the contract.

**Recommended Mitigation Steps**
Introduce a modifier nonreentrant to prevent Reentrancy vulnerabilities by implementing a Check-Effects-Interactions pattern.


### [QA-03] Possible revert of LybraEUSDVaultBase.sol#liquidation()
**Summary**
In LybraEUSDVaultBase.sol#liquidation() `reducedAsset` used for transfering collateral assets to msg.sender can be zero, because assetAmount function parameter is never checked if it is zero. The problem is that some ERC20 tokens doesn’t allow zero amount transfer.

**Impact**
The whole liquidation() function will revert.

**Instances**
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L154-L176


### [QA-04] Unchecked External Calls in LybraConfigurator.sol#distributeRewards()
**Summary**
External calls can fail due to the called contract throwing an error or running out of gas. In the LybraConfigurator.sol#distributeRewards() function, there are external calls to transfer and notifyRewardAmount functions without checking the return value. If these calls fail, the contract will not be aware, which could lead to unexpected states.

**Impact**
The whole distributeRewards() function will revert.

**Recommended Mitigation Steps**
Check the return value of external calls.


### [QA-05] Fallback Function Exploits in LybraConfigurator.sol
**Summary**
This contract does not seem to have a fallback function, but if it interacts with contracts that do, those could be exploited. Fallback functions with a higher gas cost could potentially be used for DoS attacks.