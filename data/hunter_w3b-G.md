# Gas Optimization

# Summary

| Number | Optimization Details                                                                | Context |
| :----: | :---------------------------------------------------------------------------------- | :-----: |
| [G-01] | Avoid contract `existence` checks by using low level calls                          |    1    |
| [G-02] | Using fixed bytes is cheaper than using `string`                                    |    2    |
| [G-03] | Expressions for `constant` values such as a call to keccak256(), should use immutable rather than constant                                                                                    |    7    |
| [G-04] | Using `calldata` instead of `memory` for read-only arguments                        |    7    |
| [G-05] | Before some functions, we should `check` some variables for possible gas save       |    8    |
| [G-06] | `LybraPeUSDVaultBase::LybraPeUSDVaultBase()` function not called by the contract should be removed to save deployment gas                                                                                    |    1    |
| [G-07] | Duplicated `require()` checks should be refactored to a modifier or function        |   18    |
| [G-08] | Use constants instead of `type(uintx).max`                                          |    1    |
| [G-09] | Use double `require` instead of using &&                                            |    3    |
| [G-10] | A `modifier` used only once should be inlined to save gas                           |    3    |
| [G-11] | Refactor event to avoid `emitting` data that is already present in transaction data |   26    |
| [G-12] | Use hardcode address instead `address(this)`                                        |   38    |
| [G-13] | Use Assembly To Check For `address(0)`                                              |   16    |
| [G-14] | When possible, use assembly instead of `unchecked{++i}`                             |    2    |
| [G-15] | Use nested if and, avoid multiple check combinations `&&`                           |    4    |
| [G-16] | Sort Solidity operations using `short-circuit` mode                                 |    2    |
| [G-17] | Ternary operation is cheaper than `if-else` statement                               |    2    |
| [G-18] | Use `assembly` to write address storage values                                      |   12    |
| [G-19] | Make 3 event `parameters` indexed when possible                                     |   37    |
| [G-20] | use `mappings` instead of arrays                                                    |    2    |

## [G-01] Avoid contract `existence` checks by using low level calls

Prior to 0.8.10 the compiler inserted extra code, including EXTCODESIZE (100 gas), to check for contract existence for external function calls. In more recent solidity versions, the compiler will not insert these checks if the external call has a return value. Similar behavior can be achieved in earlier versions by using low-level calls, since low level calls never check for contract existence.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L172

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

172        amount = IEUSD(configurator.getEUSDAddress()).getMintedEUSDByShares(earned(user));

232            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);

```

## [G-02] Using fixed bytes is cheaper than using `string`

use bytes for arbitrary-length raw byte data and string for arbitrary-length string (UTF-8) data.
If you can limit the length to a certain number of bytes, always use one of bytes1 to bytes32 because they are much cheaper.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

```solidity
file: lybra/token/EUSD.sol

99    function name() public pure returns (string memory) {
100        return "eUSD";
101    }
102
103    /**
104     * @return the symbol of the token, usually a shorter version of the
105     * name.
106     */
107    function symbol() public pure returns (string memory) {
108        return "eUSD";
109    }

