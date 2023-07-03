## [G-01] Function can be made external instead of public
There are 2 instances of this:

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/GovernanceTimelock.sol#L25

```solidity
File: contracts/lybra/governance/GovernanceTimelock.sol
25: function checkRole(bytes32 role, address _sender) public view  returns(bool){
26:         return hasRole(role, _sender) || hasRole(DAO, _sender);
27:     }
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L257

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
257: function getPoolTotalPeUSDCirculation() public view returns (uint256) {
```

## [G-02] Struct member can be modified to save slot
There are 3 instances of this:

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/governance/LybraGovernance.sol#L23

uint256 votes can be made uint128 votes. This is significantly larger than the proposal threshold (1e23) as well.
```solidity
File: contracts/lybra/governance/LybraGovernance.sol
15: struct Receipt {
16:        /// @notice Whether or not a vote has been cast
17:        bool hasVoted;
18:
19:        /// @notice Whether or not the voter supports the proposal or abstains
20:        uint8 support;
21:
22:        /// @notice The number of votes the voter had, which were cast
23:        uint256 votes; //@audit can be made uint128 to fit in one slot instead of two
24:    }
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L12

uint256 duration can be made uint16, which can be good enough for 180 years and miningBoost could be made uint128. This could fit in one slot instead of two.
```solidity
File: contracts/lybra/miner/esLBRBoost.sol
12: struct esLBRLockSetting {
13:        uint256 duration; //@audit use uint16
14:        uint256 miningBoost; //@audit use uint128
15:    }
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L18C6-L18C6


uint256 unlockTime and uint256 duration can be made uint16 which is good for 180 years and mining boost can be uint128. This could fit in one slot instead of three.
```solidity
File: contracts/lybra/miner/esLBRBoost.sol
18: struct LockStatus {
19:        uint256 unlockTime;//@audit use uint16
20:        uint256 duration;//@audit use uint16
21:        uint256 miningBoost;//@audit use uint128
22:    }
```

## [G-03] Assign each pool in the for loop itself to save gas

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

Instead of this:
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
93: function setPools(address[] memory _pools) external onlyOwner {
94:        for (uint i = 0; i < _pools.length; i++) {
95:            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
96:        }
97:        pools = _pools;
98:    }
```

We can use this:
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
93: function setPools(address[] memory _pools) external onlyOwner {
94:        for (uint i = 0; i < _pools.length; i++) {
95:            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
96:             pools[i] = _pools[i];
97:        }
98:    }
```

## [G-04] Function parameter can be made calldata to save gas
There are 2 instances of this:

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/EUSDMiningIncentives.sol#L93

Parameter is only read and not modified, thus can be made calldata.
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol
93: function setPools(address[] memory _pools) external onlyOwner {
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/esLBRBoost.sol#L33

Parameter is only read and not modified, thus can be made calldata.
```solidity
File: contracts/lybra/miner/esLBRBoost.sol
33: function addLockSetting(esLBRLockSetting memory setting) external onlyOwner {
```

## [G-05] Use constant to save gas
Variables that do not change can be made constant to save gas.
There are 6 instances of this:

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/miner/ProtocolRewardsPool.sol#L39

```solidity
File: contracts/lybra/miner/ProtocolRewardsPool.sol
39: uint256 immutable exitCycle = 90 days;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L21

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
21: uint256 public immutable badCollateralRatio = 150 * 1e18;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
30: uint8 immutable vaultType = 0;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18C27-L18C27

```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
18: uint8 immutable vaultType = 1;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/LBR.sol#L15

```solidity
File: contracts/lybra/token/LBR.sol
15: uint256 maxSupply = 100_000_000 * 1e18;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/token/esLBR.sol#L20C13-L20C22

```solidity
File: contracts/lybra/token/esLBR.sol
20: uint256 maxSupply = 100_000_000 * 1e18;
```

## [G-06] Extra memory variable and require check can be removed to save gas

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/LybraWstETHVault.sol#L33

Check on Line 33 not required.
```solidity
File: contracts/lybra/pools/LybraWstETHVault.sol
32: uint256 sharesAmount = lido.submit{value: msg.value}(address(configurator));
33: require(sharesAmount > 0, "ZERO_DEPOSIT");
```

The [Lido.sol](https://vscode.blockscan.com/ethereum/0x17144556fd3424edc8fc8a4c940b2d04936d17eb) contract already does the required check in the _submit() function, which is called by the submit() function.
Here is the submit() function:
```solidity
465: function submit(address _referral) external payable returns (uint256) {
466:        return _submit(_referral);
467:    }
```
Here is the _submit() function:
```solidity
922: function _submit(address _referral) internal returns (uint256) {
923:        require(msg.value != 0, "ZERO_DEPOSIT"); //check is made here
```

Additionally, since uint256 sharesAmount is only used in the require statement, the returned value from the submit function of the Lido contract need not be stored in it and thus the variable can be removed. This can save us additional deployment and function execution gas.

## [G-07] Initialising uint variable to 0 can be removed

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

Default value of uint8 is 0. Assigning it 0 has extra deployment costs.
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
30: uint8 immutable vaultType = 0;
```

## [G-08] Use uint256 instead of uint8 to save gas

There are 2 instances of this: 

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L30

Solidity compiler works 32 bytes at a time. Using uint256 instead of uint8 can help avoid overhead.
```solidity
File: contracts/lybra/pools/base/LybraEUSDVaultBase.sol
30: uint8 immutable vaultType = 0;
```

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L18

Solidity compiler works 32 bytes at a time. Using uint256 instead of uint8 can help avoid overhead.
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
18: uint8 immutable vaultType = 1;
```

## [G-08] Method can be made inline instead of storing in memory variable

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L66

Instead of this:
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
66: uint256 assetPrice = getAssetPrice();
67: _mintPeUSD(msg.sender, msg.sender, mintAmount, assetPrice);
```

We can use this:
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
66: _mintPeUSD(msg.sender, msg.sender, mintAmount, getAssetPrice());
```
This is the only function that creates an extra memory variable to store getAssetPrice(), all other contracts using _mintPeUSD() implement it in the same way above.

## [G-09]  <x> += <y> costs more gas than <x> = <x> + <y> for state variables
Using the addition operator instead of plus-equals saves 113 gas

https://github.com/code-423n4/2023-06-lybra/blob/7b73ef2fbb542b569e182d9abf79be643ca883ee/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L232

Instead of this: 
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
232: feeStored[user] += _newFee(user);
```

We can use this:
```solidity
File: contracts/lybra/pools/base/LybraPeUSDVaultBase.sol
232: feeStored[user] = feeStored[user] + _newFee(user);
```