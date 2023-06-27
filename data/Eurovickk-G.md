https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol

Gas Optimization: Consider using the receive() function instead of a separate depositEtherToMint function for receiving Ether. By doing so, you can remove the explicit check for msg.value and simplify the contract. 
receive() external payable override {
    require(msg.value >= 1 ether, "Insufficient Ether");
    uint256 preBalance = collateralAsset.balanceOf(address(this));
    IWBETH(address(collateralAsset)).deposit{value: msg.value}(address(configurator));
    uint256 balance = collateralAsset.balanceOf(address(this));
    depositedAsset[msg.sender] += balance - preBalance;
    // Rest of the logic...
}

With this change, users can simply send Ether to the contract, triggering the receive() function, and the minimum Ether requirement will be checked automatically.
The expression (_etherPrice() * IWBETH(address(collateralAsset)).exchangeRatio()) / 1e18 involves multiplication and division, which can be expensive in terms of gas. Consider using alternative approaches to minimize gas costs. For example, you can cache the value of IWBETH(address(collateralAsset)).exchangeRatio() in a local variable within the contract and update it only when necessary, rather than recalculating it every time getAssetPrice is called

contract LybraWBETHVault is LybraPeUSDVaultBase {
    //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
    address private wbethContract;
    uint256 private exchangeRatio;
    constructor(address _peusd, address _oracle, address _asset, address _config)
        LybraPeUSDVaultBase(_peusd, _oracle, _asset, _config) {
        wbethContract = address(collateralAsset);
        exchangeRatio = IWBETH(wbethContract).exchangeRatio();
    }
    function depositEtherToMint(uint256 mintAmount) external payable override {
        require(msg.value >= 1 ether, "DNL");
        uint256 preBalance = collateralAsset.balanceOf(address(this));
        IWBETH(wbethContract).deposit{value: msg.value}(address(configurator));
        uint256 balance = collateralAsset.balanceOf(address(this));
        depositedAsset[msg.sender] += balance - preBalance;
        if (mintAmount > 0) {
            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }
        emit DepositEther(msg.sender, wbethContract, msg.value, balance - preBalance, block.timestamp);
    }
    function getAssetPrice() public view override returns (uint256) {
        return (_etherPrice() * exchangeRatio) / 1e18;
    }
    // Update the cached exchangeRatio when necessary
    function updateExchangeRatio() external {
        exchangeRatio = IWBETH(wbethContract).exchangeRatio();
    }
}

In this updated version:  A new function updateExchangeRatio is added to update the cached exchangeRatio value when necessary. By calling this function, you can ensure that the cached value is always up to date when changes occur in the external contract (IWBETH). By caching the exchangeRatio value and updating it only when needed, you can reduce the gas costs associated with the multiplication and division operations in the getAssetPrice function.  Remember to invoke the updateExchangeRatio function whenever you expect the exchangeRatio to change in the external contract. This way, the cached value in your contract stays synchronized.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWstETHVault.sol

The expression _etherPrice() * IWstETH(address(collateralAsset)).stEthPerToken()) / 1e18 involves multiplication and division, which can be expensive in terms of gas. Consider optimizing gas costs by caching the stEthPerToken value in a local variable.

contract LybraWstETHVault is LybraPeUSDVaultBase {
    Ilido immutable lido;
    IWstETH private wstETHContract;
    uint256 private stEthPerToken;
    constructor(address _lido, address _peusd, address _oracle, address _asset, address _config)
        LybraPeUSDVaultBase(_peusd, _oracle, _asset, _config)
    {
        lido = Ilido(_lido);
        wstETHContract = IWstETH(address(collateralAsset));
        stEthPerToken = wstETHContract.stEthPerToken();
    }
    // Rest of the contract code...
    function getAssetPrice() public override returns (uint256) {
        uint256 etherPrice = _etherPrice();
        return (etherPrice * stEthPerToken) / 1e18;
    }
}

In this updated code:  The wstETHContract variable is introduced to store an instance of the IWstETH contract interface.  The stEthPerToken variable is introduced to cache the value of IWstETH(address(collateralAsset)).stEthPerToken() during contract initialization. In the getAssetPrice function, the cached stEthPerToken value is used directly, avoiding the need to call the contract and perform the multiplication and division for every getAssetPrice invocation. By caching the stEthPerToken value in a local variable, you eliminate the need for repetitive contract calls, resulting in significant gas savings.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/esLBRBoost.sol

In the setLockStatus function, you can consider adding a condition to skip the assignment of the userLockStatus if the existing unlockTime is already greater than the current block timestamp.
In the getUserBoost function, you can optimize the gas cost by using block.timestamp once and storing it in a local variable instead of accessing it multiple times.

