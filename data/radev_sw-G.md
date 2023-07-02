### [G‑01] Use calldata instead of memory for arrays

**Summary**
You can indeed use `calldata` instead of `memory` for the `_pools` array argument, because the function is marked as `external`.
Using `calldata` instead of `memory` can reduce the gas cost of this function, particularly for larger arrays.

**There is 1 instance of this issue:**
```solidity
File: contracts/lybra/miner/EUSDMiningIncentives.sol

    function setPools(address[] memory _pools) external onlyOwner {
        for (uint i = 0; i < _pools.length; i++) {
            require(configurator.mintVault(_pools[i]), "NOT_VAULT");
        }
        pools = _pools;
    }

```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/EUSDMiningIncentives.sol#L93-L98

**Potential Gas Savings**
When array is passed as an argument of a function, the EVM charges gas for each element of the array.
Here's the approximate cost:
- 200 gas per word for memory
- 3 gas per word for calldata


### [G‑02] Use internal function inside in the functions instead of modifiers to minimize the deployment cost.

**Summary**
When you use the modifier the whole code of the modifier is repeated every time. To minimize the deployment cost of your contract instead you can create an internal function with the code of the modifier and call it from the other functions who need it.

**There are 8 instances of this issue:**
```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

   modifier onlyRole(bytes32 role) {
        GovernanceTimelock.checkOnlyRole(role, msg.sender);
        _;
    }
   // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L85-L88
```

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

    modifier checkRole(bytes32 role) {
        GovernanceTimelock.checkRole(role, msg.sender);
        _;
    }
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L90-L93
```

```solidity
File: contracts/lybra/token/EUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    modifier onlyMintVault() {
        require(configurator.mintVault(msg.sender), "RCP");
        _;
    }
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L79-L82
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L42-L44 
```

```solidity
File: contracts/lybra/token/EUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    modifier MintPaused() {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
        _;
    }
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L83-L86
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L46-L49
```

```solidity
File: contracts/lybra/token/EUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    modifier BurnPaused() {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
        _;
    }
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L87-L90
    // https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L50-L53
```


**Optimized version of modifiers**

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

   function _onlyRole(bytes32 role) internal {
        GovernanceTimelock.checkOnlyRole(role, msg.sender);
    }
```

```solidity
File: contracts/lybra/configuration/LybraConfigurator.sol

    function _checkRole(bytes32 role) internal {
        GovernanceTimelock.checkRole(role, msg.sender);
    }
```

```solidity
File: contracts/lybra/token/EUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    function _onlyMintVault() internal {
        require(configurator.mintVault(msg.sender), "RCP");
    }
```

```solidity
File: contracts/lybra/token/EUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    function _mintPaused() internal {
        require(!configurator.vaultMintPaused(msg.sender), "MPP");
    }
```

```solidity
File: contracts/lybra/token/EUSD.sol
File: contracts/lybra/token/PeUSDMainnetStableVision.sol

    function _burnPaused() internal {
        require(!configurator.vaultBurnPaused(msg.sender), "BPP");
    }
```