### Avoid logging constant or immutable storage values to save gas

Constant or immutable storage values do not change. They can be stored locally or retrieved at any time from the contract. They will maintain the same values throughout the life of the contract. Logging them costs additional unnecessary gas usage. Check the [Solidity docs](https://docs.soliditylang.org/en/v0.8.20/contracts.html#constant-and-immutable-state-variables) for more info.

Found these instances:
`collateralAsset` is immutable but emitted as `asset` parameter in all places the following events are emitted
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L34

https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L38

https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L36


### Avoid logging `block.timestamp` in events
`block.timestamp` and similar event details like block number, contract address and transaction hash are already accessible from the Ethereum RPC specification. They can be accessed anytime the event is fetched. Logging them adds an unnecessary gas overhead. Check the [eth_getTransactionReceipt](https://ethereum.github.io/execution-apis/api-documentation/) section of the Ethereum RPC specification for more details.

Instances found:
`block.timestamp` is logged in these events.
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L44
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L43
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L42
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L41
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L40
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L39
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L38
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L36
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L34



### Store external information reused multiple times in memory 
External data retrieved from other contracts and reused in the same function should be saved locally. Storing the data locally reduces the gas cost of using the `CALL` opcode and execution of the called function to retrieve the data. 

Instances Found:
The call `configurator.vaultKeeperRatio(address(this))` is used in the instances below twice when it can be saved to memory and reused.
https://github.com/code-423n4/2023-06-lybra/blob/d7fb18cc47ca93ecdfd7f1668614c7fb0b9179aa/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L204
https://github.com/code-423n4/2023-06-lybra/blob/d7fb18cc47ca93ecdfd7f1668614c7fb0b9179aa/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L205

### `unchecked` blocks can be used to reduce gas usage.
`unchecked` block can be used for arithmetic operations which are guaranteed not to overflow or underflow. This reduces gas usage. An example of a guarantee is a comparison preceding that operation in the same function that checks if one operand is less or greater than the other.

Instance found:
The lines below can be put in an unchecked block. Since there is already a check in line 101. Line 102 can be added since `totalDepositedAsset` will certainly be greater than `depositedAsset[msg.sender]`.

https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L102
https://github.com/code-423n4/2023-06-lybra/blob/dc901a3560b71ed2376feb6418b3d81e3d067fb9/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L103