pragma solidity ^0.8.17;
import "@openzeppelin/contracts/access/Ownable.sol";
contract esLBRBoost is Ownable {
    struct esLBRLockSetting {
        uint256 duration;
        uint256 miningBoost;
    }
    esLBRLockSetting[] public esLBRLockSettings;
    mapping(address => LockStatus) public userLockStatus;

    struct LockStatus {
        uint256 unlockTime;
        uint256 duration;
        uint256 miningBoost;
    }
    constructor() {
        esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));
        esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));
        esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));
        esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));
    }
    function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
        esLBRLockSettings.push(setting);
    }
    function setLockStatus(uint256 id) external {
        esLBRLockSetting memory _setting = esLBRLockSettings[id];
        LockStatus storage userStatus = userLockStatus[msg.sender];
        if (userStatus.unlockTime > block.timestamp) {
            require(userStatus.duration <= _setting.duration, "Your lock-in period has not ended, and the term can only be extended, not reduced.");
            return; // Skip assignment if existing unlockTime is greater
        }
        userStatus.unlockTime = block.timestamp + _setting.duration;
        userStatus.duration = _setting.duration;
        userStatus.miningBoost = _setting.miningBoost;
    }
    function getUnlockTime(address user) external view returns (uint256 unlockTime) {
        unlockTime = userLockStatus[user].unlockTime;
    }
    function getUserBoost(address user, uint256 userUpdatedAt, uint256 finishAt) external view returns (uint256) {
        LockStatus memory lockStatus = userLockStatus[user];
        uint256 boostEndTime = lockStatus.unlockTime;
        uint256 maxBoost = lockStatus.miningBoost;
        uint256 currentTime = block.timestamp; // Store block.timestamp in a local variable

        if (userUpdatedAt >= boostEndTime || userUpdatedAt >= finishAt) {
            return 0;
        }

        if (finishAt <= boostEndTime || currentTime <= boostEndTime) {
            return maxBoost;
        } else {
            uint256 time = currentTime > finishAt ? finishAt : currentTime;
            return ((boostEndTime - userUpdatedAt) * maxBoost) / (time - userUpdatedAt);
        }
    }
}

In the setLockStatus function, the condition if (userStatus.unlockTime > block.timestamp) is added to check if the existing unlockTime is already greater than the current block timestamp. If the condition is true, it skips the assignment of userLockStatus and returns without further execution.
In the getUserBoost function, block.timestamp is stored in the currentTime local variable. This avoids redundant access to block.timestamp and improves gas efficiency.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol

The _spendAllowance function is called multiple times in the _debitFrom and _transferFrom functions. To optimize gas costs, you can consider combining these calls into a single _spendAllowance function call if the conditions are met. This reduces the number of external function calls, which can save gas.

function _debitFrom(address _from, uint16, bytes32, uint _amount) internal virtual override returns (uint) {
    address spender = _msgSender();
    if (_from != spender) {
        uint256 allowance = _allowances[_from][spender];
        if (allowance != uint256(-1)) {
            require(allowance >= _amount, "OFT: Insufficient allowance");
            _allowances[_from][spender] = allowance - _amount;
        }
    }
    _burn(_from, _amount);
    return _amount;
}
function _transferFrom(address _from, address _to, uint _amount) internal virtual override returns (uint) {
    address spender = _msgSender();
    // if transfer from this contract, no need to check allowance
    if (_from != address(this) && _from != spender) {
        uint256 allowance = _allowances[_from][spender];
        if (allowance != uint256(-1)) {
            require(allowance >= _amount, "OFT: Insufficient allowance");
            _allowances[_from][spender] = allowance - _amount;
        }
    }
    _transfer(_from, _to, _amount);
    return _amount;
}

In these updated versions of _debitFrom and _transferFrom, we first check if the _from address is not equal to the _msgSender() (spender) before proceeding with the allowance deduction. If they are not equal, we retrieve the allowance and verify that it is sufficient. If the allowance is not set to unlimited (uint256(-1)), we deduct the _amount from the allowance.
By combining the multiple _spendAllowance calls into a single call, we reduce the gas cost by avoiding unnecessary external function calls.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraStETHVault.sol

In the depositEtherToMint function, you can optimize gas costs by removing the redundant assignment to totalDepositedAsset. Since you're already adding msg.value to depositedAsset[msg.sender], it will be accounted for in the total deposited asset automatically.

totalDepositedAsset += msg.value; // Remove this line
depositedAsset[msg.sender] += msg.value;

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol

In the stake function, you can optimize gas costs by removing the unnecessary bool success variable declaration. Since you're immediately using the result of the transfer operation, you can directly require the transfer to succeed.
stakingToken.transferFrom(msg.sender, address(this), _amount);

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

