Vulnerability in Minting Process: Lack of Control and Validation on Individual Mint Amount (Ignoring Suggested Limitation Comment)

     * @notice The mint amount number of EUSD is minted to the address
     * Emits a `Mint` event.
     *
     * Requirements:
     * - `onBehalfOf` cannot be the zero address.
     * - `amount` Must be higher than 0. Individual mint amount shouldn't surpass 10% when the circulation reaches 10_000_000
     */
    function mint(address onBehalfOf, uint256 amount) external {
        require(onBehalfOf != address(0), "MINT_TO_THE_ZERO_ADDRESS");
        require(amount > 0, "ZERO_MINT");
        _mintEUSD(msg.sender, onBehalfOf, amount, getAssetPrice());