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


### [G‑03] Optimizations with assembly
**Summary**
1. Use assembly to check for address(0)
2. Use assembly to write storage values
3. Use assembly for math (add, sub, mul, div)

**Instances of issue 1 (zero address checking):**
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L367-L368
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L392-L393
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L412
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L441
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L460

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/configuration/LybraConfigurator.sol#L98-L101

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L99
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L127
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L141

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L83
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L97
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L111`


Example of using assembly for zero address check
```solidity
    function assemblyOwnerNotZero(address _addr) public pure {
        assembly {
            if iszero(_addr) {
                mstore(0x00, "zero address")
                revert(0x00, 0x20)
            }
        }
    }
```

**Instances of issue 2 (write storage values):**
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L154
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L201
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L249
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L269
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/EUSD.sol#L335

Example of using assembly to write storage values
```solidity
contract Contract1 {
    address owner = 0xb4c79daB8f259C7Aee6E5b2Aa729821864227e84;

    function assemblyUpdateOwner() public {
        assembly {
            sstore(owner.slot, _msgSender)
        }
    }
}
```


### [G‑04] Increments/decrements can be unchecked in for-loops
**Summary**
```solidity
- for (uint256 i; i < numIterations; i++) {
+ for (uint256 i; i < numIterations;) {
 // ...
+   unchecked { ++i; }
}
```


### [G-05] Upgrade pragma
**Summary**
Currently the best version you can use is stable 0.8.19

Why to update pragma
- Follow best practice.
- Using newer compiler versions and the optimizer give gas optimizations.
- Also, additional safety checks are available for free.
- https://blog.soliditylang.org/2023/02/22/solidity-0.8.19-release-announcement/


### [G-06] Change function visability from public to external
**Summary**
Functions withdraw(), convertToPeUSD() are never called from within contracts but yet declared public. Their visibility can be made external to save gas and improve code quality.

*There are 2 instances of this issue:*
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol#L100-L107
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L79-L88


### [G-07] Multiplication by two should use bit shifting
**Summary**
<x> * 2 is the same as <x> << 1. While the compiler uses the SHR opcode to accomplish both, the version that uses division incurs an overhead of 20 gas due to JUMPs to and from a compiler utility function that introduces checks which can be avoided by using unchecked {} around the division

*There are 2 instances of this issue:*
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/base/LybraPeUSDVaultBase.sol#L130


### [G-08] Storage pointer for structs
**Summary**
Using a storage pointer is cheaper than reading a struct field several times.

***Instances of this issue:***
```solidity
File: contracts/lybra/governance/LybraGovernance.sol

        forVotes =  proposalData[proposalId].supportVotes[0];
        againstVotes =  proposalData[proposalId].supportVotes[1];
        abstainVotes =  proposalData[proposalId].supportVotes[2];
```

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L120-L122
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L129-L133
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L60
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/governance/LybraGovernance.sol#L67

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L114-L121
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol#L168

**Recommended Mitigation Steps**
Use a storage pointer