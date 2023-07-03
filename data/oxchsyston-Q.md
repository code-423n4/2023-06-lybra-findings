                  QA Report (lows / no-critical)
          #1  Use ownable2steps instead of ownable for transfering ownership.
There is risk of losing ownership of protocol due to transfer to mistyped or non-existent address. the ownable2steps is an upgraded version of the openzeppelin ownable contract, which is safer than the ownable contract itself because the ownable, can allow the transfer of ownership to mistyped or non-existent address. Whereas, in the ownable2steps, the transfer comes in two steps where the current owner transfers ownership to a new owner who has to accept ownership for the transfer to be considered successful. 

         #2   Use the more flexible <= sign in code instead of <
In the function `setrewardduration()` in miningincentive.sol, it checks if the previous duration has elapsed before a new duration is set with a require statement
```solidity
function setRewardsDuration(uint256 _duration) external onlyOwner {
        require(finishAt < block.timestamp, "reward duration not finished");
        duration = _duration;
```
the `less than or equal to` sign should be used instead as `block.timestamp` is used to calculate the elapse of time in the contract. This means that the finishAt must be less than the current block.timestamp which is not flexible as the finishAt is considered finished if the finish bloch has been reached. so using < unnecessarily extends the time to make changes to the protocol.

         #3 No event emitted after reward duration was set
The `setrewardduration()` is a function to set the reward duration to a new one and it is callable by only the owner of the protocol. This function should emit an event after a new duration has been as it concerns core functionality of the protocol, so that the users are not caught unaware on the change of the reward duration. This will increase credibility and user trust in the protocol.

          #4 use fixed pragma instead of floating pragma
The use of fixed pragma in favor of floating pragma is highly advised. as using an unlocked pragma leads to deploying of contracts in compiler versions inwhich they have not been properly tested. This also leads to the use of multiple pragma versions across different contracts which causes inconsistency in code resulting in unknown security issues.