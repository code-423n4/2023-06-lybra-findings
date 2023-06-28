https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L98C5-L102C1

  function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){

        return esLBR.getPastVotes(account, timepoint);
     }

The parameter "bytes memory" is unused in the function '_getVotes' and has no effect on the logic.