# [N-01] Move IF/validation statements to the top of the function when validating input parameters
````solidity
File: contracts/lybra/pools/LybraRETHVault.sol

27    function depositEtherToMint(uint256 mintAmount) external payable override {
28            require(msg.value >= 1 ether, "DNL");
29            uint256 preBalance = collateralAsset.balanceOf(address(this));
30            rkPool.deposit{value: msg.value}();
31            uint256 balance = collateralAsset.balanceOf(address(this));
32            depositedAsset[msg.sender] += balance - preBalance;
33    
34:           if (mintAmount > 0) {
35:               _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
36:           }
````
[LybraRETHVault.sol#L27-L39](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L27-L39)       

Same goes for:        
[LybraStETHVault.sol#L47-L49](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L47-L49), [LybraWbETHVault.sol#L27-L29](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L27-L29), [LybraWstETHVault.sol#L41-L43](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L41-L43), [LybraEUSDVaultBase.sol#L82-L84](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L82-L84), [LybraPeUSDVaultBase.sol#L65-L68](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L65-L68).
````solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
212    function _withdraw(address _provider, address _onBehalfOf, uint256 _amount) internal {
213            require(depositedAsset[_provider] >= _amount, "Withdraw amount exceeds deposited amount.");
214            depositedAsset[_provider] -= _amount;
215            collateralAsset.transfer(_onBehalfOf, _amount);
216:           if (getBorrowedOf(_provider) > 0) {
217:               _checkHealth(_provider, getAssetPrice());
218:           }
````
[LybraPeUSDVaultBase.sol#L212-L220](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L212-L220)
# [N-02] FOR FUNCTIONS, FOLLOW SOLIDITY STANDARD NAMING CONVENTIONS
Internal and private functions: the mixedCase format starting with an underscore (_mixedCase starting with an underscore).        
- [EUSDMiningIncentives.sol#L132](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L132)
- [ProtocolRewardsPool.sol#L64](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L64)
- [ProtocolRewardsPool.sol#L69](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L69)
- [LybraEUSDVaultBase.sol#L114](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L114)
# [N-03] NatSpec comments should be increased in contracts
Context: All Contracts
## Description
It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). It is clearly stated in the Solidity official documentation.         
        
In complex projects such as Defi, the interpretation of all functions and their arguments and returns is important for code readability and auditability.         
[https://docs.soliditylang.org/en/v0.8.15/natspec-format.html](https://docs.soliditylang.org/en/v0.8.15/natspec-format.html)
# [N-04] Some require descriptions are not clear
Some require error descriptions below return results like LNA, BCE, TF.         
         
1. It does not comply with the general require error description model of the project (Either all of them should be debugged in this way, or all of them should be explained with a string not exceeding 32 bytes.)
2. For debug dapps like Tenderly, these debug messages are important, this allows the user to see the reasons for revert practically.
38 results - 13 files
````solidity
File: contracts\lybra\configuration\LybraConfigurator.sol
  127:         require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");

File: contracts\lybra\miner\EUSDMiningIncentives.sol
  101:         require(_biddingRatio <= 8000, "BCE");
  106:         require(ratio <= 1e20, "BCE");
  111:         require(ratio <= 1e20, "BCE");
  215:         require(success, "TF");

File: contracts\lybra\miner\ProtocolRewardsPool.sol
  59:         require(_ratio <= 8000, "BCE");
  114:        require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");
  132:        require(amount > 0, "QMG");

File: contracts\lybra\miner\stakerewardV2pool.sol
  86:         require(success, "TF");

File: contracts\lybra\pools\LybraRETHVault.sol
  28:         require(msg.value >= 1 ether, "DNL");

File: contracts\lybra\pools\LybraStETHVault.sol
  38:         require(msg.value >= 1 ether, "DNL");
  71:         require(success, "TF");
  86:         require(success, "TF");

File: contracts\lybra\pools\LybraWbETHVault.sol
  21:         require(msg.value >= 1 ether, "DNL");

File: contracts\lybra\pools\LybraWstETHVault.sol
  30:         require(msg.value >= 1 ether, "DNL");

File: contracts\lybra\pools\base\LybraEUSDVaultBase.sol
  76:         require(success, "TF");
  99:         require(onBehalfOf != address(0), "TZA");
  260:        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");

File: contracts\lybra\pools\base\LybraPeUSDVaultBase.sol
  83:         require(onBehalfOf != address(0), "TZA");
  84:         require(amount > 0, "ZA");
  97:         require(onBehalfOf != address(0), "TZA");
  98:         require(amount > 0, "ZA");
  111:        require(onBehalfOf != address(0), "TZA");
  112:        require(amount > 0, "ZA");
  174:        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL"); 

File: contracts\lybra\token\EUSD.sol
  80:         require(configurator.mintVault(msg.sender), "RCP");
  84:         require(!configurator.vaultMintPaused(msg.sender), "MPP");
  88:         require(!configurator.vaultBurnPaused(msg.sender), "BPP");

File: contracts\lybra\token\LBR.sol
  21:         require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");

File: contracts\lybra\token\PeUSDMainnetStableVision.sol
  43:         require(configurator.mintVault(msg.sender), "RCP");
  47:         require(!configurator.vaultMintPaused(msg.sender), "MPP");
  51:         require(!configurator.vaultBurnPaused(msg.sender), "BPP");
  64:         require(to != address(0), "TZA");
  80:         require(_msgSender() == user || _msgSender() == address(this), "MDM");
  81:         require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");
  83:         require(success, "TF");
  115:         require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
  134:         require(success, "TF");
````
# [N-05] Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10**18)
While the compiler knows to optimize away the exponentiation, it’s still better coding practice to use idioms that do not require compiler optimization, if they exist.
         
There is 3 instances of this issue:
````solidity
File: contracts\lybra\token\LBR.sol
  22:         ld2sdRatio = 10 ** (decimals - _sharedDecimals);//@audit-issue scient
````
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L22](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L22)
````solidity
File: contracts\lybra\token\PeUSD.sol
  14:         ld2sdRatio = 10 ** (decimals - _sharedDecimals);//@audit-issue scient
````
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L14](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L14)
````solidity
File: contracts\lybra\token\PeUSDMainnetStableVision.sol
  60:         ld2sdRatio = 10 ** (decimals - _sharedDecimals);//@audit-issue scient
````
[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L60](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L60)
# [N-06] Return values of approve() not checked
Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything
          
There are 2 instances of this issue.
````solidity
File: contracts\lybra\configuration\LybraConfigurator.sol
  302:                 EUSD.approve(address(curvePool), balance);

File: contracts\lybra\pools\LybraWstETHVault.sol
  35:                 lido.approve(address(collateralAsset), msg.value);
````
# [N-07] Remove SafeMath library
In-scope contracts use the SafMath library for simple addition, subtraction, multiplication and division.
       
This causes an unnecessary overhead since beginning from Solidity version 0.8.0 arithemtic operations revert by default on overflow / underflow.
       
By looking into the implementation of the SafeMath library you can also see that it is just a wrapper around the basic arithmatic operations and does not add any checks (at least for the functions used in the in-scope contracts).
# [N-08] Event is never emitted
There are two events defined that are never emitted. They can be removed to make the code cleaner.          

[https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L35](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L35)
# [N-09] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification.
           
[https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19](https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19)
# [S-01] Project Upgrade and Stop Scenario should be
Suggestion         
        
At the start of the project, the system may need to be stopped or upgraded, I suggest you have a script beforehand and add it to the documentation. This can also be called an ” EMERGENCY STOP (CIRCUIT BREAKER) PATTERN “.        
       
[https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol](https://github.com/maxwoe/solidity_patterns/blob/master/security/EmergencyStop.sol)
# [S-02] Make the Test Context with Solidity
It’s crucial to write tests with possibly 100% coverage for smart contract systems.         
         
It is recommended to write appropriate tests for all possible code streams and especially for extreme cases.         
         
But the other important point is the test context, tests written with solidity are safer, it is recommended to focus on tests with Foundry.
# [S-03] Generate perfect code headers every time
Description: I recommend using header for Solidity code layout and readability.        
            
[https://github.com/transmissions11/headers](https://github.com/transmissions11/headers)
````solidity
/*//////////////////////////////////////////////////////////////
                           TESTING 123
//////////////////////////////////////////////////////////////*/
````