```

```diff
diff --git a/EUSD.org.sol b/EUSD.sol
index 6753258..529c94b 100644
--- a/EUSD.org.sol
+++ b/EUSD.sol
@@ -1,11 +1,11 @@
-    function name() public pure returns (string memory) {
-        return "eUSD";
+    function name() public pure returns (bytes32) {
+        return bytes32("eUSD");
     }

     /**
      * @return the symbol of the token, usually a shorter version of the
      * name.
      */
-    function symbol() public pure returns (string memory) {
-        return "eUSD";
+    function symbol() public pure returns (bytes32) {
+        return bytes32("eUSD");
     }

```

## [G-03] Expressions for `constant` values such as a call to keccak256(), should use immutable rather than constant

expressions that involve constant values, such as a call to keccak256(), are recommended to be used with the immutable keyword rather than constant. This is to ensure more efficient contract deployment and reduce gas costs.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

10    bytes32 public constant DAO = keccak256("DAO");
11    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
12    bytes32 public constant ADMIN = keccak256("ADMIN");
13    bytes32 public constant GOV = keccak256("GOV");

```

```diff

diff --git a/GovernanceTimelock.org.sol b/GovernanceTimelock.sol
index 3393915..c98e5cf 100644
--- a/GovernanceTimelock.org.sol
+++ b/GovernanceTimelock.sol
@@ -1,7 +1,7 @@

 contract GovernanceTimelock is TimelockController {
     // contructor()
-    bytes32 public constant DAO = keccak256("DAO");
-    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
-    bytes32 public constant ADMIN = keccak256("ADMIN");
-    bytes32 public constant GOV = keccak256("GOV");

+    bytes32 public immutable DAO = keccak256("DAO");
+    bytes32 public immutable TIMELOCK = keccak256("TIMELOCK");
+    bytes32 public immutable ADMIN = keccak256("ADMIN");
+    bytes32 public immutable GOV = keccak256("GOV");

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L76-L78

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

76    bytes32 public constant DAO = keccak256("DAO");
77    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");
78    bytes32 public constant ADMIN = keccak256("ADMIN");
```

## [G-04] Using `calldata` instead of `memory` for read-only arguments

It is recommended to use calldata instead of memory for read-only arguments whenever possible. By accessing the arguments directly from calldata, you avoid unnecessary data copying and reduce gas costs associated with memory operations.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L15

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol


15    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

```

```diff

diff --git a/GovernanceTimelock.org.sol b/GovernanceTimelock.sol
index ebca61a..13cd795 100644
--- a/GovernanceTimelock.org.sol
+++ b/GovernanceTimelock.sol
@@ -1 +1 @@
-    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

+    constructor(uint256 minDelay, address[] calldata proposers, address[] calldata executors, address admin) TimelockController(minDelay, proposers, executors, admin) {

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L106

```solidity
File: lybra/governance/LybraGovernance.sol

106    function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

```

```diff
diff --git a/LybraGovernance.org.sol b/LybraGovernance.sol
index ce3b29e..1e4750e 100644
--- a/LybraGovernance.org.sol
+++ b/LybraGovernance.sol
@@ -1 +1 @@
-106    function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

+106    function _execute(uint256 /* proposalId */, address[] calldata targets, uint256[] calldata values, bytes[] calldata calldatas, bytes32 descriptionHash) internal virtual override {



```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

```solidity
File:

93    function setPools(address[] memory _pools) external onlyOwner {

```

## [G-05] Before some functions, we should `check` some variables for possible gas save

A function PeUSD.sol::transferFrom that performs a transfer of tokens from one address to another.
By checking if the amount being transferred is non-zero before executing the transfer, the function ensures that unnecessary transfers are avoided. This can help optimize gas usage and prevent the execution of the transfer function when the amount being transferred is zero.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L43C2-L50C6

```solidit
File: contracts/lybra/token/PeUSD.sol

43    function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
44        address spender = _msgSender();
45        // if transfer from this contract, no need to check allowance
46        if (_from != address(this) && _from != spender)
47            _spendAllowance(_from, spender, _amount);
48        _transfer(_from, _to, _amount);
49        return _amount;
50    }

```

```diff
diff --git a/PeUSD.org.sol b/PeUSD.sol
index 40e69d0..f9f9b62 100644
--- a/PeUSD.org.sol
+++ b/PeUSD.sol
@@ -1,5 +1,7 @@
     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
         address spender = _msgSender();
+        require(_amount != 0, "Amount must be greater than zero");
         // if transfer from this contract, no need to check allowance
         if (_from != address(this) && _from != spender)
             _spendAllowance(_from, spender, _amount);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L61C1-L68C6

```solidity
File: contracts/lybra/token/LBR.sol

 61   function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
 62       address spender = _msgSender();
 63       // if transfer from this contract, no need to check allowance
 64       if (_from != address(this) && _from != spender)
 65           _spendAllowance(_from, spender, _amount);
 66       _transfer(_from, _to, _amount);
 67       return _amount;
 68   }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L82

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

82        bool success = EUSD.transferFrom(user, address(this), eusdAmount);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

```solidity
File: contracts/lybra/token/EUSD.sol

153    function transfer(address _recipient, uint256 _amount) public returns (bool) {
154        address owner = _msgSender();
155        _transfer(owner, _recipient, _amount);
156        return true;
157    }



336        _transferShares(owner, _recipient, _sharesAmount);


350        _transferShares(_sender, _recipient, _sharesToTransfer);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L75

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

75        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L61

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

61        collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

```

## [G-06] `LybraPeUSDVaultBase::LybraPeUSDVaultBase()` function not called by the contract should be removed to save deployment gas

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L244C1-L246C6

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol


244    function _etherPrice() internal returns (uint256) {
245        return etherOracle.fetchPrice();
246    }
```

## [G-07] Duplicated `require()` checks should be refactored to a modifier or function

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/esLBR.sol

```solidity
File: contracts/lybra/token/esLBR.sol

31        require(configurator.tokenMiner(msg.sender), "not authorized");
39        require(configurator.tokenMiner(msg.sender), "not authorized");
```

```diff
diff --git a/esLBR.org.sol b/esLBR.sol
index 409a8b9..7171560 100644
--- a/esLBR.org.sol
+++ b/esLBR.sol
@@ -1,4 +1,4 @@
-    function mint(address user, uint256 amount) external returns (bool) {
+    function mint(address user, uint256 amount) external onlyTokenMiner returns (bool) {
         require(configurator.tokenMiner(msg.sender), "not authorized");
         require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");
         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
@@ -6,10 +6,14 @@
         return true;
     }

-    function burn(address user, uint256 amount) external returns (bool) {
-        require(configurator.tokenMiner(msg.sender), "not authorized");
+    function burn(address user, uint256 amount) external onlyTokenMiner returns (bool) {
         try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}
         _burn(user, amount);
         return true;
     }
+
+    modifier onlyTokenMiner() {
+    require(configurator.tokenMiner(msg.sender), "Not authorized");
+    _;
+}
 }
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L26

```solidity
File: contracts/lybra/token/LBR.sol#L26

26        require(configurator.tokenMiner(msg.sender), "not authorized");
33        require(configurator.tokenMiner(msg.sender), "not authorized");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L71

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

71            require(success, "TF");
86            require(success, "TF");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L84

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

84        require(_amount > 0, "amount = 0");
94        require(_amount > 0, "amount = 0");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L83

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

83        require(success, "TF");
134        require(success, "TF");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L441

```solidity
File: contracts/lybra/token/EUSD.sol

441        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
460        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

83        require(onBehalfOf != address(0), "TZA");
97        require(onBehalfOf != address(0), "TZA");
111        require(onBehalfOf != address(0), "TZA");


84        require(amount > 0, "ZA");
98        require(amount > 0, "ZA");
112        require(amount > 0, "ZA");
```

## [G-08] Use constants instead of `type(uintx).max`

type(uintx).max it uses more gas in the distribution process and also for each transaction than constant usage.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L179

```solidity
File: contracts/lybra/token/EUSD.sol

179        if (currentAllowance != type(uint256).max) {

```

```diff
diff --git a/EUSD.org.sol b/EUSD.sol
index 529790d..c1aec6e 100644
--- a/EUSD.org.sol
+++ b/EUSD.sol
@@ -1,3 +1,8 @@
+     uint256 constant MAX_UINT256 = type(uint256).max;




     function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
         uint256 currentAllowance = allowance(owner, spender);
-        if (currentAllowance != type(uint256).max) {

+        if (currentAllowance != MAX_UINT256 {

```

## [G-09] Use double `require` instead of using &&

Using double require statements can help with gas optimization. If one of the conditions fails, the function reverts immediately, saving gas by skipping the evaluation of the remaining conditions.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L64

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

64        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");

```

```diff
diff --git a/LybraStETHVault.org.sol b/LybraStETHVault.sol
index 51adc3f..cb2e429 100644
--- a/LybraStETHVault.org.sol
+++ b/LybraStETHVault.sol
@@ -1,4 +1,6 @@
     function excessIncomeDistribution(uint256 stETHAmount) external override {
         uint256 excessAmount = collateralAsset.balanceOf(address(this)) - totalDepositedAsset;
-        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");
+        require(excessAmount > 0, "Excess amount must be greater than zero");
+        require(stETHAmount > 0, "stETH amount must be greater than zero");
+        require(true, "Only LSD excess income can be exchanged");
         uint256 realAmount = stETHAmount > excessAmount ? excessAmount : stETHAmount;

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L115

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

114     function convertToEUSD(uint256 peusdAmount) external {
115:         require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
116        _burn(msg.sender, peusdAmount);
```

```diff
diff --git a/PeUSDMainnetStableVision.org.sol b/notOri.sol
index c58792f..d80e750 100644
--- a/PeUSDMainnetStableVision.org.sol
+++ b/PeUSDMainnetStableVision.sol
@@ -1,3 +1,5 @@
     function convertToEUSD(uint256 peusdAmount) external {
-        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");
+        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD, "Exceeds minted peUSD limit");
+        require(peusdAmount > 0, "peUSD amount must be greater than zero");
+        require(true, "PCE");
         _burn(msg.sender, peusdAmount);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L127

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

126    function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
127        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
128        vaultBadCollateralRatio[pool] = newRatio;
```

```diff
diff --git a/LybraConfigurator.org.sol b/LybraConfigurator.sol
index a968056..d85d0b4 100644
--- a/LybraConfigurator.org.sol
+++ b/LybraConfigurator.sol
@@ -1,3 +1,6 @@
     function setBadCollateralRatio(address pool, uint256 newRatio) external onlyRole(DAO) {
-        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");
+        require(newRatio >= 130 * 1e18, "Ratio must be greater than or equal to 130");
+        require(newRatio <= 150 * 1e18, "Ratio must be less than or equal to 150");
+        require(newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "Invalid new ratio");
+        require(true, "LNA");
         vaultBadCollateralRatio[pool] = newRatio;

```

## [G-10] A `modifier` used only once should be inlined to save gas

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46C1-L53C6

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

46    modifier MintPaused() {
47        require(!configurator.vaultMintPaused(msg.sender), "MPP");
48        _;
49    }
50    modifier BurnPaused() {
51        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
52        _;
53    }
```

The above `modifiers` is only used in the subsequent functions. It is recommended to remove the modifier and corresponding checks directly within each function instead.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

63    function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
64        require(to != address(0), "TZA");
65        _mint(to, amount);
66        return true;
67    }
68
69    function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {
70        _burn(account, amount);
71        return true;
72    }

```

```diff

diff --git a/PeUSDMainnetStableVision.org.sol b/PeUSDMainnetStableVision.sol
index 9812cdb..6d52162 100644
--- a/PeUSDMainnetStableVision.org.sol
+++ b/PeUSDMainnetStableVision.sol
@@ -1,10 +1,12 @@
     function mint(address to, uint256 amount) external onlyMintVault MintPaused returns (bool) {
+        require(!configurator.vaultMintPaused(msg.sender), "MPP");
         require(to != address(0), "TZA");
         _mint(to, amount);
         return true;
     }

     function burn(address account, uint256 amount) external onlyMintVault BurnPaused returns (bool) {
+         require(!configurator.vaultBurnPaused(msg.sender), "BPP");
         _burn(account, amount);
         return true;
     }

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83C1-L90C6

```solidity
File: contracts/lybra/token/EUSD.sol

83     modifier MintPaused() {
84        require(!configurator.vaultMintPaused(msg.sender), "MPP");
85        _;
86    }

```

The above modifer is only used in the following:

```solidity
File: contracts/lybra/token/EUSD.sol

411:    function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
412        require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");
413
414        uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount);
415        if (sharesAmount == 0) {
416            //EUSD totalSupply is 0: assume that shares correspond to EUSD 1-to-1
417            sharesAmount = _mintAmount;
418        }
412
420        newTotalShares = _totalShares.add(sharesAmount);
421        _totalShares = newTotalShares;
422
423        shares[_recipient] = shares[_recipient].add(sharesAmount);
424
425        _totalSupply += _mintAmount;
426
427        emit Transfer(address(0), _recipient, _mintAmount);
428    }
```

```diff
diff --git a/EUSD.org.sol b/EUSD.sol
index 1b91de2..3af851f 100644
--- a/EUSD.org.sol
+++ b/EUSD.sol
@@ -1,4 +1,5 @@
     function mint(address _recipient, uint256 _mintAmount) external onlyMintVault MintPaused returns (uint256 newTotalShares) {
+        require(!configurator.vaultMintPaused(msg.sender), "MPP");
         require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");

         uint256 sharesAmount = getSharesByMintedEUSD(_mintAmount);
```

## [G-11] Refactor event to avoid `emitting` data that is already present in transaction data

In the instances below, block.timestammp, does not have to be emitted since the timestamp is already present in the transaction data. This would save loading data into memory (potentially avoiding memory expansion costs) and Glogdata (8 gas) `*`bytes emitted.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L31

```solidity
File: contracts/lybra/pools/LybraWbETHVault.sol

// @audit block.timestamp  present in tx data
31        emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol#L46

```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol

// @audit block.timestamp  present in tx data
45        emit DepositEther(msg.sender, address(collateralAsset), msg.value,wstETHAmount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L38

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

// @audit block.timestamp  present in tx data
38        emit DepositEther(msg.sender, address(collateralAsset), msg.value,balance - preBalance, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol

```solidity
File: contracts/lybra/pools/LybraStETHVault.sol

// @audit block.timestamp  present in tx data
51        emit DepositEther(msg.sender, address(collateralAsset), msg.value, msg.value, block.timestamp);


// @audit block.timestamp  present in tx data
83            emit FeeDistribution(address(configurator), income, block.timestamp);


// @audit block.timestamp  present in tx data
89            emit FeeDistribution(address(configurator), payAmount, block.timestamp);


// @audit block.timestamp  present in tx data
94        emit LSDValueCaptured(realAmount, payAmount, getDutchAuctionDiscountPrice(), block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L89C1-L90C1

```solidity
File: contracts/lybra/miner/stakerewardV2pool.sol

// @audit block.timestamp  present in tx data
89        emit StakeToken(msg.sender, _amount, block.timestamp);

// @audit block.timestamp  present in tx data
98        emit WithdrawToken(msg.sender, _amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol

// @audit block.timestamp  present in tx data
97        emit UnstakeLBR(msg.sender, amount, block.timestamp);


// @audit block.timestamp  present in tx data
106        emit WithdrawLBR(user, amount, block.timestamp);


// @audit block.timestamp  present in tx data
146        emit Restake(msg.sender, amount, block.timestamp);

// @audit block.timestamp  present in tx data
203                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(peUSD), reward - eUSDShare, block.timestamp);

// @audit block.timestamp  present in tx data
211                    emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(token), reward - eUSDShare, block.timestamp);


// @audit block.timestamp  present in tx data
214                emit ClaimReward(msg.sender, EUSD.getMintedEUSDByShares(eUSDShare), address(0), 0, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L149

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

// @audit block.timestamp  present in tx data
149        emit EUSDMiningIncentivesChanged(addr, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol


// @audit block.timestamp  present in tx data
198            emit ClaimReward(msg.sender, reward, block.timestamp);

// @audit block.timestamp  present in tx data
222            emit ClaimedOtherEarnings(msg.sender, user, reward, biddingFee, useEUSD, block.timestamp);

// @audit block.timestamp  present in tx data
241        emit NotifyRewardChanged(amount, block.timestamp);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

// @audit block.timestamp  present in tx data
85        emit DepositAsset(msg.sender, address(collateralAsset), assetAmount, block.timestamp);


// @audit block.timestamp  present in tx data
111        emit WithdrawAsset(msg.sender, address(collateralAsset), onBehalfOf, withdrawal, block.timestamp);

// @audit block.timestamp  present in tx data
175        emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, reducedAsset, reward2keeper, false, block.timestamp);


// @audit block.timestamp  present in tx data
210        emit LiquidationRecord(provider, msg.sender, onBehalfOf, eusdAmount, assetAmount, reward2keeper, true, block.timestamp);


// @audit block.timestamp  present in tx data
243        emit RigidRedemption(msg.sender, provider, eusdAmount, collateralAmount, block.timestamp);

// @audit block.timestamp  present in tx data
268        emit Mint(msg.sender, _onBehalfOf, _mintAmount, block.timestamp);

// @audit block.timestamp  present in tx data
285        emit Burn(_provider, _onBehalfOf, amount, block.timestamp);

```

## [G-12] Use hardcode address instead `address(this)`

it can be more gas-efficient to use a hardcoded address instead of the address(this) expression, especially if you need to use the same address multiple times in your contract.

The reason for this is that using address(this) requires an additional EXTCODESIZE operation to retrieve the contract's address from its bytecode, which can increase the gas cost of your contract. By pre-calculating and using a hardcoded address, you can avoid this additional operation and reduce the overall gas cost of your contract.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/GovernanceTimelock.sol#L20

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol

20        _grantRole(DAO, address(this));

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L22

```solidity
File: contracts/lybra/pools/LybraWbETHVault.sol

22        uint256 preBalance = collateralAsset.balanceOf(address(this));

24        uint256 balance = collateralAsset.balanceOf(address(this));

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol#L29

```solidity
File: contracts/lybra/pools/LybraRETHVault.sol

29        uint256 preBalance = collateralAsset.balanceOf(address(this));

31        uint256 balance = collateralAsset.balanceOf(address(this));

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46

```solidity
File: lybra/token/PeUSD.sol

25        return address(this);

46        if (_from != address(this) && _from != spender)
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L43

```solidity
File: lybra/token/LBR.sol

43        return address(this);

64        if (_from != address(this) && _from != spender)
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol#L63

```solidity
File: lybra/pools/LybraStETHVault.sol

63        uint256 excessAmount = collateralAsset.balanceOf(address(this)) - totalDepositedAsset;

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L84

```solidity
File: lybra/miner/stakerewardV2pool.sol

84        bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

80        require(_msgSender() == user || _msgSender() == address(this), "MDM");

81        require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");

82        bool success = EUSD.transferFrom(user, address(this), eusdAmount);

133        bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));

176        return address(this);

199        if (_from != address(this) && _from != spender)

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
File: lybra/miner/ProtocolRewardsPool.sol

195            uint256 balance = EUSD.sharesOf(address(this));

200                uint256 peUSDBalance = peUSD.balanceOf(address(this));

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L85

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

75        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

160        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");

171            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;

197        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");

204        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");

205            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;

260        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");

292        if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");


301        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

```solidity
File:  lybra/pools/base/LybraPeUSDVaultBase.sol

45        return collateralAsset.balanceOf(address(this));

60        uint256 preBalance = collateralAsset.balanceOf(address(this));

61        collateralAsset.transferFrom(msg.sender, address(this), assetAmount);

62        require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");

128        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");

131        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");

141            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;

174        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");

226        if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this)))

238        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;

```

## [G-13] Use Assembly To Check For `address(0)`

it's generally more gas-efficient to use assembly to check for a zero address (address(0)) than to use the == operator.

The reason for this is that the == operator generates additional instructions in the EVM bytecode, which can increase the gas cost of your contract. By using assembly, you can perform the zero address check more efficiently and reduce the overall gas cost of your contract.

Here's an example of how you can use assembly to check for a zero address:

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L60

```solidity

    function isZeroAddress(address addr) public pure returns (bool) {
        uint256 addrInt = uint256(addr);

        assembly {
            // Load the zero address into memory
            let zero := mload(0x00)

            // Compare the address to the zero address
            let isZero := eq(addrInt, zero)

            // Return the result
            mstore(0x00, isZero)
            return(0, 0x20)
        }
    }
```

```solidity
File: lybra/miner/stakerewardV2pool.sol

60      if (_account != address(0)) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L64

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

64      require(to != address(0), "TZA");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

```solidity
File: lybra/token/EUSD.sol

367        require(_owner != address(0), "APPROVE_FROM_ZERO_ADDRESS");

368        require(_spender != address(0), "APPROVE_TO_ZERO_ADDRESS");

412     require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");

441     require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");

460     require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol

```solidity
File: lybra/configuration/LybraConfigurator.sol

99      if (address(EUSD) == address(0)) EUSD = IEUSD(\_eusd);

100     if (address(peUSD) == address(0)) peUSD = IEUSD(\_peusd);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L76

```solidity
File: lybra/miner/EUSDMiningIncentives.sol

76              if (_account != address(0)) {
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

```solidity
File:   lybra/pools/base/LybraEUSDVaultBase.sol

99      require(onBehalfOf != address(0), "TZA");

127 require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");

141 require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

```solidity
File: lybra/pools/base/LybraPeUSDVaultBase.sol

83      require(onBehalfOf != address(0), "TZA");

97    require(onBehalfOf != address(0), "TZA");

111    require(onBehalfOf != address(0), "TZA");
```

## [G-14] When possible, use assembly instead of `unchecked{}`

You can also use unchecked{} for even more gas savings but this will not check to see if i overflows. For best gas savings, use inline assembly, however, this limits the functionality you can achieve.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L181C1-L183C14

```solidity
File: lybra/token/EUSD.sol

181         unchecked {
182                _approve(owner, spender, currentAllowance - amount);
183            }

```

```diff
diff --git a/EUSD.org.sol b/EUSD.sol
index ae47494..7da18e3 100644
--- a/EUSD.org.sol
+++ b/EUSD.sol
@@ -2,7 +2,10 @@
         uint256 currentAllowance = allowance(owner, spender);
         if (currentAllowance != type(uint256).max) {
             require(currentAllowance >= amount, "ERC20: insufficient allowance");
-            unchecked {
-                _approve(owner, spender, currentAllowance - amount);
-            }
+              assembly {
+            let allowanceSlot := add(_allowances_slot, mul(owner, 2))
+            let currentAllowance := sload(allowanceSlot)
+            let newAllowance := sub(currentAllowance, amount)
+            sstore(allowanceSlot, newAllowance)
+        }
         }

```

```solidity
File: lybra/token/EUSD.sol

272        unchecked {
273            _approve(owner, spender, currentAllowance - subtractedValue);
274        }
```

```diff
diff --git a/EUSD.org.sol b/EUSD.sol
index cd3f679..a400678 100644
--- a/EUSD.org.sol
+++ b/notOri.sol
@@ -2,8 +2,11 @@
         address owner = _msgSender();
         uint256 currentAllowance = allowance(owner, spender);
         require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
-        unchecked {
-            _approve(owner, spender, currentAllowance - subtractedValue);
+           assembly {
+        let allowanceSlot := add(_allowances_slot, mul(owner, 2))
+        let currentAllowance := sload(allowanceSlot)
+        let newAllowance := sub(currentAllowance, subtractedValue)
+        sstore(allowanceSlot, newAllowance)
         }

         return true;

```

## [G-15] Use nested if and, avoid multiple check combinations `&&`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSD.sol#L46

```solidity
File: lybra/token/PeUSD.sol

46        if (_from != address(this) && _from != spender)
```

```diff
diff --git a/PeUSD.org.sol b/PeUSD.sol
index 62bc4ce..f7e9166 100644
--- a/PeUSD.org.sol
+++ b/PeUSD.sol
@@ -1,6 +1,9 @@
     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
         address spender = _msgSender();
         // if transfer from this contract, no need to check allowance
-        if (_from != address(this) && _from != spender)
+        if (_from != address(this)) {
+            if (_from != spender) {
             _spendAllowance(_from, spender, _amount);
+        }
         _transfer(_from, _to, _amount);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol#L64

```solidity
File: lybra/token/LBR.sol

64        if (_from != address(this) && _from != spender)
```

```diff
diff --git a/LBR.org.sol b/LBR.sol
index 62bc4ce..f7e9166 100644
--- a/LBR.org.sol
+++ b/LBR.sol
@@ -1,6 +1,9 @@
     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
         address spender = _msgSender();
         // if transfer from this contract, no need to check allowance
-        if (_from != address(this) && _from != spender)
+        if (_from != address(this)) {
+            if (_from != spender) {
             _spendAllowance(_from, spender, _amount);
+        }
         _transfer(_from, _to, _amount);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

```solidity
File: lybra/token/PeUSDMainnetStableVision.sol

199        if (_from != address(this) && _from != spender)

```

```diff
diff --git a/PeUSDMainnetStableVision.org.sol b/PeUSDMainnetStableVision.sol
index 62bc4ce..f7e9166 100644
--- a/PeUSDMainnetStableVision.org.sol
+++ b/PeUSDMainnetStableVision.sol
@@ -1,6 +1,9 @@
     function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
         address spender = _msgSender();
         // if transfer from this contract, no need to check allowance
-        if (_from != address(this) && _from != spender)
+        if (_from != address(this)) {
+            if (_from != spender) {
             _spendAllowance(_from, spender, _amount);
+        }
         _transfer(_from, _to, _amount);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204

```solidity
File: lybra/pools/base/LybraEUSDVaultBase.sol

204        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {

```

## [G-16] Sort Solidity operations using `short-circuit` mode

Short-circuiting is a solidity contract development model that uses OR/AND logic to sequence different cost operations. It puts low gas cost operations in the front and high gas cost operations in the back, so that if the front is low If the cost operation is feasible, you can skip (short-circuit) the subsequent high-cost Ethereum virtual machine operation.

```solidity

//f(x) is a low gas cost operation
//g(y) is a high gas cost operation

//Sort operations with different gas costs as follows
f(x) || g(y)
f(x) && g(y)

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol

```solidity
File: lybra/miner/esLBRBoost.sol

60         if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt) {


63        if (finishAt <= boostEndTime || block.timestamp <= boostEndTime) {

```

## [G-17] Ternary operation is cheaper than `if-else` statement

There are instances where a ternary operation can be used instead of if-else statement.
In these cases, `using ternary operation will save modest amounts of gas` .

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L301

```solidity
File: lybra/token/EUSD.sol

301        if (totalMintedEUSD == 0) {
302            return 0;
303        } else {
304            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
305        }
```

```diff

diff --git a/EUSD.org.sol b/EUSD.sol
index 2be1551..4e40b81 100644
--- a/EUSD.org.sol
+++ b/EUSD.sol
@@ -1,8 +1,5 @@
     function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
         uint256 totalMintedEUSD = _totalSupply;
-        if (totalMintedEUSD == 0) {
-            return 0;
-        } else {
-            return _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
-        }

+        return totalMintedEUSD == 0 ? 0 : _EUSDAmount.mul(_totalShares).div(totalMintedEUSD);
     }

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L312

```solidity
File: lybra/token/EUSD.sol

312        if (_totalShares == 0) {
313            return 0;
314        } else {
315           return _sharesAmount.mul(_totalSupply).div(_totalShares);
316        }
```

```diff
diff --git a/EUSD.org.sol b/EUSD.sol
index 0a66f6e..a66648a 100644
--- a/EUSD.org.sol
+++ b/EUSD.sol
@@ -1,7 +1,3 @@
     function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
-        if (_totalShares == 0) {
-            return 0;
-        } else {
-            return _sharesAmount.mul(_totalSupply).div(_totalShares);
-        }
+     return _totalShares == 0 ? 0 : _sharesAmount.mul(_totalSupply).div(_totalShares);
     }

```

## [G-18] Use assembly to write address storage values

By using assembly to write to address storage values, you can bypass some of these operations and lower the gas cost of writing to storage. Assembly code allows you to directly access the Ethereum Virtual Machine (EVM) and perform low-level operations that are not possible in Solidity.

example of using assembly to write to address storage values:

```

    address private myAddress;

    function setAddressUsingAssembly(address newAddress) public {
        assembly {
            sstore(0, newAddress)
        }
    }

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol

```solidity
File: /contracts/lybra/configuration/LybraConfigurator.sol


81    GovernanceTimelock = IGovernanceTimelock(_dao);

82    curvePool = ICurvePool(_curvePool);

138   lybraProtocolRewardsPool = IProtocolRewardsPool(addr);

148   eUSDMiningIncentives = IeUSDMiningIncentives(addr);

262   stableToken = _token;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol


86   esLBR = _eslbr;

90   lbrPriceFeed = AggregatorV3Interface(_lbrOracle);

116  esLBRBoost = IesLBRBoost(_boost);

125  ethlbrStakePool = _pool;

126  ethlbrLpToken = _lp;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

```solidity
File: /contracts/lybra/miner/ProtocolRewardsPool.sol

55  esLBRBoost = IesLBRBoost(_boost);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol

```solidity
File: /contracts/lybra/pools/LybraRETHVault.sol

43   rkPool = IRkPool(addr);
```

## [G-19] Make 3 event `parameters` indexed when possible

It’s the most gas efficient to make up to 3 event parameters indexed. If there are less than 3 parameters, you need to make all parameters indexed.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol

59    event ClaimReward(address indexed user, uint256 amount, uint256 time);

60    event ClaimedOtherEarnings(address indexed user, address indexed Victim, uint256 buyAmount, uint256 biddingFee, bool useEUSD, uint256 time);

61    event NotifyRewardChanged(uint256 addAmount, uint256 time);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol

```solidity
File: /contracts/lybra/configuration/LybraConfigurator.sol

63  event RedemptionFeeChanged(uint256 newSlippage);

64    event SafeCollateralRatioChanged(address indexed pool, uint256 newRatio);

65  event RedemptionProvider(address indexed user, bool status);

66    event ProtocolRewardsPoolChanged(address indexed pool, uint256 timestamp);

67    event EUSDMiningIncentivesChanged(address indexed pool, uint256 timestamp);

68    event BorrowApyChanged(address indexed pool, uint256 newApy);

69    event KeeperRatioChanged(address indexed pool, uint256 newSlippage);

70  event tokenMinerChanges(address indexed pool, bool status);

74  event FlashloanFeeUpdated(uint256 fee);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L42-L46

```solidity
File: /contracts/lybra/miner/ProtocolRewardsPool.sol

42   event Restake(address indexed user, uint256 amount, uint256 time);

43    event StakeLBR(address indexed user, uint256 amount, uint256 time);

44    event UnstakeLBR(address indexed user, uint256 amount, uint256 time);

45    event WithdrawLBR(address indexed user, uint256 amount, uint256 time);

46    event ClaimReward(address indexed user, uint256 eUSDAmount, address token, uint256 tokenAmount, uint256 time);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

```solidity
File: /contracts/lybra/miner/stakerewardV2pool.sol

44  event StakeToken(address indexed user, uint256 amount, uint256 time);

    event WithdrawToken(address indexed user, uint256 amount, uint256 time);

    event ClaimReward(address indexed user, uint256 amount, uint256 time);

    event NotifyRewardChanged(uint256 addAmount, uint256 time);

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol

```solidity
File: /contracts/lybra/pools/base/LybraEUSDVaultBase.sol

    event DepositEther(address indexed onBehalfOf, address asset, uint256 etherAmount, uint256 assetAmount, uint256 timestamp);

    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

    event WithdrawAsset(address sponsor, address asset, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 liquidateEtherAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

    event LSDValueCaptured(uint256 stETHAdded, uint256 payoutEUSD, uint256 discountRate, uint256 timestamp);

    event RigidRedemption(address indexed caller, address indexed provider, uint256 eusdAmount, uint256 collateralAmount, uint256 timestamp);



    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

```solidity
File: /contracts/lybra/pools/base/LybraPeUSDVaultBase.sol

    event DepositEther(address indexed onBehalfOf, address asset, uint256    etherAmount, uint256 assetAmount, uint256 timestamp);

    event DepositAsset(address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

    event WithdrawAsset(address sponsor, address indexed onBehalfOf, address asset, uint256 amount, uint256 timestamp);

    event Mint(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

    event Burn(address sponsor, address indexed onBehalfOf, uint256 amount, uint256 timestamp);

    event LiquidationRecord(address provider, address keeper, address indexed onBehalfOf, uint256 eusdamount, uint256 LiquidateAssetAmount, uint256 keeperReward, bool superLiquidation, uint256 timestamp);

    event RigidRedemption(address indexed caller, address indexed provider, uint256 peusdAmount, uint256 assetAmount, uint256 timestamp);

    event FeeDistribution(address indexed feeAddress, uint256 feeAmount, uint256 timestamp);
```

## [G-20] use `mappings` instead of arrays

Arrays are useful when you need to maintain an ordered list of data that can be iterated over, but they have a higher gas cost for read and write operations, especially when the size of the array is large. This is because Solidity needs to iterate over the entire array to perform certain operations, such as finding a specific element or deleting an element.

Mappings, on the other hand, are useful when you need to store and access data based on a key, rather than an index. Mappings have a lower gas cost for read and write operations, especially when the size of the mapping is large, since Solidity can perform these operations based on the key directly, without needing to iterate over the entire data structure.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol#L8

```solidity
File: /contracts/lybra/miner/esLBRBoost.sol

8    esLBRLockSetting[] public esLBRLockSettings;
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L33

```solidity
File: /contracts/lybra/miner/EUSDMiningIncentives.sol

33   address[] public pools;
```
