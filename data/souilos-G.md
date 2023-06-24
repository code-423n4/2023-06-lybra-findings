# VULN 1 

## [GAS] Use != 0 instead of > 0 for unsigned integer comparison
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 115 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");


Found in line 84 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(_amount > 0, "amount = 0");


Found in line 94 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(_amount > 0, "amount = 0");


Found in line 113 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        if (reward > 0) {


Found in line 140 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(rewardRatio > 0, "reward ratio = 0");


Found in line 102 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        if (amount > 0) {


Found in line 117 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        if (amount > 0) {


Found in line 132 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(amount > 0, "QMG");


Found in line 192 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        if (reward > 0) {


Found in line 205 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

                    if(peUSDBalance > 0) {


Found in line 230 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(amount > 0, "amount = 0");


Found in line 195 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        if (reward > 0) {


Found in line 208 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        if (reward > 0) {


Found in line 229 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(amount > 0, "amount = 0");


Found in line 237 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(rewardRatio > 0, "reward ratio = 0");


Found in line 33 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

        require(sharesAmount > 0, "ZERO_DEPOSIT");


Found in line 41 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

        if (mintAmount > 0) {


Found in line 34 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

        if (mintAmount > 0) {


Found in line 27 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

        if (mintAmount > 0) {


Found in line 41 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(sharesAmount > 0, "ZERO_DEPOSIT");


Found in line 47 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        if (mintAmount > 0) {


Found in line 64 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");


Found in line 65 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        if (mintAmount > 0) {


Found in line 84 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(amount > 0, "ZA");


Found in line 98 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(amount > 0, "ZA");


Found in line 112 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(amount > 0, "ZA");


Found in line 131 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 216 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        if (getBorrowedOf(_provider) > 0) {


Found in line 82 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        if (mintAmount > 0) {


Found in line 100 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(amount > 0, "ZERO_WITHDRAW");


Found in line 108 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        if (borrowed[msg.sender] > 0) {


Found in line 128 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(amount > 0, "ZERO_MINT");


Found in line 160 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");

------------------------------------------------------------------------ 

### Mitigation 

Use != 0 instead of > 0 for unsigned integer comparison.










# VULN 2 

## [GAS] Use shift Right/Left instead of division/multiplication if possible
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 357 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        return (EUSD.totalSupply() * maxStableRatio) / 10_000;


Found in line 157 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        return (share * configurator.flashloanFee()) / 10_000;


Found in line 134 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);


Found in line 152 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        amount = (a * (75e18 - ((time2fullRedemption[user] - block.timestamp) * 70e18) / (exitCycle / 1 days - 3) / 1 days)) / 100e18;


Found in line 210 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

            uint256 biddingFee = (reward * biddingFeeRatio) / 10000;


Found in line 66 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        uint256 payAmount = (((realAmount * getAssetPrice()) / 1e18) * getDutchAuctionDiscountPrice()) / 10000;


Found in line 104 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        return 10000 - (time / 30 minutes - 1) * 100;


Found in line 135 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        uint256 reducedAsset = (assetAmount * 11) / 10;


Found in line 164 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        uint256 collateralAmount = (((peusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;


Found in line 238 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;


Found in line 115 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        withdrawal = block.timestamp - 3 days >= depositedTime[user] ? amount : (amount * 999) / 1000;


Found in line 164 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        uint256 reducedAsset = (assetAmount * 11) / 10;


Found in line 239 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        uint256 collateralAmount = (((eusdAmount * 1e18) / assetPrice) * (10000 - configurator.redemptionFee())) / 10000;


Found in line 301 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;


Found in line 52 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        return esLBR.getPastTotalSupply(timepoint) / 3;

------------------------------------------------------------------------ 

### Mitigation 

Use shift Right/Left instead of division/multiplication if possible.










# VULN 3 

## [GAS] ++i/i++ should be unchecked{++i}/unchecked{i++} and ++i costs less gas than i++
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 236 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        for (uint256 i = 0; i < _contracts.length; i++) {


Found in line 94 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        for (uint i = 0; i < _pools.length; i++) {


Found in line 138 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        for (uint i = 0; i < pools.length; i++) {

------------------------------------------------------------------------ 

### Mitigation 

++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for and while-loops. Moreover ++i costs less gas than i++, especially when its used in for-loops (--i/i-- too).










# VULN 4 

## [GAS] Don’t initialize variables with default value
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 236 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        for (uint256 i = 0; i < _contracts.length; i++) {


Found in line 94 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        for (uint i = 0; i < _pools.length; i++) {


Found in line 138 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        for (uint i = 0; i < pools.length; i++) {


Found in line 30 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    uint8 immutable vaultType = 0;

------------------------------------------------------------------------ 

### Mitigation 

In such cases, initializing the variables with default values would be unnecessary and can be considered a waste of gas. Additionally, initializing variables with default values can sometimes lead to unnecessary storage operations, which can increase gas costs. For example, if you have a large array of variables, initializing them all with default values can result in a lot of unnecessary storage writes, which can increase the gas costs of your contract.










# VULN 5 

## [GAS] Use Custom Errors
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 127 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");


Found in line 187 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        require(newFee <= 500, "Max Redemption Fee is 5%");


Found in line 200 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

            require(newRatio >= 160 * 1e18, "eUSD vault safe collateralRatio should more than 160%");


Found in line 202 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

            require(newRatio >= vaultBadCollateralRatio[pool] + 1e19, "PeUSD vault safe collateralRatio should more than bad collateralRatio");


Found in line 214 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        require(newApy <= 200, "Borrow APY cannot exceed 2%");


Found in line 225 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        require(newRatio <= 5, "Max Keeper reward is 5%");


Found in line 247 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        require(_ratio <= 10_000, "The maximum value is 10000");


Found in line 21 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");


Found in line 26 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        require(configurator.tokenMiner(msg.sender), "not authorized");


Found in line 27 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");


Found in line 33 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        require(configurator.tokenMiner(msg.sender), "not authorized");


Found in line 27 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        revert("not authorized");


Found in line 31 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        require(configurator.tokenMiner(msg.sender), "not authorized");


Found in line 32 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        require(totalSupply() + amount <= maxSupply, "exceeding the maximum supply quantity.");


Found in line 39 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        require(configurator.tokenMiner(msg.sender), "not authorized");


Found in line 80 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(configurator.mintVault(msg.sender), "RCP");


Found in line 84 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(!configurator.vaultMintPaused(msg.sender), "MPP");


Found in line 88 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(!configurator.vaultBurnPaused(msg.sender), "BPP");


Found in line 180 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

            require(currentAllowance >= amount, "ERC20: insufficient allowance");


Found in line 271 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");


Found in line 367 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_owner != address(0), "APPROVE_FROM_ZERO_ADDRESS");


Found in line 368 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_spender != address(0), "APPROVE_TO_ZERO_ADDRESS");


Found in line 392 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_sender != address(0), "TRANSFER_FROM_THE_ZERO_ADDRESS");


Found in line 393 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_recipient != address(0), "TRANSFER_TO_THE_ZERO_ADDRESS");


Found in line 396 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_sharesAmount <= currentSenderShares, "TRANSFER_AMOUNT_EXCEEDS_BALANCE");


Found in line 412 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_recipient != address(0), "MINT_TO_THE_ZERO_ADDRESS");


Found in line 441 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");


Found in line 460 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");


Found in line 466 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(_sharesAmount <= accountShares, "BURN_AMOUNT_EXCEEDS_BALANCE");


Found in line 43 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(configurator.mintVault(msg.sender), "RCP");


Found in line 47 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(!configurator.vaultMintPaused(msg.sender), "MPP");


Found in line 51 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(!configurator.vaultBurnPaused(msg.sender), "BPP");


Found in line 59 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");


Found in line 64 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(to != address(0), "TZA");


Found in line 80 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(_msgSender() == user || _msgSender() == address(this), "MDM");


Found in line 81 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");


Found in line 83 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(success, "TF");


Found in line 115 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");


Found in line 134 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(success, "TF");


Found in line 13 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");


Found in line 42 at 2023-06-lybra-finance/contracts/miner/esLBRBoost.sol:

            require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");


Found in line 84 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(_amount > 0, "amount = 0");


Found in line 86 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(success, "TF");


Found in line 94 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(_amount > 0, "amount = 0");


Found in line 122 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(finishAt < block.timestamp, "reward duration not finished");


Found in line 140 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(rewardRatio > 0, "reward ratio = 0");


Found in line 59 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(_ratio <= 8000, "BCE");


Found in line 88 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");


Found in line 114 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");


Found in line 132 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(amount > 0, "QMG");


Found in line 230 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        require(amount > 0, "amount = 0");


Found in line 95 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

            require(configurator.mintVault(_pools[i]), "NOT_VAULT");


Found in line 101 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(_biddingRatio <= 8000, "BCE");


Found in line 106 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(ratio <= 1e20, "BCE");


Found in line 111 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(ratio <= 1e20, "BCE");


Found in line 120 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(finishAt < block.timestamp, "reward duration not finished");


Found in line 193 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(!isOtherEarningsClaimable(msg.sender), "Insufficient DLP, unable to claim rewards");


Found in line 203 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(isOtherEarningsClaimable(user), "The rewards of the user cannot be bought out");


Found in line 205 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

            require(isEUSDBuyoutAllowed, "The purchase using EUSD is not permitted.");


Found in line 215 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

                require(success, "TF");


Found in line 229 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(amount > 0, "amount = 0");


Found in line 237 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(rewardRatio > 0, "reward ratio = 0");


Found in line 30 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

        require(msg.value >= 1 ether, "DNL");


Found in line 33 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

        require(sharesAmount > 0, "ZERO_DEPOSIT");


Found in line 28 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

        require(msg.value >= 1 ether, "DNL");


Found in line 42 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

        require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender));


Found in line 21 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

        require(msg.value >= 1 ether, "DNL");


Found in line 26 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(configurator.hasRole(keccak256("ADMIN"), msg.sender), "not authorized");


Found in line 38 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(msg.value >= 1 ether, "DNL");


Found in line 41 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(sharesAmount > 0, "ZERO_DEPOSIT");


Found in line 64 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");


Found in line 71 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

            require(success, "TF");


Found in line 86 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

            require(success, "TF");


Found in line 59 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(assetAmount >= 1 ether, "Deposit should not be less than 1 collateral asset.");


Found in line 62 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");


Found in line 83 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(onBehalfOf != address(0), "TZA");


Found in line 84 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(amount > 0, "ZA");


Found in line 97 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(onBehalfOf != address(0), "TZA");


Found in line 98 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(amount > 0, "ZA");


Found in line 111 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(onBehalfOf != address(0), "TZA");


Found in line 112 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(amount > 0, "ZA");


Found in line 128 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");


Found in line 130 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");


Found in line 131 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 158 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");


Found in line 159 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(borrowed[provider] >= peusdAmount, "peusdAmount cannot surpass providers debt");


Found in line 162 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");


Found in line 174 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");


Found in line 213 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(depositedAsset[_provider] >= _amount, "Withdraw amount exceeds deposited amount.");


Found in line 227 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

            revert("collateralRatio is Below safeCollateralRatio");


Found in line 73 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(assetAmount >= 1 ether, "Deposit should not be less than 1 stETH.");


Found in line 76 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(success, "TF");


Found in line 99 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(onBehalfOf != address(0), "TZA");


Found in line 100 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(amount > 0, "ZERO_WITHDRAW");


Found in line 101 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(depositedAsset[msg.sender] >= amount, "Withdraw amount exceeds deposited amount.");


Found in line 127 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");


Found in line 128 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(amount > 0, "ZERO_MINT");


Found in line 141 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(onBehalfOf != address(0), "BURN_TO_THE_ZERO_ADDRESS");


Found in line 157 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(onBehalfOfCollateralRatio < badCollateralRatio, "Borrowers collateral ratio should below badCollateralRatio");


Found in line 159 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");


Found in line 160 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 189 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require((totalDepositedAsset * assetPrice * 100) / poolTotalEUSDCirculation < badCollateralRatio, "overallCollateralRatio should below 150%");


Found in line 191 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(onBehalfOfCollateralRatio < 125 * 1e18, "borrowers collateralRatio should below 125%");


Found in line 192 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(assetAmount <= depositedAsset[onBehalfOf], "total of collateral can be liquidated at most");


Found in line 197 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");


Found in line 233 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(configurator.isRedemptionProvider(provider), "provider is not a RedemptionProvider");


Found in line 234 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(borrowed[provider] >= eusdAmount, "eusdAmount cannot surpass providers debt");


Found in line 237 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(providerCollateralRatio >= 100 * 1e18, "provider's collateral ratio should more than 100%");


Found in line 260 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");


Found in line 292 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");


Found in line 78 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        require(state(proposalId) == ProposalState.Active, "GovernorBravo::castVoteInternal: voting is closed");


Found in line 79 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        require(support <= 2, "GovernorBravo::castVoteInternal: invalid vote type");


Found in line 82 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");


Found in line 107 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        require(GovernanceTimelock.checkOnlyRole(keccak256("TIMELOCK"), msg.sender), "not authorized");


Found in line 152 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

       require(clock() == block.number, "Votes: broken clock mode");

------------------------------------------------------------------------ 

### Mitigation 

Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.










# VULN 6 

## [GAS] Use calldata instead of memory for function arguments that do not get mutated
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 33 at 2023-06-lybra-finance/contracts/miner/esLBRBoost.sol:

    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {


Found in line 93 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setPools(address[] memory _pools) external onlyOwner {


Found in line 76 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {


Found in line 98 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

     function _getVotes(address account, uint256 timepoint, bytes memory) internal view override returns (uint256){


Found in line 106 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function _execute(uint256 /* proposalId */, address[] memory targets, uint256[] memory values, bytes[] memory calldatas, bytes32 descriptionHash) internal virtual override {

------------------------------------------------------------------------ 

### Mitigation 

Mark data types as calldata instead of memory where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as calldata. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies memory storage.










# VULN 7 

## [GAS] Cache array length outside of loop
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 236 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        for (uint256 i = 0; i < _contracts.length; i++) {


Found in line 94 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        for (uint i = 0; i < _pools.length; i++) {


Found in line 138 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        for (uint i = 0; i < pools.length; i++) {

------------------------------------------------------------------------ 

### Mitigation 

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).










# VULN 8 

## [GAS] Use assembly to check for address(0)
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 99 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        if (address(EUSD) == address(0)) EUSD = IEUSD(_eusd);


Found in line 100 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        if (address(peUSD) == address(0)) peUSD = IEUSD(_peusd);

------------------------------------------------------------------------ 

### Mitigation 

Using assembly to check for the zero address can result in significant gas savings compared to using a Solidity expression, especially if the check is performed frequently or in a loop. However, it's important to note that using assembly can make the code less readable and harder to maintain, so it should be used judiciously and with caution.










# VULN 9 

## [GAS] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 29 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    function vaultType() external view returns (uint8);


Found in line 18 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("LBR", "LBR") BaseOFTV2(_sharedDecimals, _lzEndpoint) {


Found in line 20 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        uint8 decimals = decimals();


Found in line 49 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {


Found in line 56 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {


Found in line 114 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

    function decimals() public pure returns (uint8) {


Found in line 55 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    constructor(address _config, uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "PeUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {


Found in line 58 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        uint8 decimals = decimals();


Found in line 99 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        uint16 dstChainId,


Found in line 184 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {


Found in line 191 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {


Found in line 11 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

    constructor(uint8 _sharedDecimals, address _lzEndpoint) ERC20("peg-eUSD", "peUSD") BaseOFTV2(_sharedDecimals, _lzEndpoint) {


Found in line 12 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

        uint8 decimals = decimals();


Found in line 31 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

    function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {


Found in line 38 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

    function _creditTo(uint16, address _toAddress, uint _amount) internal virtual override returns (uint) {


Found in line 18 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    uint8 immutable vaultType = 1;


Found in line 265 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    function getVaultType() external pure returns (uint8) {


Found in line 30 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    uint8 immutable vaultType = 0;


Found in line 323 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    function getVaultType() external pure returns (uint8) {


Found in line 20 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        uint8 support;


Found in line 29 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        mapping(uint8 => uint256) supportVotes;


Found in line 76 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function _countVote(uint256 proposalId, address account, uint8 support, uint256 weight, bytes memory) internal override {


Found in line 129 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function getReceipt(uint256 proposalId, address account) external view returns (bool voted, uint8 support, uint256 votes){  

------------------------------------------------------------------------ 

### Mitigation 

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html. Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.










# VULN 10 

## [GAS] Public Functions to external
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 38 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    function circulatingSupply() public view virtual override returns (uint) {


Found in line 42 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    function token() public view virtual override returns (address) {


Found in line 141 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    function transferFrom(address from, address to, uint256 amount) public override returns (bool) {


Found in line 171 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    function circulatingSupply() public view virtual override returns (uint) {


Found in line 175 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    function token() public view virtual override returns (address) {


Found in line 20 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

    function circulatingSupply() public view virtual override returns (uint) {


Found in line 24 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

    function token() public view virtual override returns (address) {


Found in line 48 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

    function getAssetPrice() public override returns (uint256) {


Found in line 46 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

    function getAssetPrice() public override returns (uint256) {


Found in line 34 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

    function getAssetPrice() public override returns (uint256) {


Found in line 107 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

    function getAssetPrice() public override returns (uint256) {


Found in line 51 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function quorum(uint256 timepoint) public view override returns (uint256){


Found in line 143 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function votingPeriod() public pure override returns (uint256){


Found in line 147 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

     function votingDelay() public pure override returns (uint256){


Found in line 151 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

     function CLOCK_MODE() public override view returns (string memory){


Found in line 156 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function COUNTING_MODE() public override view virtual returns (string memory){


Found in line 161 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function clock() public override view returns (uint48){


Found in line 165 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function hasVoted(uint256 proposalId, address account) public override view virtual returns (bool){


Found in line 172 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function proposalThreshold() public pure override returns (uint256) {


Found in line 179 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

    function supportsInterface(bytes4 interfaceId) public view virtual override(GovernorTimelockControl) returns (bool) {

------------------------------------------------------------------------ 

### Mitigation 

The following functions could be set external to save gas and improve code quality. External call cost is less expensive than of public functions.










# VULN 11 

## [GAS] <x> += <y> Costs More Gas Than <x> = <x> + <y> For State Variables
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 425 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        _totalSupply += _mintAmount;


Found in line 85 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        userConvertInfo[user].depositedEUSDShares += share;


Found in line 86 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        userConvertInfo[user].mintedPeUSD += eusdAmount;


Found in line 87 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        balanceOf[msg.sender] += _amount;


Found in line 88 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        totalSupply += _amount;


Found in line 93 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

            total += unstakeRatio[msg.sender] * (time2fullRedemption[msg.sender] - block.timestamp);


Found in line 122 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

        grabableAmount += burnAmount;


Found in line 144 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

            amount += borrowed;


Found in line 39 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

        depositedAsset[msg.sender] += wstETHAmount;


Found in line 32 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

        depositedAsset[msg.sender] += balance - preBalance;


Found in line 25 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

        depositedAsset[msg.sender] += balance - preBalance;


Found in line 43 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        totalDepositedAsset += msg.value;


Found in line 44 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        depositedAsset[msg.sender] += msg.value;


Found in line 64 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        depositedAsset[msg.sender] += assetAmount;


Found in line 179 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        borrowed[_provider] += _mintAmount;


Found in line 182 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        poolTotalPeUSDCirculation += _mintAmount;


Found in line 232 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

            feeStored[user] += _newFee(user);


Found in line 78 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        totalDepositedAsset += assetAmount;


Found in line 79 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        depositedAsset[msg.sender] += assetAmount;


Found in line 262 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        borrowed[_provider] += _mintAmount;


Found in line 266 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        poolTotalEUSDCirculation += _mintAmount;


Found in line 296 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        feeStored += _newFee();


Found in line 84 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        proposalExtraData.supportVotes[support] += weight;


Found in line 90 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        proposalExtraData.totalVotes += weight;

------------------------------------------------------------------------ 

### Mitigation 

When you use the += operator on a state variable, the EVM has to perform three operations: load the current value of the state variable, add the new value to it, and then store the result back in the state variable. On the other hand, when you use the = operator and then add the values separately, the EVM only needs to perform two operations: load the current value of the state variable and add the new value to it. Better use <x> = <x> + <y> For State Variables.










# VULN 12 

## [GAS] Multiple Address Mappings Can Be Combined Into A Single Mapping Of An Address To A Struct, Where Appropriate
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 38 and 39 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    mapping(address => bool) public mintVault;

    mapping(address => uint256) public mintVaultMaxSupply;


Found in line 40 and 41 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    mapping(address => bool) public vaultMintPaused;

    mapping(address => bool) public vaultBurnPaused;


Found in line 42 and 43 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    mapping(address => uint256) vaultSafeCollateralRatio;

    mapping(address => uint256) vaultBadCollateralRatio;


Found in line 44 and 45 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    mapping(address => uint256) public vaultMintFeeApy;

    mapping(address => uint256) public vaultKeeperRatio;


Found in line 46 and 47 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    mapping(address => bool) redemptionProvider;

    mapping(address => bool) public tokenMiner;


Found in line 34 and 35 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

    // User address => rewards to be claimed

    mapping(address => uint256) public rewards;


Found in line 34 and 35 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    // User address => rewards to be claimed

    mapping(address => uint) public rewards;


Found in line 36 and 37 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    mapping(address => uint) public time2fullRedemption;

    mapping(address => uint) public unstakeRatio;


Found in line 47 and 48 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    // User address => rewards to be claimed

    mapping(address => uint256) public rewards;


Found in line 21 and 22 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    mapping(address => uint256) public depositedAsset;

    mapping(address => uint256) borrowed;


Found in line 23 and 24 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    mapping(address => uint256) feeStored;

    mapping(address => uint256) feeUpdatedAt;


Found in line 28 and 29 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    mapping(address => uint256) public depositedAsset;

    mapping(address => uint256) borrowed;


Found in line 28 and 29 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        mapping(address => Receipt)  receipts;

        mapping(uint8 => uint256) supportVotes;

------------------------------------------------------------------------ 

### Mitigation 

Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined. Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot.










# VULN 13 

## [GAS] Use hardcode address instead address(this)
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 290 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        uint256 peUSDBalance = peUSD.balanceOf(address(this));


Found in line 295 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        uint256 balance = EUSD.balanceOf(address(this));


Found in line 43 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        return address(this);


Found in line 64 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        if (_from != address(this) && _from != spender)


Found in line 80 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(_msgSender() == user || _msgSender() == address(this), "MDM");


Found in line 81 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(),"ESL");


Found in line 82 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        bool success = EUSD.transferFrom(user, address(this), eusdAmount);


Found in line 133 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        bool success = EUSD.transferFrom(address(receiver), address(this), EUSD.getMintedEUSDByShares(shareAmount));


Found in line 176 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        return address(this);


Found in line 199 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        if (_from != address(this) && _from != spender)


Found in line 25 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

        return address(this);


Found in line 46 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

        if (_from != address(this) && _from != spender)


Found in line 85 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        bool success = stakingToken.transferFrom(msg.sender, address(this), _amount);


Found in line 195 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

            uint256 balance = EUSD.sharesOf(address(this));


Found in line 200 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

                uint256 peUSDBalance = peUSD.balanceOf(address(this));


Found in line 29 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

        uint256 preBalance = collateralAsset.balanceOf(address(this));


Found in line 31 at 2023-06-lybra-finance/contracts/pools/LybraRETHVault.sol:

        uint256 balance = collateralAsset.balanceOf(address(this));


Found in line 22 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

        uint256 preBalance = collateralAsset.balanceOf(address(this));


Found in line 24 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

        uint256 balance = collateralAsset.balanceOf(address(this));


Found in line 63 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        uint256 excessAmount = collateralAsset.balanceOf(address(this)) - totalDepositedAsset;


Found in line 45 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        return collateralAsset.balanceOf(address(this));


Found in line 60 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        uint256 preBalance = collateralAsset.balanceOf(address(this));


Found in line 61 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        collateralAsset.transferFrom(msg.sender, address(this), assetAmount);


Found in line 62 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(collateralAsset.balanceOf(address(this)) >= preBalance + assetAmount, "");


Found in line 128 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(onBehalfOfCollateralRatio < configurator.getBadCollateralRatio(address(this)), "Borrowers collateral ratio should below badCollateralRatio");


Found in line 131 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 141 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;


Found in line 174 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(poolTotalPeUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");


Found in line 226 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        if (((depositedAsset[user] * price * 100) / getBorrowedOf(user)) < configurator.getSafeCollateralRatio(address(this))) 


Found in line 238 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        return (borrowed[user] * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - feeUpdatedAt[user])) / (86400 * 365) / 10000;


Found in line 75 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        bool success = collateralAsset.transferFrom(msg.sender, address(this), assetAmount);


Found in line 160 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 171 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

            reward2keeper = (reducedAsset * configurator.vaultKeeperRatio(address(this))) / 110;


Found in line 197 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");


Found in line 204 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        if (msg.sender != provider && onBehalfOfCollateralRatio >= 1e20 + configurator.vaultKeeperRatio(address(this)) * 1e18) {


Found in line 205 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

            reward2keeper = ((assetAmount * configurator.vaultKeeperRatio(address(this))) * 1e18) / onBehalfOfCollateralRatio;


Found in line 260 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(poolTotalEUSDCirculation + _mintAmount <= configurator.mintVaultMaxSupply(address(this)), "ESL");


Found in line 292 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        if (((depositedAsset[_user] * _assetPrice * 100) / borrowed[_user]) < configurator.getSafeCollateralRatio(address(this))) revert("collateralRatio is Below safeCollateralRatio");


Found in line 301 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        return (poolTotalEUSDCirculation * configurator.vaultMintFeeApy(address(this)) * (block.timestamp - lastReportTime)) / (86400 * 365) / 10000;


Found in line 20 at 2023-06-lybra-finance/contracts/governance/GovernanceTimelock.sol:

        _grantRole(DAO, address(this));

------------------------------------------------------------------------ 

### Mitigation 

Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this.










# VULN 14 

## [GAS] Functions guaranteed to revert when called by normal users can be marked payable
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 33 at 2023-06-lybra-finance/contracts/miner/esLBRBoost.sol:

    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {


Found in line 121 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

    function setRewardsDuration(uint256 _duration) external onlyOwner {


Found in line 127 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

    function setBoost(address _boost) external onlyOwner {


Found in line 132 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

    function notifyRewardAmount(uint256 _amount) external onlyOwner updateReward(address(0)) {


Found in line 52 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    function setTokenAddress(address _eslbr, address _lbr, address _boost) external onlyOwner {


Found in line 58 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    function setGrabCost(uint256 _ratio) external onlyOwner {


Found in line 84 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setToken(address _lbr, address _eslbr) external onlyOwner {


Found in line 89 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setLBROracle(address _lbrOracle) external onlyOwner {


Found in line 93 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setPools(address[] memory _pools) external onlyOwner {


Found in line 100 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setBiddingCost(uint256 _biddingRatio) external onlyOwner {


Found in line 105 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setExtraRatio(uint256 ratio) external onlyOwner {


Found in line 110 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setPeUSDExtraRatio(uint256 ratio) external onlyOwner {


Found in line 115 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setBoost(address _boost) external onlyOwner {


Found in line 119 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setRewardsDuration(uint256 _duration) external onlyOwner {


Found in line 124 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setEthlbrStakeInfo(address _pool, address _lp) external onlyOwner {


Found in line 128 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function setEUSDBuyoutAllowed(bool _bool) external onlyOwner {

------------------------------------------------------------------------ 

### Mitigation 

If a function modifier or require such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.










# VULN 15 

## [GAS] Using private rather than public for constants, saves gas
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 21 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    uint256 public immutable badCollateralRatio = 150 * 1e18;

------------------------------------------------------------------------ 

### Mitigation 

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table.










# VULN 16 

## [GAS] >= costs less gas than >
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 189 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        return (stakedLBRLpValue(user) * 10000) / stakedOf(user) < 500;


Found in line 103 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        if (time < 30 minutes) return 10000;


Found in line 67 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        return proposalData[proposalId].supportVotes[1] > proposalData[proposalId].supportVotes[0];

------------------------------------------------------------------------ 

### Mitigation 

The compiler uses opcodes GT and ISZERO for solidity code that uses >, but only requires LT for >=, which saves 3 gas.










# VULN 17 

## [GAS] Empty blocks should be removed or emit something
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 33 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}


Found in line 40 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        try IProtocolRewardsPool(configurator.getProtocolRewardsPool()).refreshReward(user) {} catch {}


Found in line 9 at 2023-06-lybra-finance/contracts/Proxy/LybraProxy.sol:

    constructor(address _logic,address _admin,bytes memory _data) TransparentUpgradeableProxy(_logic, _admin, _data) {}


Found in line 6 at 2023-06-lybra-finance/contracts/Proxy/LybraProxyAdmin.sol:

contract LybraProxyAdmin is ProxyAdmin {}

Found in line 184 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    function refreshReward(address _account) external updateReward(_account) {}


Found in line 174 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    function refreshReward(address _account) external updateReward(_account) {}


Found in line 216 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

                try configurator.distributeRewards() {} catch {}


Found in line 18 at 2023-06-lybra-finance/contracts/pools/LybraWbETHVault.sol:

        LybraPeUSDVaultBase(_peusd, _oracle, _asset, _config) {}


Found in line 73 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

            try configurator.distributeRewards() {} catch {}


Found in line 87 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

            try configurator.distributeRewards() {} catch {}


Found in line 177 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        try configurator.refreshMintReward(_provider) {} catch {}


Found in line 193 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        try configurator.refreshMintReward(_onBehalfOf) {} catch {}


Found in line 205 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        try configurator.distributeRewards() {} catch {}


Found in line 261 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        try configurator.refreshMintReward(_provider) {} catch {}


Found in line 280 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        try configurator.refreshMintReward(_onBehalfOf) {} catch {}


Found in line 8 at 2023-06-lybra-finance/contracts/governance/AdminTimelock.sol:

    constructor(uint256 minDelay, address[] memory proposers, address[] memory executors, address admin) TimelockController(minDelay, proposers, executors, admin) {}

------------------------------------------------------------------------ 

### Mitigation 

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}}).










# VULN 18 

## [GAS] Use double require instead of using &&
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 127 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

        require(newRatio >= 130 * 1e18 && newRatio <= 150 * 1e18 && newRatio <= vaultSafeCollateralRatio[pool] + 1e19, "LNA");



Found in line 115 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD &&peusdAmount > 0, "PCE");



Found in line 64 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        require(excessAmount > 0 && stETHAmount > 0, "Only LSD excess income can be exchanged");


------------------------------------------------------------------------ 

### Mitigation 

Using double require instead of operator && can save more gas. When having a require statement with 2 or more expressions needed, place the expression that cost less gas first.










# VULN 19 

## [GAS] require() or revert() statements that check input arguments should be at the top of the function (Also restructured some if’s)
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 21 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");


Found in line 80 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(configurator.mintVault(msg.sender), "RCP");


Found in line 84 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(!configurator.vaultMintPaused(msg.sender), "MPP");


Found in line 88 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

        require(!configurator.vaultBurnPaused(msg.sender), "BPP");


Found in line 43 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(configurator.mintVault(msg.sender), "RCP");


Found in line 47 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(!configurator.vaultMintPaused(msg.sender), "MPP");


Found in line 51 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(!configurator.vaultBurnPaused(msg.sender), "BPP");


Found in line 59 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");


Found in line 13 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

        require(_sharedDecimals <= decimals, "OFT: sharedDecimals must be <= decimals");


Found in line 140 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        require(rewardRatio > 0, "reward ratio = 0");


Found in line 215 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

                require(success, "TF");


Found in line 237 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

        require(rewardRatio > 0, "reward ratio = 0");


Found in line 71 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

            require(success, "TF");


Found in line 86 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

            require(success, "TF");


Found in line 131 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        require(PeUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 160 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) > 0, "provider should authorize to provide liquidation EUSD");


Found in line 197 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        require(EUSD.allowance(provider, address(this)) >= eusdAmount, "provider should authorize to provide liquidation EUSD");


Found in line 82 at 2023-06-lybra-finance/contracts/governance/LybraGovernance.sol:

        require(receipt.hasVoted == false, "GovernorBravo::castVoteInternal: voter already voted");

------------------------------------------------------------------------ 

### Mitigation 

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting alot of gas in a function that may ultimately revert in the unhappy case.
