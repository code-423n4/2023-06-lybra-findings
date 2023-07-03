# mockCurve.sol

1. Unused function parameter:int128 i, int128 j, uint256 dx

function get_dy_underlying(int128 i, int128 j, uint256 dx) external view returns (uint256) {
        return price;
}

2. Unused function parameter: int128 i, int128 j

function exchange_underlying(int128 i, int128 j, uint256 dx, uint256 min_dy) external returns(uint256) {
        eusd.transferFrom(msg.sender, address(this), dx);
        usdc.transfer(msg.sender, min_dy);
        return min_dy;
}

3. adding public - a visibility specifier - to a constructor has no effect.

mockWstETH.sol

  constructor(IStETH _stETH)
        public
        ERC20Permit("Wrapped liquid staked Ether 2.0")
        ERC20("Wrapped liquid staked Ether 2.0", "wstETH")
    {
        stETH = _stETH;
    }