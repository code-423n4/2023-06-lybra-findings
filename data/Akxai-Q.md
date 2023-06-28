// SPDX-License-Identifier: BUSL-1.1

pragma solidity ^0.8.17;

import "../interfaces/IEUSD.sol";
import "./base/LybraPeUSDVaultBase.sol";
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

interface IRETH {
    function getExchangeRatio() external view returns (uint256);
}

interface IRkPool {
    function deposit() external payable;
}

contract LybraRETHVault is LybraPeUSDVaultBase {
    IRkPool public rkPool;

    constructor(
        address _peusd,
        address _config,
        address _rETH,
        address _oracle,
        address _rkPool
    ) LybraPeUSDVaultBase(_peusd, _oracle, _rETH, _config) {
        rkPool = IRkPool(_rkPool);
    }

    function depositEtherToMint(uint256 mintAmount) external payable override {
        require(msg.value >= 1 ether, "DNL");
        uint256 preBalance = collateralAsset.balanceOf(address(this));
        rkPool.deposit{value: msg.value}();
        uint256 balance = collateralAsset.balanceOf(address(this));
        depositedAsset[msg.sender] += balance - preBalance;

        if (mintAmount > 0) {
            _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
        }

        emit DepositEther(msg.sender, address(collateralAsset), msg.value, balance - preBalance, block.timestamp);
    }

    function setRkPool(address addr) external {
        require(configurator.hasRole(keccak256("TIMELOCK"), msg.sender), "Not authorized");
        rkPool = IRkPool(addr);
    }

    function getAssetPrice() public override returns (uint256) {
        return (_etherPrice() * IRETH(address(collateralAsset)).getExchangeRatio()) / 1e18;
    }
}
