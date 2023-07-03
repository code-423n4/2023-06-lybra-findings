# My approach taken in evaluating the codebase
I mainly focus on voting feature by analyzing how things work from
creating proposal to executing it. I also take deep look into how configurator
module works.

Lybra protocol takes extensive uses of Openzeppelin's libraries like `Governor`,
`TimelockController`, etc,...So I read these libraries carefully, test it
myself and see how the protocol uses these libraries, as this often causes
problems.

It takes quite sometime to set up the tests and understand how things work
because there are no tests and the document is not that detailed.

# About Lybra protocol architecture
I think the architecture is already properly designed. The protocol takes
extensive use of openzeppelin's libraries, which is a good idea comparing to
implementing them yourselves. The vulnerabilities I found usually involves
proper usage of these libraries.

# Access control risks
I decided to set aside a separate part to talk about Lybra protocols'
access control risk; because this is the part where I spent the most times
working on. This involves understanding Lybra's `LybraGovernance`, `LybraConfigurator`
modules and also openzeppelin's `Governor`, `TimelockController`,...
-   First of all, although the protocol is designed to be `governed by esLBR holders` through
    proposal process (create proposal, vote, queue, execute,..), there are many users having
    administration privileges:
    + In `GovernanceTimelock`:
        ```solidity
        _setRoleAdmin(DAO, GOV);
        _setRoleAdmin(TIMELOCK, GOV);
        _setRoleAdmin(ADMIN, GOV);
        _grantRole(DAO, address(this));
        _grantRole(DAO, msg.sender);
        _grantRole(GOV, msg.sender);
        ```
    the deployer `msg.sender` automatically have all the rights to grant/revoke
    `TIMELOCK`, `ADMIN`, `DAO` role (users with `DAO` role can directly call
    functions in `Configurator` and make changes without going through voting process).
    
    + The `proposers`,`executors` and `admin` in `GovernanceTimelock`'s constructor
      poses new threats as is commented in openzeppelin's `GovernorTimelockControl` contract:
    ```
    WARNING: Setting up the TimelockController to have additional proposers besides the governor is very risky, as it
    grants them powers that they must be trusted or known not to use: 1) {onlyGovernance} functions like {relay} are
    available to them through the timelock, and 2) approved governance proposals can be blocked by them, effectively
    executing a Denial of Service attack. This risk will be mitigated in a future release.
    ```
    So it's a good idea to set these `proposers` and `executors` to be empty
    list when initializing the `GovernanceTimelock` contract.
- As I stated in a submitted issue,  normal user cannot execute succeeded
    proposal, which is a very basic feature of community governed protocol
- The protocol cannot work without some additional access control setup; for example, contract
    `LybraGovernance` must be granted `PROPOSER_ROLE` and `EXECUTOR_ROLE` because it is the
  one who directly calls functions `schedule` or `execute` in timelock. These
    are missing from the code


### Time spent:
30 hours