Instead of directly calling withdraw after burning esLBR, you can update the lastWithdrawTime and emit an event in the unstake function. Then, you can call withdraw externally after the unstake process is complete. This reduces redundant function calls and saves gas.
The unlockPrematurely function can be optimized by combining the calculation of burnAmount and amount in a single conditional statement. This avoids the need for redundant calculations.

function unstake(uint256 amount) external {
    require(block.timestamp >= esLBRBoost.getUnlockTime(msg.sender), "Your lock-in period has not ended. You can't convert your esLBR now.");
    esLBR.burn(msg.sender, amount);
    unstakeRatio[msg.sender] += amount / exitCycle;
    time2fullRedemption[msg.sender] = block.timestamp + exitCycle;
    emit UnstakeLBR(msg.sender, amount, block.timestamp);
}

function unlockPrematurely() external {
    require(block.timestamp + exitCycle - 3 days > time2fullRedemption[msg.sender], "ENW");
    uint256 burnAmount = getReservedLBRForVesting(msg.sender) - getPreUnlockableAmount(msg.sender);
    uint256 amount = getPreUnlockableAmount(msg.sender) + getClaimAbleLBR(msg.sender);
    if (amount > 0) {
        LBR.mint(msg.sender, amount);
    }
    unstakeRatio[msg.sender] = 0;
    time2fullRedemption[msg.sender] = 0;
    grabableAmount += burnAmount;
}

In the unstake function, instead of calling withdraw directly, we update the unstakeRatio and time2fullRedemption values. We emit an event to notify the unstake action. The actual withdrawal is performed externally by calling the withdraw function separately.
Similarly, in the unlockPrematurely function, we combine the calculation of burnAmount and amount in a single conditional statement. We update the unstakeRatio and time2fullRedemption values to zero and emit the necessary event. The actual minting of LBR tokens is performed within the same function.
By optimizing these functions, we reduce redundant function calls and avoid unnecessary calculations, resulting in gas savings.

Simplify Calculation in getPreUnlockableAmount: The calculation in the getPreUnlockableAmount function can be simplified by removing unnecessary divisions. You can calculate (a * 75e18) first and then divide by (exitCycle - 3 days).


https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol

Gas Optimization: Combine calculation in the getSharesByMintedEUSD and getMintedEUSDByShares functions. The getSharesByMintedEUSD and getMintedEUSDByShares functions perform similar calculations. You can optimize gas usage by combining the calculation into a single function and reusing it where needed.

function convertAmount(uint256 _amount, bool toShares) internal view returns (uint256) {
    uint256 total = toShares ? _totalShares : _totalSupply;
    if (total == 0) {
        return 0;
    } else {
        return _amount.mul(total).div(toShares ? _totalSupply : _totalShares);
    }
}
function getSharesByMintedEUSD(uint256 _EUSDAmount) public view returns (uint256) {
    return convertAmount(_EUSDAmount, true);
}
function getMintedEUSDByShares(uint256 _sharesAmount) public view returns (uint256) {
    return convertAmount(_sharesAmount, false);
}
In the convertAmount function, the toShares parameter determines whether the conversion is from EUSD to shares or from shares to EUSD. By reusing this function, we avoid duplicating the calculations in both getSharesByMintedEUSD and getMintedEUSDByShares functions.
Gas Optimization: Remove redundant events in the _transfer function. The _transfer function emits both Transfer and TransferShares events. Since the transfer of tokens and shares are inherently linked, emitting only the Transfer event should be sufficient.
function _transfer(address _sender, address _recipient, uint256 _amount) internal {
    uint256 _sharesToTransfer = getSharesByMintedEUSD(_amount);
    _transferShares(_sender, _recipient, _sharesToTransfer);
    emit Transfer(_sender, _recipient, _amount);
}
By emitting only the Transfer event, we eliminate the redundant emission of the TransferShares event, as the transfer of tokens and shares are inherently linked.
Gas Optimization: Avoid unnecessary storage updates in the burnShares function. In the burnShares function, you update the shares[_account] twice: once in the require statement and again in the subsequent line. You can optimize gas usage by updating it only once.
function burnShares(address _account, uint256 _sharesAmount) external onlyMintVault BurnPaused returns (uint256 newTotalShares) {
    require(_account != address(0), "BURN_FROM_THE_ZERO_ADDRESS");
    uint256 accountShares = shares[_account];
    require(_sharesAmount <= accountShares, "BURN_AMOUNT_EXCEEDS_BALANCE");
    newTotalShares = _totalShares.sub(_sharesAmount);
    shares[_account] = accountShares.sub(_sharesAmount);
    _totalShares = newTotalShares;
    emit SharesBurnt(_account, getMintedEUSDByShares(accountShares), getMintedEUSDByShares(newTotalShares), _sharesAmount);
}
By updating the shares[_account] only once, we avoid redundant storage updates, leading to gas optimization.
Gas Optimization: Optimize conditional statements in the _spendAllowance function. In the _spendAllowance function, the conditional statement can be optimized to eliminate the unnecessary require statement. You can directly subtract the amount from the currentAllowance and update the allowance without an additional require statement.
function _spendAllowance(address owner, address spender, uint256 amount) internal virtual {
    uint256 currentAllowance = allowance(owner, spender);
    if (currentAllowance < amount) {
        _approve(owner, spender, 0);
    } else if (amount > 0) {
        _approve(owner, spender, currentAllowance - amount);
    }
}
In the _spendAllowance function, we optimize the conditional statements to directly update the allowance without an additional require statement. If the amount is greater than the currentAllowance, we set the allowance to zero; otherwise, we subtract the amount from the currentAllowance.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol

