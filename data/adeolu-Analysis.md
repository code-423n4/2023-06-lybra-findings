## Approach taken in evaluating the codebase

Used manual review + forge for testing and solodit for comparison with similar issues. 

## Anything that you learned from evaluating the codebase

Learnt more about liquidity staking tokens. 


## Any comments for the judge to contextualize your findings

Noticed a few reverts due to possible division by 0 or subtraction of larger uint from smaller uint. I believe these cases should have been checked and can be easily avioded. Please note that especially where the reverts can be caused by a value derieved/gotten/is from a user/externally supplied arg, the possibility of it happening increases exponentially. 

### Time spent:
38 hours