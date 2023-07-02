## [L-1] Refactor `_countVote` function to remove the unnamed parameter.

The last parameter of `_countVote` function in LibraGovernance.sol is unnamed.
Refactor code to remove unnamed parameter.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L76

```
function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {
      
        require(state(proposalId) == ProposalState.Active, "GovernorBravo::castVoteInternal: voting is closed");
        require(support <= 2, "GovernorBravo::castVoteInternal: invalid vote type");
        ProposalExtraData storage proposalExtraData = proposalData[proposalId];
        Receipt storage receipt = proposalExtraData.receipts[account];
        require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");
        
        proposalExtraData.supportVotes[support] += weight;
       

        receipt.hasVoted = true;
        receipt.support = support;
        receipt.votes = weight;
        proposalExtraData.totalVotes += weight;
        
    }
```

## [L-2] Refactor code to remove commented variable
The `proposalId` parameter of the `_execute` function in LibraGovernance.sol is commented out. Refactor code to remove commented parameter.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L106
```
function _execute(uint256 /* proposalId */,  address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {
        require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");
        super._execute(1, targets, values, calldatas, descriptionHash);
        // _timelock.executeBatch{value: msg.value}(targets, values, calldatas, 0, descriptionHash);//@audit commented code.
    }
```

## [L-3] Remove commented code
Remove commented code.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L109

```
 function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {
        require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");
        super._execute(1, targets, values, calldatas, descriptionHash);
        // _timelock.executeBatch{value: msg.value}(targets, values, calldatas, 0, descriptionHash);
    }

```
## [L-4] Do not hardcode address since contracts will be deployed to multiple chains.

The `stakedLBRLpValue` function in the EUSDMinningIncentives.sol uses a hardcoded address. Do not hardcode addresses since the contract will be deployed to multiple network.

File: https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L153

```
function stakedLBRLpValue(address user) public view returns (uint256) {
        uint256 totalLp = IEUSD(ethlbrLpToken).totalSupply();
        (, int etherPrice, , , ) = etherPriceFeed.latestRoundData();
        (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
        uint256 etherInLp = (IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken) * uint(etherPrice)) / 1e8;
        uint256 lbrInLp = (IEUSD(LBR).balanceOf(ethlbrLpToken) * uint(lbrPrice)) / 1e8;
        uint256 userStaked = IEUSD(ethlbrStakePool).balanceOf(user);
        return (userStaked * (lbrInLp + etherInLp)) / totalLp;
    }
```

## [L-5] Variable Shadowing in the constructors of the listed files below.
The `decimals` local variable shadows the ERC20.decimals state variable of the parent contract.

Use another local variable name in order to avoid variable shadowing.
Files: 
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L20
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSD.sol#L12
- https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L58
- 

```
constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint)
        ERC20("LBR", "LBR")
        BaseOFTV2(_sharedDecimals, _lzEndpoint)
    {
        configurator = Iconfigurator(_config);
        uint8 decimals = decimals(); //@audit shadowed variables - check for further attack.
        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");
        ld2sdRatio = 10 ** (decimals - _sharedDecimals);
    }
```



## [NC-1] Use a more recent version of solidity
Use a solidity version of at least 0.8.19 as latest stable versions will bug fixes and security updates.
Files: most files.