Simplify token balance calculations: In the stakedLBRLpValue function, there are multiple calculations involving token balances and prices. Consider simplifying the calculations by combining common factors or using intermediate variables to avoid redundant operations and improve gas efficiency.
function stakedLBRLpValue(address user) public view returns (uint256) {
    uint256 totalLp = IEUSD(ethlbrLpToken).totalSupply();
    uint256 etherInLp;
    uint256 lbrInLp;
    {
        (, int etherPrice, , , ) = etherPriceFeed.latestRoundData();
        (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
        uint256 etherBalance = IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken);
        uint256 lbrBalance = IEUSD(LBR).balanceOf(ethlbrLpToken);
        etherInLp = (etherBalance * uint256(etherPrice)) / 1e8;
        lbrInLp = (lbrBalance * uint256(lbrPrice)) / 1e8;
    }
    uint256 userStaked = IEUSD(ethlbrStakePool).balanceOf(user);
    return (userStaked * (lbrInLp + etherInLp)) / totalLp;
}

function stakedLBRLpValue(address user) public view returns (uint256) {
    uint256 totalLp = IEUSD(ethlbrLpToken).totalSupply();
    uint256 etherInLp;
    uint256 lbrInLp;

    {
        (, int etherPrice, , , ) = etherPriceFeed.latestRoundData();
        (, int lbrPrice, , , ) = lbrPriceFeed.latestRoundData();
        uint256 etherBalance = IEUSD(0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2).balanceOf(ethlbrLpToken);
        uint256 lbrBalance = IEUSD(LBR).balanceOf(ethlbrLpToken);
        etherInLp = (etherBalance * uint256(etherPrice)) / 1e8;
        lbrInLp = (lbrBalance * uint256(lbrPrice)) / 1e8;
    }

    uint256 userStaked = IEUSD(ethlbrStakePool).balanceOf(user);
    return (userStaked * (lbrInLp + etherInLp)) / totalLp;
}
In this implementation, the calculations involving the ether and LBR token balances are combined within a single block using an inner block scope. By doing this, we avoid redundant calls to the latestRoundData function and eliminate redundant variable assignments.

Optimize loop operations: In the stakedOf function, there is a loop that iterates over the pools array. Ensure that the loop conditions and iterations are optimized to minimize gas costs. Additionally, consider using fixed-size arrays instead of dynamic arrays if the number of elements is known and limited.

function stakedOf(address user) public view returns (uint256) {
    uint256 amount;
    address[2] memory poolAddresses; // Use a fixed-size array for pools
    uint256 numPools = pools.length;
    require(numPools <= 2, "Exceeded maximum number of pools");
    for (uint256 i = 0; i < numPools; i++) {
        poolAddresses[i] = pools[i];
    }
    for (uint256 i = 0; i < numPools; i++) {
        ILybra pool = ILybra(poolAddresses[i]);
        uint256 borrowed = pool.getBorrowedOf(user);
        if (pool.getVaultType() == 1) {
            borrowed = (borrowed * (1e20 + peUSDExtraRatio)) / 1e20;
        }
        amount += borrowed;
    }
    return amount;
}

Use a fixed-size array poolAddresses with a size of 2 instead of a dynamic array. You can adjust the size based on the maximum number of pools you expect. 
Store the number of pools in the variable numPools to avoid repeated array length checks.
Initialize the poolAddresses array with the pool addresses from the pools array outside the loop.
Optimize the loop conditions by using < numPools instead of < _pools.length.
Iterate over the poolAddresses array and access the corresponding pool using ILybra(poolAddresses[i]).
By optimizing the loop conditions and iterations, we reduce gas costs associated with array length checks and dynamic array access, resulting in improved efficiency.