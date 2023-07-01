1. setPools will reset the pools array if it's called with an empty array.
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93
```
 function setPools(address[] memory _pools) external onlyOwner {
        for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
        //@audit-qa if the function called with 0 array length. it will resetr the pools. Must check it's > 0
        pools = _pools;
    }
```

2. User can front-run the TX which invoke `notifyRewardAmount` and stake large amount to profit from the reward.
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L227
```
function notifyRewardAmount(uint amount, uint tokenType) external { // @audit-qa bot can fornt-run and stake large amount to before ditrbuiteRewards
        require(msg.sender == address(configurator));
        if (totalStaked() == 0) return;
        require(amount > 0, "amount = 0");
        if(tokenType == 0) {
            uint256 share = IEUSD(configurator.getEUSDAddress()).getSharesByMintedEUSD(amount);
            rewardPerTokenStored = rewardPerTokenStored + (share * 1e18) / totalStaked();
        } else if(tokenType == 1) {
            ERC20 token = ERC20(configurator.stableToken());
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e36 / token.decimals()) / totalStaked();
        } else {
            rewardPerTokenStored = rewardPerTokenStored + (amount * 1e18) / totalStaked();
        }
    }
```

3. `executeFlashloan` shouldn't be marked as payable, any ETH sent by mistake could be lost.
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L129

4. Due to rounding error on `grabEsLBR` function when calculating the amount to be burned, an attacker can send 3 WEI as amount which will result attacker mint 3 WEI without burning any tokens due to the calculation round down to Zero
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L131

```
function grabEsLBR(uint256 amount) external {
        require(amount > 0, "QMG");
        grabableAmount -= amount;
        LBR.burn(msg.sender, (amount * grabFeeRatio) / 10000);
        // 3 * 3000 / 10000 = 9000 / 10000 = 0
        esLBR.mint(msg.sender, amount);
    }
```

5. Wrong address for WBETH on LybraWBETHVault
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraWbETHVault.sol#L16

The commented mentioned that the address of WBETH is `0xae78736Cd615f374D3085123A210448E74Fc6393` but this is the address for the `rETH` contract used in `LybraRETHVault`. The correct address is `0xa2E3356610840701BDf5611a53974510Ae27E2e1`. While this is not directly making an issue since it's just a comment, this can influence the deployer to use wrong address.