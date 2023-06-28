contracts/lybra/configuration
/LybraConfigurator.sol


// SPDX-License-Identifier: BUSL-1.1

/**
 * @title Lybra Protocol V2 Configurator Contract
 * @dev The Configurator contract is used to set various parameters and control functionalities of the Lybra Protocol. It is based on OpenZeppelin's Proxy and AccessControl libraries, allowing the DAO to control contract upgrades. There are three types of governance roles:
 * * DAO: A time-locked contract initiated by esLBR voting, with a minimum effective period of 14 days. After the vote is passed, only the developer can execute the action.
 * * TIMELOCK: A time-locked contract controlled by the developer, with a minimum effective period of 2 days.
 * * ADMIN: A multisignature account controlled by the developer.
 * All setting functions have three levels of calling permissions:
 * * onlyRole(DAO): Only callable by the DAO for governance purposes.
 * * checkRole(TIMELOCK): Callable by both the DAO and the TIMELOCK contract.
 * * checkRole(ADMIN): Callable by all governance roles.
 */

pragma solidity ^0.8.17;

import "../interfaces/IGovernanceTimelock.sol";
import "../interfaces/IEUSD.sol";

interface IProtocolRewardsPool {
    function notifyRewardAmount(uint256 amount, uint256 tokenType) external;
}

interface IeUSDMiningIncentives {
    function refreshReward(address user) external;
}

interface IVault {
    function vaultType() external view returns (uint8);
}

interface ICurvePool {
    function get_dy_underlying(int128 i, int128 j, uint256 dx) external view returns (uint256);
    function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256);
}

