# Lybra Finance - Analysis 

|Head |Details|
|:----------------|:------|
| Approach taken in evaluating the codebase | What is unique? How are the existing patterns used? |
|Codebase quality analysis| its structure, readability, maintainability, and adherence to best practices|
|Centralization risks| power, control, or decision-making authority is concentrated in a single entity|
|Bug Fix|process of identifying and resolving issues or errors|
|Gas Optimization|process of reducing the amount of gas consumed by a smart contract or transaction on a blockchain network|
|Other recommendations| Recommendations for improving the quality of your codebase|
|Time spent on analysis | Time detail of individual stages in my code review and analysis |

## Approach taken in evaluating the codebase

Steps:

- ``Use a static code analysis tool``: Static code analysis tools can scan the code for potential bugs and vulnerabilities. These tools can be used to identify a wide range of issues, including:

    - Insecure coding practices
    - Common vulnerabilities
    - Code that is not compliant with security standards

- ``Read the documentation``: The documentation for Lybra Finance should provide a detailed overview of the protocol and its codebase. This documentation can be used to understand the purpose of the code and to identify potential areas of concern.

- ``Scope the analysis``: Once you have a basic understanding of the protocol and its codebase, you can start to scope the analysis. This involves identifying the specific areas of code that you want to focus on. For example, you may want to focus on the code that handles user input, the code that interacts with external APIs, or the code that stores sensitive data.

- ``Manually review the code``: Once you have scoped the analysis, you can start to manually review the code. This involves reading the code line-by-line and looking for potential problems. Some of the things you should look for include:

   - Unvalidated user input
   - Hardcoded credentials
   - Insecure cryptographic functions
   - Unsafe deserialization

- ``Mark vulnerable code parts with @audit tags``: Once you have identified any potential vulnerabilities, you should mark them with @audit tags. This will help you to identify the vulnerable code parts later on.

- ``Dig deep into vulnerable code parts and compare with documentations``:  For each vulnerable code part, you should dig deep to understand how it works and why it is vulnerable. You should also compare the code with the documentation to see if there are any discrepancies.

- ``Perform a series of tests``: Once you have finished reviewing the code, you should perform a series of tests to ensure that it works as intended. These tests should cover a wide range of scenarios, including:

  - Valid and invalid user input
  - Different types of attacks
  - Different operating systems and hardware platforms

- ``Report any problems``:  If you find any problems with the code, you should report them to the developers of Lybra Finance. The developers will then be able to fix the problems and release a new version of the protocol.

## Codebase quality analysis

### LybraPeUSDVaultBase.sol

- The contract does not have explicit access control modifiers, such as "onlyOwner" or "onlyAuthorized," to restrict access to sensitive functions
- The contract lacks comprehensive error handling mechanisms. It does not provide explicit error messages or revert reasons in many cases
-The contract lacks detailed inline comments explaining the purpose and functionality of the code

### LybraEUSDVaultBase.sol

- The contract lacks sufficient comments to explain the purpose and functionality of the code
- The contract uses a mix of different naming conventions for variables, functions, and events
- Some functions and variables have no access modifiers specified, such as totalDepositedAsset, lastReportTime, and poolTotalEUSDCirculation (e.g., public, external, internal, private).
- There are some missing or inadequate error handling mechanisms in the contract. For example, the require statements do not provide specific error messages, which could make it challenging to diagnose issues when a transaction fails
- The contract's event names, such as LiquidationRecord and LSDValueCaptured, do not follow the typical event naming conventions
- Some variables, such as vaultType, feeStored, and lastReportTime, are declared but not used in the contract. Removing these unused variables would enhance code clarity and reduce unnecessary storage costs
- There is some code duplication in functions like liquidation and superLiquidation. Duplicated code increases the risk of errors and makes the contract harder to maintain

### EUSDMiningIncentives.sol

-  Instead of initializing the configurator and esLBRBoost variables in the constructor, consider using constructor initialization syntax. This can improve readability and reduce the number of function calls needed during deployment
- Add input validation checks to functions that require specific conditions to be met. For example, in the setBiddingCost function, ensure that the _biddingRatio parameter is within the allowed range
- When calculating the stakedOf function, consider optimizing the loop by using a for loop with a fixed length instead of a dynamic for loop. This can improve gas efficiency.
- Explicitly specify the visibility modifiers for all functions, including external and internal functions
- Consider using enumerations to represent different states or types within the contract
- Look for opportunities to refactor repetitive code sections into reusable functions or modifiers. For example, the reward calculation logic in the earned function can be extracted into a separate internal function to improve code readability and maintainability.
-  Implement appropriate error handling mechanisms, such as reverting transactions with informative error messages when specific conditions are not met

### LybraConfigurator.sol

- Consider adding visibility specifiers like public, external, or internal to functions and state variables. ``vaultBadCollateralRatio,vaultSafeCollateralRatio,redemptionProvider`` don't have any visibility.
- In the setBadCollateralRatio function, you can add additional checks to validate the newRatio value
- You can combine similar mapping variables like ``vaultMintPaused and vaultBurnPaused`` into a single mapping that stores a struct with both flags

### ProtocolRewardsPool.sol 

