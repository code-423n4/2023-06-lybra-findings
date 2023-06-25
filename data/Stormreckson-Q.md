1-The `EUSDMiningIncentives.sol` contract functions lack Natspec, add natspec documentation to each function, describing their purpose and parameters. This will help users in understanding what each function purpose is.

2- in `LybraEUSDVaultBase.sol` 
the nastpec states
        //* Emits a `WithdrawEther` event.
        (..)
        function withdraw(address 
        onBehalfOf, uint256 amount) 
        external 
        virtual {
        (...)
        emit WithdrawAsset(msg.sender, 
        address(collateralAsset), 
        onBehalfOf, withdrawal, 
        block.timestamp);

But the code emits a `withdrawAsset` event
This can be confusing for users, making the natspec  compatible with the function statement is good practice.

3-https://github.com/code-423n4/2023-06-lybra/blob/26915a826c90eeb829863ec3851c3c785800594b/contracts/lybra/pools/LybraStETHVault.sol#L37-L42

Bug Report: Lack of "Paused" State Check in deposEtherToMint()

Description:
In the deposEtherToMint() function of the Lido contract, there is no check for the paused state, which poses a potential vulnerability. This report aims to highlight this issue and propose a remediation strategy.

Detailed Explanation:
When a user calls the deposEtherToMint() function, no verification is performed to ensure that the Lido contract is not in a paused state. This lack of a paused state check could lead to unintended user interaction and potential exploitability, as the function could be called even when the contract is not active or undergoing maintenance.
it is recommended to add a paused state check within the deposEtherToMint() function. This can be achieved by modifying the function as follows:

        function depositEtherToMint() 
        public payable {
        require(!paused(), "The lido  is 
        currently paused."); // Added 
         paused state check

4-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/LBR.sol#L49-L59

 Unsed functions internal functions in LBR.sol 

It is recommended to remove unsed used functions for the following reasons 
Reason 1: Improved code readability
    * By removing unused code, the contract becomes easier to read and understand for developers and auditors. 
    * Ensuring only necessary functions are present enhances code readability and reduces confusion.

 Reason 2: Reduced contract size and gas cost
    * Unused functions contribute to the overall size of the contract. By removing them, the contract's bytecode size is reduced, 
    * resulting in lower deployment and transaction costs. It also reduces the potential for bugs and vulnerabilities in untested code.

Reason 3: Mitigated security risks
    * Unused functions may contain unintended vulnerabilities or bugs that pose security risks. By removing them, the attack surface of the contract is reduced,
    * thereby minimizing the potential for exploitation and increasing the overall security of the smart contract.

Improved contract performance
    * Removing unused code can lead to improved contract performance. Unnecessary calculations and operations are eliminated,
    * resulting in more efficient execution and faster transaction processing.

5-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/EUSD.sol#L333-L341

Potential transfer of zero or negative shares without proper validation

This Proof of Concept aims to demonstrate the impact and severity of a potential issue in the "transferShares" function, where the input parameter sharesAmount is not checked for being greater than zero. This omission may allow unintended transfers of zero or negative shares, resulting in unexpected behavior and potentially compromising the integrity of the application.

This may lead to 
   - Undesired transfers of zero or negative shares can occur.
   - Intended transfers may fail due to the incorrect validation.
   - Account balances and share ownership may become inconsistent.
   - The overall integrity of the application may be compromised.

To mitigate this issue, it is recommended to add a check within the "transferShares" function to verify that sharesAmount is greater than zero before proceeding with the transfer. This ensures that only valid share amounts are transferred, preventing unintended behavior and maintaining the integrity of the application.

6-it is generally a good practice to check for a zero address when dealing with address-related functions or operations. In the following functions, where the user address is required, you can add a check to ensure that the supplied user address is not a zero address. This can help prevent potential errors or malicious actions caused by passing a zero address as the user parameter.

In `contract PeUSDMainnetStableVision` there are a few missing address zero checks in the following functions

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L79-L80

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L97-L98

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L129-L130

https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/PeUSDMainnetStableVision.sol#L142

Adding this check ensures that the user` address provided is valid and not a zero address before executing any further logic within the function.

7-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L95-L96

Mapping Access Issue

the mapping access issue in the code is that it leads to incorrect function invocation. The function `configurator.mintVault(_pools[i])` is mistakenly called as if it were a function, while it is actually a mapping. This results in a runtime error and prevents the code from functioning as intended.
During the execution, the code attempts to invoke `configurator.mintVault(_pools[i])` as a function, but due to the mapping access issue, it fails to execute correctly. As a result, it throws a runtime error, and the transaction reverts.
it may cause inconvenience and waste user gas

To fix the issue, we need to modify the code to correctly access the mapping. Replace the incorrect function invocation with `configurator.mintVault[_pools[i]]` in the setPools function. This will ensure that the mapping is accessed properly, allowing the code to function as intended.

8-https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L72-L87

Token validation if the token is stETH in the depositAssetToMint function.

In the depositAssetToMint function, there should be a check if the token is stEth, it would be appropriate to add a check to verify if the transferred token is indeed stEth.

The primary goal of this Proof of Concept is to evaluate the potential impact and severity of implementing a check to verify if the token is stETH in the depositAssetToMint function. This check aims to ensure that the deposited asset is indeed stETH and prevent any potential issues that may arise from using other tokens as collateral.

Token Integrity: Without token verification, non-stETH tokens could be mistakenly accepted as collateral. This might lead to incorrect calculations, miscalculations of the mintAmount, or other unpredictable behaviors.
   - Protocol Stability: Unauthorized tokens might not have the same functionality as stETH, potentially causing issues during the minting process or other critical operations.
   - Risk of Exploitation: Malicious actors could exploit the absence of token verification, leading to unintended behaviors, loss of assets, or attacks on the protocol.
   - Asset Valuation: Using unsupported tokens as collateral may result in incorrect asset valuations, compromising the trust and reliability of the system and affecting user confidence.
   - Financial Loss: In the absence of token verification, mintAmount calculations might be inaccurate, potentially resulting in financial losses for users or the protocol itself.
   - User Experience: Users might mistakenly deposit incorrect tokens, encounter unexpected errors, or experience delays due to failed transactions or incorrect calculations. This could harm user trust and hinder the adoption of the lending/borrowing protocol.

        require(collateralAsset == 
        address(stEthTokenContract), 
        "Invalid token"); ensures that the 
        token being transferred is exactly 
        stEth.

9- https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L137-L138

in the `StakedOf` function the line         uint256 amount; is not initialized before being used in the loop, which could lead to unexpected results. 

To ensure accurate calculations, it is advisable to initialize `amount` to zero before entering the loop, like this:

      uint256 `amount` = 0;

By initializing `amount` to zero, you can guarantee that its value is consistent throughout the execution of the loop.

