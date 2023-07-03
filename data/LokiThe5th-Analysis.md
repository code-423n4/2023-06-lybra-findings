## Analysis: Lybra Finance  

### Type of analysis  
This analysis is intended as a "Basic" level analysis, although an attempt was made to answer "Advanced" level questions where possible.   

### Auditor Learning Points from Codebase  

1. Tests and up-to-date documentation is crucial for any protocol undergoing an audit. The absence of the aforementioned created various difficulties which slows down auditing. That being said, circumstances are rarely ideal and this was a good chance to develop and test out approaches to auditing a codebase without tests.  

2. Valuable lessons were learnt in time-management for the auditing process, as well as inspecting code on a "per function" basis. This was valuable for instances where internal functions were implemented but not inherited from.  

### Audit Approach  
The approach for this review was manual review. For some functions tests were created in Foundry to stress the intended functionality of the code.  

As the auditor's time was extremely limited the goal was to attempt to audit at least the `governance` folder of the contracts.  

### Time Spent  
Six hours were available to audit the repo and do PoCs/write-ups.  

### Analysis of the Codebase  
The `AdminTimeLock.sol` contract, which inherits the `"@openzeppelin/contracts/governance/TimelockController.sol"`, is used for governance functionality within the derived `LybraGovernance` contract. 

Some concerns with this is that the `LybraGovernance.sol` contract implements internal functions `_quorumReached`, `_voteSucceeded` and `_countVote` with logic that contradicts the intended versatility of the implementation. In addition, these functions are not marked as virtual, which would have made it easier for Lybra to achieve the intended generic versatile governance functionality.  

Another concern is that in `LybraREthVault.sol` an interface, `IRETH` is defined and is intended to serve as the interface with the Rocket Pool Eth contract. Unfortunately the interface defines the function as `getExchangeRatio` instead of `getExchangeRate()`, which means any calls to `depositEtherToMint` will fail, essentially bricking the contract.  

### Other Recommendations

In general, and with the greatest respect to the developers as this is a complicated project, the codebase needs to be thoroughly tested by the development team before it is resubmitted for an audit (*this might be a great case for Code4rena's test services*). 

From a quick overview there were coding errors which should have been caught during the development process. The team might also benefit from following some generally accepted development processes such as Test Driven Development and/or implement some automated checks within a CI/CD pipeline.

The amount of fixes that the codebase requires will likely result in the repo needing to undergo a second full audit, as the changes will inevitably introduce other bugs into the code.  

On a personal note: I respect the developers for the challenging protocol that they are building. There will be some critisim about the standards of the code submitted, but this is what audits are for and I look forward to see what the future holds for the team and their project.

### Time spent:
6 hours