- The contract lacks sufficient inline comments to explain the purpose and functionality of the cod
- Some functions, such as getPreUnlockableAmount and getReservedLBRForVesting, contain complex calculations and lack clear explanations or documentation
- Ensuring consistent naming throughout the contract improves code readability and maintainability.
- The contract combines various functionalities, such as staking, claiming rewards, and conversion between different tokens, in a single contract. Breaking down the contract into smaller, modular components can enhance code organization and reusability
- The contract does not provide detailed error messages in some cases, making it challenging to identify the root cause of failures or exceptions

### PeUSDMainnetStableVision.sol

- The contract does not perform sufficient input validation for certain parameters, such as the eusdAmount in the convertToPeUSD function
- The code uses require statements to check conditions and revert transactions when the conditions are not met. However, the error messages provided in the require statements are not descriptive
- The contract's constructor initializes several variables and requires the input of _config, _sharedDecimals, and _lzEndpoint. It is crucial to ensure that these parameters are correctly set during contract deployment

##

## Centralization risks

A single point of failure is not acceptable for this project Centrality risk is high in the project, the role of 'onlyOwner' detailed below has very critical and important powers

Project and funds may be compromised by a malicious or stolen private key ``onlyOwner`` ``msg.sender``

```solidity
FILE: 2023-06-lybra/contracts/lybra/miner/EUSDMiningIncentives.sol

84: function setToken(address _lbr, address _eslbr) external onlyOwner {
89: function setLBROracle(address _lbrOracle) external onlyOwner {
93: function setPools(address[] memory _pools) external onlyOwner {
100:function setBiddingCost(uint256 _biddingRatio) external onlyOwner {
105:function setExtraRatio(uint256 ratio) external onlyOwner {
110: function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
115: function setBoost(address _boost) external onlyOwner {
119: function setRewardsDuration(uint256 _duration) external onlyOwner {
124: function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {
128: function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {


FILE: 2023-06-lybra/contracts/lybra/miner/ProtocolRewardsPool.sol

52:  function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {
58:  function setGrabCost(uint256 _ratio) external onlyOwner {

FILE: Breadcrumbs2023-06-lybra/contracts/lybra/miner/stakerewardV2pool.sol

121: function setRewardsDuration(uint256 _duration) external onlyOwner {
127: function setBoost(address _boost) external onlyOwner {
132: function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {

```

## Bug Fix

1. `` Does it use a timelock function?:  True`` timeclock functions not implemented as per documentations 

2. Potential reentrancy vulnerability the code includes external contract calls, such as EUSD.transfer and peUSD.transfer. If these contracts are not implemented securely and follow the ``checks-effects-interactions`` pattern, there may be a risk of ``reentrancy attacks``.

3.  ``deposit`` and ``withdraw``, are not properly checked for potential integer overflow or underflow. 

4. Should disable the ``renownceOwnerhip()`` when ever we use ``Ownable``. Its possible ``onlyOwner`` can ``renounceOwner`` in unexpected way. So contracts become deep trouble without ``owner``  

5. The ``withdraw`` function allows the sender to specify an ``arbitrary address`` to send the funds. This design could be ``vulnerable to ``denial-of-service`` scenario by consuming ``excessive gas`` during the withdrawal process.

6. Consider adding ``event`` logging to important contract functions, such as ``deposit and withdraw`` important state changes 

7. The ``setBiddingCost`` function should include a check to ensure that the `` _biddingRatio`` is within a valid range. 

8. The contract includes some external function calls (e.g., EUSD.transferFrom, configurator.distributeRewards), but it does not handle potential exceptions that could arise from these calls.

9. Some state variables, such as redemptionFee, flashloanFee, and maxStableRatio, are directly modifiable by privileged roles without any additional checks or restrictions.

10. The code does not include mechanisms to prevent or mitigate DoS attacks, such as gas limit restrictions, rate limiting, or circuit breakers. Malicious actors could potentially exploit vulnerable areas in the code to consume excessive gas, leading to DoS attacks.

11. In the ``notifyRewardAmount`` function, when calculating the ``rewardPerTokenStored``, there could be precision loss when performing division operations. The code should handle the decimal places appropriately and ensure that precision loss does not occur, especially when dealing with token amounts or ratios

## Gas Optimization

- Some calculations, especially in the ``getPreUnlockableAmount`` function, involve multiple operations that may consume a significant amount of gas. Optimizing these calculations can improve the contract's gas efficiency and reduce transaction costs

- ``Review data types``: Analyze the data types used in your smart contracts and consider if they can be further optimized. For example, changing uint256 to uint128 or uint94 can save gas and storage slots

- ``Struct packing`` : Look for opportunities to pack structs into fewer storage slots. By carefully selecting appropriate data types for struct members, you can reduce the overall storage usage.

- ``Use constant values`` : If certain values in your contracts are constant and do not change, declare them as constants rather than storing them as state variables. This can significantly save gas costs.

- ``Avoid unnecessary storage`` : Examine your code and eliminate any unnecessary storage of variables or addresses that are not required for contract functionality.

- ``Storage vs. memory usage`` : When working with arrays or structs, consider whether using storage instead of memory can save gas. Using storage allows direct access to the state variables and avoids unnecessary copying of data.

- replacing the use of ``memory`` with ``calldata`` for read-only arguments in ``external functions``

##

## Other recommendations

- Regular code reviews and adherence to best practices
- Conduct external audits by security experts
- Consider open sourcing the contract for community review
- Maintain comprehensive security documentation
- Establish a responsible disclosure policy for vulnerabilities
- Implement continuous monitoring for unusual activity
- Educate users about risks and best practices

## Time spent on analysis 

15 Hours




### Time spent:
15 hours