# [G-01] Using a SafeMath library, where substraction can be unchecked.

```
File: contracts/lybra/token/EUSD.sol

396:     require(_sharesAmount <= currentSenderShares, "TRANSFER_AMOUNT_EXCEEDS_BALANCE");
397:
398:        shares[_sender] = currentSenderShares.sub(_sharesAmount);
```

Line 396 requires that `_sharesAmount <= currentSenderShares`, thus `currentSenderShares - (_sharesAmount)` will never underflow. This operation can be `unchecked`, instead of being calculated with a help of SafeMath `sub` function.