### 1. Use Uint256 to store Constants instead of Bytes32
Instead of using a bytes32 to store keccack256 hash as state variable, we can instead use a uint256 to store the state variables examples can be found in solady contract https://github.com/Vectorized/solady/blob/8c751db06e614e26e6acb9b385b463feef5e3a9f/src/tokens/ERC20.sol#L51 
```
contract test {//deployment cost 113364 gas, 
    uint256 public constant _TRANSFER_EVENT_SIGNATURE =
        0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef; //
}

contract test {//deployment cost 121071 gas, 
    bytes32 public constant _TRANSFER_EVENT_SIGNATURE =
        keccak256(bytes("Transfer(address,address,uint256)"));
}
```
one reason this might be is because i think solidity use extra operations to check whether the bytes are valid bytes but does not do this for uint256 
NOTE: uint256 is equal to bytes32 they both contain 32 bytes and can be typecasted and vice versa
an example of this problem can be found in the `LybraConfigurator` contract

Instances in the code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L76
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L77
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L78
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/GovernanceTimelock.sol#L10
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/GovernanceTimelock.sol#L11


### 2. Change The Visibility of the Bytes32 constant to private from public
There's no reason to use public on bytes32 constant variable, when you use public for a function solidity creates a getter for that variable similar to a function and this leads to more gas usage
```
contract test { //Deployment cost: 77126 gas
    bytes32 private constant _TRANSFER_EVENT_SIGNATURE =
        keccak256(bytes("Transfer(address,address,uint256)"));
}

contract test { //Deployment cost: 121071 gas
    bytes32 public constant _TRANSFER_EVENT_SIGNATURE =
        keccak256(bytes("Transfer(address,address,uint256)"));
}
```
this saves us 5000+ in deployment cost this same problem can be found in `LybraConfigurator` contract

Instances in the code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L76
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L77
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L78
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/GovernanceTimelock.sol#L13
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/GovernanceTimelock.sol#L12

### 3.Use Custom Errors to instead of require 
This chat from overflow explains why custom errors are more efficient than require statement https://ethereum.stackexchange.com/questions/123381/when-should-i-use-require-vs-custom-revert-errors
This example can be in the `LybraConfigurator` and all other part of the code

Instances in Code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L200
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L214
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L225
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L78
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L79
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L82

### 4.Do not Initialize variables in loops to zero
by default variables in solidity are zero by default, there is no reason to initialize them to zero again especially in a loop
```
contract test {//Deployment cost : 180927 gas 
    function tests(uint[] calldata arr) public view {// runtime gas: 1220 gas
       for(uint i; i < arr.length; ++i){
           
       }
    }
}

contract test {//Deployment cost : 180927 gas 
    function tests(uint[] calldata arr) public view {// runtime gas: 1230 gas
       uint t;
       for(uint i = 0; i < arr.length; ++i){
           t = i;
       }
    }
}
```
this saves us 10+ in gas fees this same problem can be found in `LybraConfigurator` contract
Instances in code
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L236
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L94
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L138


### 5. Consider using named return Variables 
Named return variables is the cheapest method form of return you can do(apart from the returning using assembly - not advisible)
```
contract test {//Deployment cost : 178449 gas
    function tests(uint[] calldata arr) public pure returns(uint r){// runtime gas : 696 gas
      r = arr[0];
    }
}

contract test {//Deployment cost : 178449 gas
    function tests(uint[] calldata arr) public pure returns(uint){// call gas : 696 gas
      return arr[0];
    }
}
```

Instances in code
Note: all functions in the codebase have this issue

### 6. Replace return type bool with uint256
Bools in solidity are 1s and 0s by default, 1 for true and 0 for false, the thing is bools can also be called uint8, yes every bool type is uint8 meaning they are one byte, so everytime you return a type as uint8 or bool, solidity will have to perform extra operations to shift the bits and make sure they fit into a uint8 type
```
contract test {//Deploymnet cost : 107133 gas
    function tests() public view returns(bool r){//runtime cost : 321 gas
       assembly{
           r := 1
       }
     }
}

contract test {//Deployment cost: 106637
    function tests() public view returns(uint256 r){//runtime cost : 315 gas
       assembly{
           r := 1
       }
     }
}
```
turns out replacing bool with bool with a uint256 saves you 900+ in deployment cost and 6+ in runtime cost
this can be found in the `Governance Timelock` contract