contract Configurator {
    mapping(address => bool) public mintVault;
    mapping(address => uint256) public mintVaultMaxSupply;
    mapping(address => bool) public vaultMintPaused;
    mapping(address => bool) public vaultBurnPaused;
    mapping(address => uint256) vaultSafeCollateralRatio;
    mapping(address => uint256) vaultBadCollateralRatio;
    mapping(address => uint256) public vaultMintFeeApy;
    mapping(address => uint256) public vaultKeeperRatio;
    mapping(address => bool) redemptionProvider;
    mapping(address => bool) public tokenMiner;

    uint256 public redemptionFee = 50;
    IGovernanceTimelock public GovernanceTimelock;

    IeUSDMiningIncentives public eUSDMiningIncentives;
    IProtocolRewardsPool public lybraProtocolRewardsPool;
    IEUSD public EUSD;
    IEUSD public peUSD;
    uint256 public flashloanFee = 500;
    // Limiting the maximum percentage of eUSD that can be cross-chain transferred to L2 in relation to the total supply.
    uint256 public maxStableRatio = 5_000;
    address public stableToken;
    ICurvePool public curvePool;
    bool public premiumTradingEnabled;

    event RedemptionFeeChanged(uint256 newSlippage);
    event SafeCollateralRatioChanged(address indexed pool, uint256 newRatio);
    event RedemptionProvider(address indexed user, bool status);
    event ProtocolRewardsPoolChanged(address indexed pool, uint256 timestamp);
    event EUSDMiningIncentivesChanged(address indexed pool, uint256 timestamp);
    event BorrowApyChanged(address indexed pool, uint256 newApy);
    event KeeperRatioChanged(address indexed pool, uint256 newSlippage);
    event TokenMinerChanges(address indexed pool, bool status);

    bytes32 public constant DAO_ROLE = keccak256("DAO_ROLE");
    bytes32 public constant TIMELOCK_ROLE = keccak256("TIMELOCK_ROLE");
    bytes32 public constant ADMIN_ROLE = keccak256("ADMIN_ROLE");

    modifier onlyRole(bytes32 role) {
        require(hasRole(role, msg.sender), "Caller does not have the required role");
        _;
    }

    modifier checkRole(bytes32 role) {
        if (!hasRole(role, msg.sender)) {
            require(
                hasRole(TIMELOCK_ROLE, msg.sender) || hasRole(DAO_ROLE, msg.sender),
                "Caller does not have the required role"
            );
        }
        _;
    }

    constructor(
        address _governanceTimelock,
        address _eUSDMiningIncentives,
        address _lybraProtocolRewardsPool,
        address _EUSD,
        address _peUSD,
        address _stableToken,
        address _curvePool
    ) {
        GovernanceTimelock = IGovernanceTimelock(_governanceTimelock);
        eUSDMiningIncentives = IeUSDMiningIncentives(_eUSDMiningIncentives);
        lybraProtocolRewardsPool = IProtocolRewardsPool(_lybraProtocolRewardsPool);
        EUSD = IEUSD(_EUSD);
        peUSD = IEUSD(_peUSD);
        stableToken = _stableToken;
        curvePool = ICurvePool(_curvePool);
    }

    /**
     * @dev Set the redemption fee percentage.
     * @param newRedemptionFee The new redemption fee percentage (in basis points).
     */
    function setRedemptionFee(uint256 newRedemptionFee) external onlyRole(DAO_ROLE) {
        require(newRedemptionFee <= 1_000, "Redemption fee must be less than or equal to 10%");
        redemptionFee = newRedemptionFee;
        emit RedemptionFeeChanged(newRedemptionFee);
    }

    /**
     * @dev Set the safe collateral ratio for a specific pool.
     * @param pool The address of the pool.
     * @param newRatio The new safe collateral ratio (in basis points).
     */
    function setSafeCollateralRatio(address pool, uint256 newRatio) external checkRole(ADMIN_ROLE) {
        require(newRatio <= 10_000, "Safe collateral ratio must be less than or equal to 100%");
        vaultSafeCollateralRatio[pool] = newRatio;
        emit SafeCollateralRatioChanged(pool, newRatio);
    }

    /**
     * @dev Enable or disable a redemption provider.
     * @param user The address of the redemption provider.
     * @param status The status to set (true for enabled, false for disabled).
     */
    function setRedemptionProvider(address user, bool status) external onlyRole(DAO_ROLE) {
        redemptionProvider[user] = status;
        emit RedemptionProvider(user, status);
    }

    /**
     * @dev Set the Lybra Protocol rewards pool contract address.
     * @param pool The address of the rewards pool contract.
     */
    function setProtocolRewardsPool(address pool) external checkRole(ADMIN_ROLE) {
        lybraProtocolRewardsPool = IProtocolRewardsPool(pool);
        emit ProtocolRewardsPoolChanged(pool, block.timestamp);
    }

    /**
     * @dev Set the eUSD mining incentives contract address.
     * @param incentives The address of the eUSD mining incentives contract.
     */
    function setEUSDMiningIncentives(address incentives) external checkRole(ADMIN_ROLE) {
        eUSDMiningIncentives = IeUSDMiningIncentives(incentives);
        emit EUSDMiningIncentivesChanged(incentives, block.timestamp);
    }

    /**
     * @dev Set the borrowing APY for a specific vault.
     * @param pool The address of the vault.
     * @param newApy The new borrowing APY.
     */
    function setBorrowApy(address pool, uint256 newApy) external onlyRole(DAO_ROLE) {
        vaultMintFeeApy[pool] = newApy;
        emit BorrowApyChanged(pool, newApy);
    }

    /**
     * @dev Set the keeper ratio for a specific vault.
     * @param pool The address of the vault.
     * @param newRatio The new keeper ratio (in basis points).
     */
    function setKeeperRatio(address pool, uint256 newRatio) external checkRole(ADMIN_ROLE) {
        require(newRatio <= 1_000, "Keeper ratio must be less than or equal to 10%");
        vaultKeeperRatio[pool] = newRatio;
        emit KeeperRatioChanged(pool, newRatio);
    }

    /**
     * @dev Enable or disable token mining for a specific token.
     * @param token The address of the token.
     * @param status The status to set (true for enabled, false for disabled).
     */
    function setTokenMiner(address token, bool status) external onlyRole(DAO_ROLE) {
        tokenMiner[token] = status;
        emit TokenMinerChanges(token, status);
    }
}
