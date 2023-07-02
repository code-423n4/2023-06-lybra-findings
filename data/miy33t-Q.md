I put this issue here because I cannot submit another one in Gas optimization.
I found that you can optimize the storage in the file LybraConfiguration by just swapping two varibale

-> LybraConfiguration.sol
  Swap the variable "bool public premiumTradingEnabled" at ligne 61 with the variable "ICurvePool public curvePool" at ligne 60.