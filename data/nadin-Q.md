# QA Report
## Summary
Total 04 Low and 09 Non-Critical
### Low Risk Issues
[L-01] getAssetPrice() SHOULD BE TURNED INTO A VIEW FUNCTION
[L-02] REFACTORING TO SUPPORT UINT256 AMOUNT
[L-03] ROUNDING ERROR IN `purchaseOtherEarnings()` MIGHT RESULT IN `biddingFee`
[L-04] MAY LEAD TO DOS IF `curvePool.exchange_underlying` REVERT

## [L-01] getAssetPrice() SHOULD BE TURNED INTO A VIEW FUNCTION
`getAssetPrice()` is used to get the price of the asset, it is used in most of the important functions, should be set to view function to avoid arbitrary price exploitation like reentrancy attack.
```
File: LybraRETHVault
46:    function getAssetPrice() public override returns (uint256) {
File: LybraWbETHVault.sol
34:    function getAssetPrice() public override returns (uint256) { // @audit change to view function
File: LybraWstETHVault.sol
48:    function getAssetPrice() public override returns (uint256) {
File: LybraEUSDVaultBase.sol
327:    function getAssetPrice() public virtual returns (uint256);
File: LybraPeUSDVaultBase.sol
269:    function getAssetPrice() public virtual returns (uint256);
```
[LybraRETHVault.sol#L46](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraRETHVault.sol#L46)
[LybraWstETHVault.sol#L48](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWstETHVault.sol#L48)
[LybraWbETHVault.sol#L34](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWbETHVault.sol#L34)
[LybraEUSDVaultBase.sol#L327](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L327)
[LybraPeUSDVaultBase.sol#L269](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L269)

## [L-02] REFACTORING TO SUPPORT UINT256 AMOUNT
- Although `contract/OFT/OFTCoreV2.sol ` is out of scope, [PeUSDMainnetStableVision#convertToPeUSDAndCrossChain](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L97-L105) calls  `sendFrom()` in above contract.
- According this [PR](https://github.com/LayerZero-Labs/solidity-examples/pull/53) OFT v2 Refactoring to support uint256 amount.
```
The scope of this PR:

Remove type(uint64).max limit for amount from OFTV2Core, ProxyOFTV2 and ProxyOFTWithFee
Introduce OFTV2Aptos, ProxyOFTV2Aptos, ProxyOFTWithFeeAptos that allow no more than 6 shared decimals and have type(uint64).max limit for amount
Replace abi.encodePacked with abi.encode in OFTV2Core and derived contracts
Using abi.encodePacked makes encoding inconsistent with previous versions and decoding ugly. Additionally, abi.encodePacked will be removed in a future version of Solidity (see Remove abi.encodePacked ethereum/solidity#11593)
```

## [L-03] ROUNDING ERROR IN `purchaseOtherEarnings()` MIGHT RESULT IN `biddingFee`
```
File: EUSDMiningIncentives.sol
210:            uint256 biddingFee = (reward * biddingFeeRatio) / 10000;
```
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L210)
If `reward * biddingFeeRatio < 10000` , `biddingFee` will return 0

## [L-04] MAY LEAD TO DOS IF `curvePool.exchange_underlying` REVERT
### Description:
`distributeRewards()` returns the resulting token amount is transferred to the LybraProtocolRewardsPool. 
```
    function distributeRewards() external {
        uint256 peUSDBalance = peUSD.balanceOf(address(this));
        if(peUSDBalance >= 1e21) {
            peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);
            lybraProtocolRewardsPool.notifyRewardAmount(peUSDBalance, 2);
        }
        uint256 balance = EUSD.balanceOf(address(this));
        if (balance > 1e21) {
            uint256 price = curvePool.get_dy_underlying(0, 2, 1e18);
            if(!premiumTradingEnabled || price <= 1005000) {
                EUSD.transfer(address(lybraProtocolRewardsPool), balance);
                lybraProtocolRewardsPool.notifyRewardAmount(balance, 0);
            } else {
                EUSD.approve(address(curvePool), balance);
                uint256 amount = curvePool.exchange_underlying(0, 2, balance, balance * price * 998 / 1e21);
                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);
                lybraProtocolRewardsPool.notifyRewardAmount(amount, 1);
            }
        }
    }
```
[Link to code](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L289-L308)
- [Line 295](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L295) defines balance.
- If `balance <= 1e21` then [Line 303](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L303) will be executed and returns the amount of `2` received in the exchange.
- According to [docs](https://curve.readthedocs.io/factory-pools.html#StableSwap.exchange_underlying) the transaction will revert if the swap would result in less.
- It's unlikely and is a third party issue so the rating is low isuue.
### Recommendations:
Added error handling mechanism if `curvePool.exchange_underlying` reverts.

### Non-Critical Issues
[N-01] COMMENT WRONG ADDRESS OF WBETH
[N-02] SOLIDITY VERSION 0.8.19 CAN BE USED
[N-03] DO NOT CALCULATE CONSTANTS VARIABLES 
[N-04] USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONSTANTS
[N-05] INCLUDE THE PROJECT NAME AND DEVELOPMENT TEAM INFORMATION IN THE CONTRACT TO INCREASE THE POPULARITY AND TRUSR OF USERS
[N-06] Use SMTChecker
[N-07] ACCORDING TO THE SYNTAX RULES, USE => `mapping (` INSTEAD OF => `mapping(` USING SPACES AS KEYWORD
[N-08] USE NAMED PARAMETERS FOR MAPPING TYPE DECLARATIONS
[N-09] IMMUTABLE SHOULD BE UPPERCASE

## [N-01] COMMENT WRONG ADDRESS OF WBETH
### Descriptions:
```
File: contracts/lybra/pools/LybraWbETHVault.sol
16:    //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
```
[LybraWbETHVault.sol#L16](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/LybraWbETHVault.sol#L16) use the address of [rETH](https://etherscan.io/address/0xae78736Cd615f374D3085123A210448E74Fc6393)
### Recommendation:
Change to:
```
16: //WBETH = 0xa2E3356610840701BDf5611a53974510Ae27E2e1
```
[WBETH address](https://etherscan.io/address/0xa2E3356610840701BDf5611a53974510Ae27E2e1)

## [N-02] SOLIDITY VERSION 0.8.19 CAN BE USED
Using the more updated version of Solidity can enhance security. As described in https://github.com/ethereum/solidity/releases, Version `0.8.19` is the latest version of Solidity, which "contains a fix for a long-standing bug that can result in code that is only used in creation code to also be included in runtime bytecode". To be more secured and more future-proofed, please consider using Version `0.8.19` for the following contracts.
CONTEXTS: ALL CONTRACTS

## [N-03] DO NOT CALCULATE CONSTANTS VARIABLES 
Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas. Use hash rather than keccak256 for constant.
```
File: LybraConfigurator.sol
76:    bytes32 public constant DAO = keccak256("DAO");
77:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78:    bytes32 public constant ADMIN = keccak256("ADMIN");

File: GovernanceTimelock.sol
10:    bytes32 public constant DAO = keccak256("DAO");
11:    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12:    bytes32 public constant ADMIN = keccak256("ADMIN");
13:    bytes32 public constant GOV = keccak256("GOV");
```
[LybraConfigurator.sol#L76-L78](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L76-L78)
[GovernanceTimelock.sol#L10-L13](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/GovernanceTimelock.sol#L10-L13)

## [N-04] USE A SINGLE FILE FOR ALL SYSTEM-WIDE CONSTANTS
### Description:
There are many addresses and constants used in the system. It is recommended to put the most used ones in one file (for example constants.sol, use inheritance to access these values).

This will help with readability and easier maintenance for future changes. This also helps with any issues, as some of these hard-coded values are admin addresses.

#### constants.sol
Use and import this file in contracts that require access to these values. This is just a suggestion, in some use cases this may result in higher gas usage in the distribution.

## [N-05] INCLUDE THE PROJECT NAME AND DEVELOPMENT TEAM INFORMATION IN THE CONTRACT TO INCREASE THE POPULARITY AND TRUSR OF USERS
### Recommendation: Use form like FraxFinance project
https://github.com/FraxFinance/frxETH-public/blob/7f7731dbc93154131aba6e741b6116da05b25662/src/sfrxETH.sol#L4-L24

## [N-06] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19

## [N-07] ACCORDING TO THE SYNTAX RULES, USE => `mapping (` INSTEAD OF => `mapping(` USING SPACES AS KEYWORD
[LybraConfigurator.sol#L38-L47](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/configuration/LybraConfigurator.sol#L38-L47)
[LybraGovernance.sol#L28-L29](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L28-L29)
[esLBRBoost.sol#L9](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L9)
[EUSDMiningIncentives.sol#L46-L49](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L46-L49)
[ProtocolRewardsPool.sol#L33-L38](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L33-L38)
[LybraEUSDVaultBase.sol#L28-L29](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L28-L29)
[LybraEUSDVaultBase.sol#L32](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L32)
[LybraPeUSDVaultBase.sol#L21-L24](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L21-L24)
[EUSD.sol#L51-L56](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L51-L56)
[PeUSDMainnetStableVision.sol#L33](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L33)

## [N-08] USE NAMED PARAMETERS FOR MAPPING TYPE DECLARATIONS
Consider using named parameters in mappings (e.g. mapping(address account => uint256 balance)) to improve readability. This feature is present since Solidity 0.8.18
Cases already listed in N-07

## [N-09] IMMUTABLE SHOULD BE UPPERCASE
```
File: ProtocolRewardsPool.sol
39:    uint256 immutable exitCycle = 90 days;
```
[ProtocolRewardsPool.sol#L39](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L39)
```
File: LybraWstETHVault.sol
22:    Ilido immutable lido;
```
[LybraWstETHVault.sol#L22](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWstETHVault.sol#L22)
```
File: LybraEUSDVaultBase
19:    IERC20 public immutable collateralAsset;
21:    uint256 public immutable badCollateralRatio = 150 * 1e18;
22:    IPriceFeed immutable etherOracle;
30:    uint8 immutable vaultType = 0;
```
[LybraEUSDVaultBase.sol#L19-L22](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L19-L22) , [LybraEUSDVaultBase.sol#L30](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30)
```
File: LybraPeUSDVaultBase 
15:    IERC20 public immutable collateralAsset;
18:    uint8 immutable vaultType = 1;
19:    IPriceFeed immutable etherOracle;
```
[LybraPeUSDVaultBase.sol#L15](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L15) , [LybraPeUSDVaultBase.sol#L18-L19](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18-L19)
