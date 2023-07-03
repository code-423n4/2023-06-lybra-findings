[L]  Timelock_ parameter is not assigned to the timelock variable

In contract LybraGovernor, the timelock_ parameter is not assigned to the timelock variable. 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L38

     constructor(string memory name_, TimelockController timelock_, address _esLBR) GovernorTimelockControl(timelock_)  Governor(name_) {
        // timelock = timelock_;
        esLBR = IesLBR(_esLBR);
        GovernanceTimelock = IGovernanceTimelock(address(timelock_));
    }

This means that the timelock_ parameter is not used in the contract, and the contract will not be able to use the timelock parameter to control proposals.

    Mitigation:

    constructor(address timelock_) public {
        timelock = timelock_;
    }

It's important to ensure that the timelock_ parameter is assigned to the timelock variable in the GovernorTimelockControl contract to enable time-based control over proposals.