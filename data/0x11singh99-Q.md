# Non Critical 
1. Function params name in all over the protocol doesn't follow one convention, sometimes it is prepended with _ sometimes not. Which is confusing.
2. named return is confusing
3. In many places natspec comments not written, which is really confusing to understand the intention of function or contract.
4. Prepend internal/private functions and variables with _ to make clear. Sometimes it is done sometimes not.
5. Use constant variables instead of direct numeric value it not good practice. It is used all over the protocol.
## 6. Use proper solidity style guide layout it is not followed all over the protocol
### Order of Solidity Layout :
### Layout contract elements in the following order:
Import statements
Interfaces
Libraries
Contracts

## Inside each contract, library or interface, use the following order:

Type declarations
State variables
Events
Errors
Modifiers
Functions

### Functions should be grouped according to their visibility and ordered:

constructor
receive function (if exists)
fallback function (if exists)
external
public
internal
private

#### Within a grouping, place the view and pure functions last.

# Low level
## L-01  Function input not verified, which can be vulnerable and gas consuming verify to fail early
###  user can withdraw more than he staked . or unexpected underflow will occur which is not desired response
To get it check the balance before tranfser

```solidity
File : contracts/lybra/miner/stakerewardV2pool.sol
    //@audit low check balance before transfer to prevent unexpected under flow
        balanceOf[msg.sender] -= _amount;
```
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol#L95
### it is on many places in protocol check it and improve it.
## L-02 Transfer return value not checked, if transfer fails it will not report which is a issue
### transfer retrun not checked
```solidity
```

