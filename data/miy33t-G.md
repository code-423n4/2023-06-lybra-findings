I found a function that is note used, and that cost gas in deploy phase.
-> BitLib.sol - Ligne 52 - function countSetBits(uint x) internal pure returns (uint256)

I found that there is an external function that stock input args in memory so we can replace it with calldata
-> EUSDMiningIncentives.sol - Ligne 93 - function setPools(address[] memory _pools) external
-> LZEndpontMock.sol - Ligne 114 - function send(uint16 _chainId, bytes memory _path, bytes calldata _payload, address payable _refundAddress, address _zroPaymentAddress, bytes memory _adapterParams) external
