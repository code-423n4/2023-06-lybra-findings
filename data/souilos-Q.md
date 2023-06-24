
# VULN 1 

## [LOW] Keccak Constant values should used to immutable rather than constant
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 76 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    bytes32 public constant DAO = keccak256("DAO");


Found in line 77 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");


Found in line 78 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

    bytes32 public constant ADMIN = keccak256("ADMIN");


Found in line 10 at 2023-06-lybra-finance/contracts/governance/GovernanceTimelock.sol:

    bytes32 public constant DAO = keccak256("DAO");


Found in line 11 at 2023-06-lybra-finance/contracts/governance/GovernanceTimelock.sol:

    bytes32 public constant TIMELOCK = keccak256("TIMELOCK");


Found in line 12 at 2023-06-lybra-finance/contracts/governance/GovernanceTimelock.sol:

    bytes32 public constant ADMIN = keccak256("ADMIN");


Found in line 13 at 2023-06-lybra-finance/contracts/governance/GovernanceTimelock.sol:

    bytes32 public constant GOV = keccak256("GOV");

------------------------------------------------------------------------ 

### Mitigation 

By using immutable variables, the Keccak256 hash is computed at contract deployment time rather than at compilation time. This means that the value can be updated if the algorithm changes in a future compiler version, without breaking backward compatibility. Additionally, it provides better gas optimization, as the Keccak256 hash is computed only once at contract deployment instead of every time the variable is accessed during execution.










# VULN 2 

## [LOW] Use .call instead of .transfer to send ether
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 292 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

            peUSD.transfer(address(lybraProtocolRewardsPool), peUSDBalance);


Found in line 299 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

                EUSD.transfer(address(lybraProtocolRewardsPool), balance);


Found in line 304 at 2023-06-lybra-finance/contracts/configuration/LybraConfigurator.sol:

                IEUSD(stableToken).transfer(address(lybraProtocolRewardsPool), amount);


Found in line 97 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

        stakingToken.transfer(msg.sender, _amount);


Found in line 202 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

                    peUSD.transfer(msg.sender, reward - eUSDShare);


Found in line 206 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

                        peUSD.transfer(msg.sender, peUSDBalance);


Found in line 210 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

                    token.transfer(msg.sender, tokenAmount);


Found in line 93 at 2023-06-lybra-finance/contracts/pools/LybraStETHVault.sol:

        collateralAsset.transfer(msg.sender, realAmount);


Found in line 139 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

            collateralAsset.transfer(msg.sender, reducedAsset);


Found in line 142 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

            collateralAsset.transfer(provider, reducedAsset - reward2keeper);


Found in line 143 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

            collateralAsset.transfer(msg.sender, reward2keeper);


Found in line 166 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        collateralAsset.transfer(msg.sender, collateralAmount);


Found in line 215 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

        collateralAsset.transfer(_onBehalfOf, _amount);


Found in line 107 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        collateralAsset.transfer(onBehalfOf, withdrawal);


Found in line 169 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

            collateralAsset.transfer(msg.sender, reducedAsset);


Found in line 172 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

            collateralAsset.transfer(provider, reducedAsset - reward2keeper);


Found in line 173 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

            collateralAsset.transfer(msg.sender, reward2keeper);


Found in line 206 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

            collateralAsset.transfer(msg.sender, reward2keeper);


Found in line 208 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        collateralAsset.transfer(provider, assetAmount - reward2keeper);


Found in line 242 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

        collateralAsset.transfer(msg.sender, collateralAmount);

------------------------------------------------------------------------ 

### Mitigation 

.transfer will relay 2300 gas and .call will relay all the gas. If the receive/fallback function from the recipient proxy contract has complex logic, using .transfer will fail, causing integration issues.Replace .transfer with .call. Note that the result of .call need to be checked.










# VULN 3 

## [LOW] Use the safe variant and ERC721.mint
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 28 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        _mint(user, amount);


Found in line 57 at 2023-06-lybra-finance/contracts/token/LBR.sol:

        _mint(_toAddress, _amount);


Found in line 34 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

        _mint(user, amount);


Found in line 65 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        _mint(to, amount);


Found in line 87 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        _mint(user, eusdAmount);


Found in line 192 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

        _mint(_toAddress, _amount);


Found in line 39 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

        _mint(_toAddress, _amount);

------------------------------------------------------------------------ 

### Mitigation 

.mint won’t check if the recipient is able to receive the NFT. If an incorrect address is passed, it will result in a silent failure and loss of asset. OpenZeppelin recommendation is to use the safe variant of _mint. Replace _mint() with _safeMint().










# VULN 4 

## [LOW] Immutables should be in uppercase
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 14 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    Iconfigurator public immutable configurator;


Found in line 16 at 2023-06-lybra-finance/contracts/token/LBR.sol:

    uint internal immutable ld2sdRatio;


Found in line 18 at 2023-06-lybra-finance/contracts/token/esLBR.sol:

    Iconfigurator public immutable configurator;


Found in line 39 at 2023-06-lybra-finance/contracts/token/EUSD.sol:

    Iconfigurator public immutable configurator;


Found in line 30 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    IEUSD public immutable EUSD;


Found in line 31 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    Iconfigurator public immutable configurator;


Found in line 32 at 2023-06-lybra-finance/contracts/token/PeUSDMainnetStableVision.sol:

    uint internal immutable ld2sdRatio;


Found in line 9 at 2023-06-lybra-finance/contracts/token/PeUSD.sol:

    uint internal immutable ld2sdRatio;


Found in line 18 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

    IERC20 public immutable stakingToken;


Found in line 19 at 2023-06-lybra-finance/contracts/miner/stakerewardV2pool.sol:

    IesLBR public immutable rewardsToken;


Found in line 25 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    Iconfigurator public immutable configurator;


Found in line 39 at 2023-06-lybra-finance/contracts/miner/ProtocolRewardsPool.sol:

    uint256 immutable exitCycle = 90 days;


Found in line 28 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    Iconfigurator public immutable configurator;


Found in line 30 at 2023-06-lybra-finance/contracts/miner/EUSDMiningIncentives.sol:

    IEUSD public immutable EUSD;


Found in line 22 at 2023-06-lybra-finance/contracts/pools/LybraWstETHVault.sol:

    Ilido immutable lido;


Found in line 14 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    IPeUSD public immutable PeUSD;


Found in line 15 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    IERC20 public immutable collateralAsset;


Found in line 16 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    Iconfigurator public immutable configurator;


Found in line 18 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    uint8 immutable vaultType = 1;


Found in line 19 at 2023-06-lybra-finance/contracts/pools/base/LybraPeUSDVaultBase.sol:

    IPriceFeed immutable etherOracle;


Found in line 18 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    IEUSD public immutable EUSD;


Found in line 19 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    IERC20 public immutable collateralAsset;


Found in line 20 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    Iconfigurator public immutable configurator;


Found in line 21 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    uint256 public immutable badCollateralRatio = 150 * 1e18;


Found in line 22 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    IPriceFeed immutable etherOracle;


Found in line 30 at 2023-06-lybra-finance/contracts/pools/base/LybraEUSDVaultBase.sol:

    uint8 immutable vaultType = 0;

------------------------------------------------------------------------ 

### Mitigation 

Immutables should be in uppercase, it helps to distinguish immutables from other types of variables and provides better code readability.