Instances in code
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L360
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L348
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L268

### 7.Make Mappings Private
A lot of states variables could be made private for example in the `Governance Timelock` contract a lot of mappings should have been made private instead of public, when you make a state variable 

```
contract test {//deployment cost: 126699 gas
    mapping(uint => bool) private num;
    function tests(uint r) public  {//write cost for initialization: 50408 gas
        num[r] = true;              //write cost after initialization: 27523 gas
     }
}

contract test {//deployment cost: 166630 gas
    mapping(uint => bool) public num;
    function tests(uint r) public  {//write cost for initialization: 50434 gas
        num[r] = true;              //write cost after initialization: 27549 gas
     }
}
```
from the above we save over 4000+ in gas during deployment and 34+ in gas during runtime

Instances in code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L33
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/stakerewardV2pool.sol#L33
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/stakerewardV2pool.sol#L35
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/stakerewardV2pool.sol#L36
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/ProtocolRewardsPool.sol#L33
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/ProtocolRewardsPool.sol#L35
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/ProtocolRewardsPool.sol#L36


### 8. Offchain computation
A lot of computation could have been done offchain, instead they were done onchain and this led to the increase of gas, for example in the keccak256 computaion and a lot of constants multiplication computation, with this we could have saved 100+ in gas if they were done off chain by the dev and simply brought on chain, one way we should actually do this is use uint256 instead of keccak256, but that would mean rewriting the contract anyway

```
contract test {//Deployment in gas cost: 164429 gas
    function tests() public  {//call gas cost: 24428 gas
        check(keccak256("timelock"), address(this));
     }

     function check(bytes32, address _sender) public view {

     }
}

contract test {//Deployment in gas cost: 165167 gas
    function tests() public {
        check(
            0x3b101b2aa2577f8e2ab6b4a2521a67e48b95c9db8515c5daa4e493b8e4be8a8e,
            address(this)
        );
    }

    function check(bytes32, address _sender) public view {}
}

```

Instances in code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/LybraRETHVault.sol#L42
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L26
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L27
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L28
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/esLBRBoost.sol#L29
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L181


### 9. Use byte32 as return type where string length is more than 32 
when you use strings as return types, then you definitely have to write to memory since it's a dynamic type, however if we were bytes32 type it goes directly to the stack and saves us 40+ in gas fees

Instances in code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L151
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L156

### 10. Use Calldata as Storage instead of memory for function param
storing array in calldata is way cheaper than using memory,

```
contract test {//deployment cost: 217662 gas
   function reddish(uint[] memory ) public pure {//call cost: 1818 gas

   }
}

contract test {//deployment cost: 117785 gas
   function reddish(uint[] memory ) public pure {//call cost: 452 gas

   }
}


```
This was conducted using the same param

Instances in code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L106C5-L106C5

### 11. Some state varaiables could be stored as bytecode using immutables
to read from storage is 5100 in gas but when declared as immutables it only cost 3 gas, with this you get to save over 4997 in gas fees


### 12. Use Shift left to perform multiplication if one of the number to be multiplied is a power of 2
In one part of the code mul opcode was used to perform multiplication this cost 5 gas, we could have done it using SHL(shift right which would only cost 3 gas)

Instances in code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/pools/base/LybraEUSDVaultBase.sol#L159

### 13. Remove SafeMath
Starting from solidity version 0.8.0, there's no need for safemath any longer, safemath was introduced to curb overflow and underflow, however from 0.8.0 the compiler can already do this whuch just makes safemath irrelevant and will cause increase in deployment cost

Instances in the code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/EUSD.sol#L5
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/token/EUSD.sol#L38

### 14. Less than or Equal to against constant constant
whenever you are performing the operation <= on a constant, e.g ``` i <= 2 ``` this cost 6+ in gas fees since you would have to use two opcodes that costs 3 each, instead you can simply do ``` i < 3 ```, this two perform the same function but the second part is cheaper since it only uses 3 opcode

Instances in the code:
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L127
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L187
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L214
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L225
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/configuration/LybraConfigurator.sol#L247
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/governance/LybraGovernance.sol#L79
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L111
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L106
https://github.com/code-423n4/2023-06-lybra/blob/5d70170f2c68dbd3f7b8c0c8fd6b0b2218784ea6/contracts/lybra/miner/EUSDMiningIncentives.sol#L101

