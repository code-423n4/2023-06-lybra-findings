 require(assetAmount * 2 <= depositedAsset[onBehalfOf], "a max of 50% collateral can be liquidated");
it said a max of 50% collateral can be liquidated but it took more than that.
   uint256 reducedAsset = (assetAmount * 11) / 10;
        totalDepositedAsset -= reducedAsset;
        depositedAsset[onBehalfOf] -= reducedAsset;
it took more 1/10 of max collateral can be liquidated