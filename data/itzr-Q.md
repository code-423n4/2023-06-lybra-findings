
# Issue 1 

## `TIMELOCK` role is not assigned to any address/account

In `GovernanceTimelock::constructor`, roles and role admins are granted, yet the `TIMELOCK` role is not assigned to any address.

```
constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {
   _setRoleAdmin(DAO, GOV);
   _setRoleAdmin(TIMELOCK, GOV);
   _setRoleAdmin(ADMIN, GOV);
   _grantRole(DAO, address(this));
   _grantRole(DAO, msg.sender);
   _grantRole(GOV, msg.sender);
}
```
Failure to grant the `TIMELOCK` role to any address means that the protocol is not upgradable via DAO Governance, since `LybraGovernance::_execute` requires: `(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");`

This is not a critical bug since the team can grant roles at a future date, however on deployment `LybraGovernance::_execute` will not run.

It is recommended to grant the address of `AdminTimelock` with the `TIMELOCK` role within the `GovernanceTimelock::constructor` and open source the deployment script.

# Issue 2 

## Both `msg.sender` and `address(this)` control the DAO

The v2 docs claim that `The Lybra protocol is governed by esLBR token holders`, and in `LybraConfigurator` it's stated that the `DAO` is `A time-locked contract initiated by esLBR voting, with a minimum effective period of 14 days. After the vote is passed, only the developer can execute the action`. However, in `GovernanceTimelock::constructor`, `msg.sender` is assigned with the `DAO` role and given full administrative rights over role assignment via `GOV`. So long as this is the case, the protocol remains 100% in the control of the core team, not the DAO.


```
constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {
   _setRoleAdmin(DAO, GOV);
   _setRoleAdmin(TIMELOCK, GOV);
   _setRoleAdmin(ADMIN, GOV);
   _grantRole(DAO, address(this));
   _grantRole(DAO, msg.sender);
   _grantRole(GOV, msg.sender);
}
```

Assuming the docs are reflective of the teams intentions, it would be more accurate to decouple core team / admin privileges from that of the DAOs i.e. assign `msg.sender` as `ADMIN` and `address(this)` as `DAO` in `GovernanceTimelock::constructor`, and then rewrite the governance process such that the `DAO` can propose changes, and then only `ADMIN` can execute them.

# Issue 3

## `GovernanceTimelock::checkRole` is not an accurate reflection of what the function does.

In `GovernanceTimelock` there are two ways of checking for roles:

```
function checkRole(bytes32 role, address _sender) public view  returns(bool){
  return hasRole(role, _sender) || hasRole(DAO, _sender);
}

function checkOnlyRole(bytes32 role, address _sender) public view  returns(bool){
  return hasRole(role, _sender);
}
``` 

This is code is obfuscating since `checkOnlyRole` does what `checkRole` should do.

Two options for mitigation: 

1. Rename `checkOnlyRole` to `checkRole` and rename `checkRole` to `checkRoleOrDAORole`

2. Where `checkRole` is used in the code base, e.g. `checkRole(TIMELOCK)`, use `checkOnlyRole(TIMELOCK)` and  `checkOnlyRole(DAO)` instead.
