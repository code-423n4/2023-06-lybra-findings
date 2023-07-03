# Summary
The main objective of this report was to minimize storage operations. As such, gas optimizations that dealt with storage were prioritized so as to provide the most value when juxtaposed with the findings in the Bot Race. Since no tests are available and specific benchmarking is not possible, all optimizations are explained via EVM gas costs and opcodes.

*Notes*: 

- Only optimizations to state-mutating functions and `view`/`pure` function invoked by state-mutating functions are highlighted below.
- Only runtime gas is highlighted below, as it will inevitably out-weight deployment gas costs throughout the lifetime of the protocol.
- Some code snippets may be truncated to save space. Code snippets may also be accompanied by @audit tags in comments to aid in explaining the issue.

## Gas Optimizations
| Number |Issue|Instances| Estimated Gas Saved |
|-|:-|:-:|:-:| 
| [G-01](#state-variables-can-be-cached-instead-of-re-reading-them-from-storage) | State variables can be cached instead of re-reading them from storage | 22 | 2200 |
| [G-02](#state-variables-only-set-during-construction-should-be-declared-constant) | State variables only set during construction should be declared constant | 2 | 4200 |
| [G-03](#state-variables-can-be-packed-into-fewer-storage-slots) | State variables can be packed into fewer storage slots | 5 | 22000 |
| [G-04](#structs-can-be-packed-into-fewer-storage-slots) | Structs can be packed into fewer storage slots | 1 | 2000 |
| [G-05](#cache-state-variables-outside-of-loop-to-avoid-reading-storage-on-every-iteration) | Cache state variables outside of loop to avoid reading storage on every iteration | 1 | 100 |
| [G-06](#use-calldata-instead-of-memory-for-function-parameters-that-dont-change) | Use `calldata` instead of `memory` for function parameters that don't change | 2 | 200 |
| [G-07](#cache-function-calls) | Cache function calls | 5 | 600 |
| [G-08](#refactor-functions-to-avoid-excessive-storage-reads) | Refactor functions to avoid excessive storage reads | 4 | 900 |
| [G-09](#avoid-emitting-event-on-every-iteration) | Avoid emitting event on every iteration | 1 | 375 |
| [G-10](#multiple-addressid-mappings-can-be-combined-into-a-single-mapping-of-an-addressid-to-a-struct-where-appropriate) | Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate | 1 | 22100 |

*Total Estimated Gas Saved: 54675*

## State variables can be cached instead of re-reading them from storage
Caching of a state variable replaces each `Gwarmaccess (100 gas)` with a much cheaper stack read.

*Note: These are instances missed by the Bot Race.*

Total Instances: `22`

Estimated Gas Saved: `22 * 100 = 2200`

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

*Note: This `view` function is invoked within state-mutating functions*
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

## State variables only set during construction should be declared constant
The solidity compiler will directly embed the values of constant variables into your contract bytecode and therefore will save you from incurring a `Gsset (20000 gas)` when you set storage variables during construction, a `Gcoldsload (2100 gas)` when you access storage variables for the first time in a transaction, and a `Gwarmaccess (100 gas)` for each subsequent access to that storage slot.

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

## State variables can be packed into fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if the values combined are <= 32 bytes). If the variables packed together are retrieved together in functions we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

Total Instances: `5`

Estimated Gas Saved: `11 (slots) * 2000 = 22000`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L38-L57

### Pack `finishAt`, `updatedAt`, `extraRatio`, `preUSDExtraRatio`, `biddingFeeRatio`, and `isEUSDBuyoutAllowed` into one storage slot to save 4 SLOTs (~8000 gas)

*Note: `finishAt` and `updatedAt` hold timestamps and therefore a `unit` type of `uint40` should be big enough to hold those values. `extraRatio`, `preUSDExtraRatio`, and `biddingFeeRatio` are all enforced to have a max upper bounds and can therefore be reduced to a lower `uint` type that can hold those max values (see Diff below).*
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

*Note: `redemptionFee`, `flashLoanFee`, and `maxStableRatio` are all enforced to have a max upper bounds and can each fit into a type `uint16`*
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

*Note: `grabFeeRatio` has a max upper bound and therefore its `uint` size can be reduced to a lower size so that it can be packed with an address (160 bits)*
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

*Note: The `uint` type for `finishAt` and `updatedAt` can be reduced since these variables hold timestamps*
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

*Note: This can only be done by rearranging so that the inherited storage slot is correct*

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

## Structs can be packed into fewer storage slots
The EVM works with 32 byte words. Variables less than 32 bytes can be declared next to eachother in storage and this will pack the values together into a single 32 byte storage slot (if values combined are <= 32 bytes). If the variables packed together are retrieved together in functions (more likely with structs) we will effectively save ~2000 gas with every subsequent SLOAD for that storage slot. This is due to us incurring a `Gwarmaccess (100 gas)` versus a `Gcoldsload (2100 gas)`.

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

## Cache state variables outside of loop to avoid reading storage on every iteration
Reading from storage should always try to be avoided within loops. In the following instances, we are able to cache state variables outside of the loop to save a `Gwarmaccess (100 gas)` per loop iteration.

Total Instances: `1`

Estimiated Gas Saved: `100`

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L136-L145

### Cache `peUSDExtraRatio` outside of the loop to save 1 SLOAD per iteration

*Note: This `view` function is invoked within other public `view` functions that themselves are invoked within state-mutating functions. Example: `getReward()` (state-mutating) invokes `isOtherEarningsClaimable` which in turn invokes `stakeOf`*
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

## Use `calldata` instead of `memory` for function parameters that don't change
When you specify a data location as `memory`, that value will be copied into memory. When you specify the location as `calldata`, the value will stay static within calldata. If the value is a large, complex type, using memory may result in extra memory expansion costs.

Total Instances: `2`

Estimated Gas Saved: `200` 

*Note: This is a commonly known high impact finding, however due to lack of available tests the exact gas costs can not be known. We will therefore be conservative in our projected gas savings and give each instance a a projected savings of 100 gas.*

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

## Cache function calls
External calls are expensive as they are performed via the `CALL`/`STATICCALL` opcodes. Calls to internal functions can also be expensive when the internal functions themselves read from storage and/or perform external calls. If a function call such as the ones explained above is performed more than once, it should be cached to avoid incurring the costs multiple times.

Total Instances: `5`

Estimated Gas Saved: `600`

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

## Refactor functions to avoid excessive storage reads
The functions below read storage slots that are previously read in the functions that invoke them. We can refactor the functions so we could pass cached storage variables as stack variables and avoid the extra storage reads that would otherwise take place in the public/internal functions.

Total Instances: `4`

Estimated Gas Saved: `900`

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

## Avoid emitting event on every iteration
Expensive operations should always try to be avoided within loops. Such operations include: reading/writing to storage, heavy calculations, external calls, and emitting events. In this instance, an event is being emitted every iteration. Events have a base cost of `Glog (375 gas)` per emit and `Glogdata (8 gas) * number of bytes in event`. We can avoid incurring those costs each iteration by emitting the event outside of the loop.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L236-L238

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol
236:        for (uint256 i = 0; i < _contracts.length; i++) {
237:            tokenMiner[_contracts[i]] = _bools[i];
238:            emit tokenMinerChanges(_contracts[i], _bools[i]); // @audit: emit on every iteration
```

## Multiple address/ID mappings can be combined into a single mapping of an address/ID to a struct, where appropriate 
We can combine multiple mappings below into structs. We can then pack the structs by modifying the uint type for the values. This will result in cheaper storage reads since multiple mappings are accessed in functions and those values are now occupying the same storage slot, meaning the slot will become warm after the first SLOAD. In addition, when writing to and reading from the struct values we will avoid a `Gsset (20000 gas)` and `Gcoldsload (2100 gas)` since multiple struct values are now occupying the same slot.

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