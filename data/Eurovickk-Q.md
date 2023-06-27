
https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/ProtocolRewardsPool.sol

Use uint256 for grabFeeRatio: As the grabFeeRatio variable is used for calculations involving uint256 values, it's recommended to change its type to uint256 instead of uint. This ensures consistency and avoids potential issues.

https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/PeUSDMainnetStableVision.sol

The convertToPeUSD function allows any address to call it and convert eUSD to PeUSD. Consider adding additional access control mechanisms to restrict the function's usage only to trusted addresses.
It is generally recommended to explicitly specify the visibility (e.g., public, external, internal, private) for all functions and modifiers to improve code readability and reduce the likelihood of unintended access.

In the convertToPeUSD function, you can optimize gas by using the safeTransferFrom function instead of transferFrom to transfer eUSD tokens. The safeTransferFrom function checks if the transfer was successful and reverts the transaction if it fails, preventing potential loss of funds.
In the convertToEUSD function, you can optimize gas by using the safeTransferShares function instead of transferShares to transfer shares of eUSD. Similar to the previous point, using the safeTransferShares function ensures the transfer is successful before proceeding. 

function convertToPeUSD(address user, uint256 eusdAmount) public {
    require(_msgSender() == user || _msgSender() == address(this), "MDM");
    require(EUSD.balanceOf(address(this)) + eusdAmount <= configurator.getEUSDMaxLocked(), "ESL");
    bool success = EUSD.safeTransferFrom(user, address(this), eusdAmount);
    require(success, "TF");
    uint256 share = EUSD.getSharesByMintedEUSD(eusdAmount);
    userConvertInfo[user].depositedEUSDShares += share;
    userConvertInfo[user].mintedPeUSD += eusdAmount;
    _mint(user, eusdAmount);
}

function convertToEUSD(uint256 peusdAmount) external {
    require(peusdAmount <= userConvertInfo[msg.sender].mintedPeUSD && peusdAmount > 0, "PCE");
    _burn(msg.sender, peusdAmount);
    uint256 share = (userConvertInfo[msg.sender].depositedEUSDShares * peusdAmount) / userConvertInfo[msg.sender].mintedPeUSD;
    userConvertInfo[msg.sender].mintedPeUSD -= peusdAmount;
    userConvertInfo[msg.sender].depositedEUSDShares -= share;
    EUSD.safeTransferShares(msg.sender, share);
}

In the modified code, the safeTransferFrom function is used in the convertToPeUSD function to transfer eUSD tokens, and the safeTransferShares function is used in the convertToEUSD function to transfer shares of eUSD. These functions will revert the transaction if the transfers fail, ensuring the safety of the funds being transferred.


https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/miner/stakerewardV2pool.sol


The updateReward modifier currently updates the reward-related data for a user, including the rewards, userRewardPerTokenPaid, and userUpdatedAt mappings. It's important to ensure that the _account parameter is not the zero address to avoid potential issues. For example: 
Reward Calculation: If the zero address is used as the _account, it can lead to incorrect reward calculations. The modifier updates the rewards mapping for the _account and calculates the earned rewards based on the balance and boost of the account. If the zero address is allowed, it may result in undesired rewards or rewards being incorrectly allocated.
Mapping Corruption: Allowing the zero address as the _account parameter could potentially corrupt the mappings used to track rewards and other user-related data. Storing data for the zero address may lead to unexpected behavior and make it challenging to distinguish between real user accounts and the zero address.
Security Vulnerabilities: Allowing the zero address as a valid account can introduce security vulnerabilities. If certain functions or operations rely on user-specific data, processing data for the zero address may result in unintended consequences or even security breaches.


https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/token/LBR.sol


Max Supply Check: In the mint function, the check totalSupply() + amount <= maxSupply ensures that the total supply doesn't exceed the maximum supply. However, the current implementation doesn't account for the case where totalSupply() is already close to maxSupply and adding amount would cause an overflow. Consider using the SafeMath library or the SafeERC20 wrapper from OpenZeppelin to handle arithmetic operations safely and prevent potential overflows.

Security Considerations: It's important to note that the mint and burn functions are currently accessible by any address as they don't have any access control mechanisms in place. Consider adding appropriate access control modifiers to restrict these functions to authorized addresses.

contract LBR is BaseOFTV2, ERC20 {
    // ...
    // Define a modifier to restrict access to authorized addresses
    modifier onlyTokenMiner() {
        require(configurator.tokenMiner(msg.sender), "OFT: Not authorized");
        _;
    }
    // ...
    // Function to mint new LBR tokens
    function mint(address user, uint256 amount) external onlyTokenMiner returns (bool) {
        require(totalSupply() + amount <= maxSupply, "OFT: Exceeding the maximum supply quantity.");
        _mint(user, amount);
        return true;
    }
    // Function to burn LBR tokens
    function burn(address user, uint256 amount) external onlyTokenMiner returns (bool) {
        _burn(user, amount);
        return true;
    }
    // ...
}
In this updated code, we have added the onlyTokenMiner modifier, which restricts access to the mint and burn functions to addresses that are authorized as token miners. The modifier checks if the msg.sender (the caller of the function) is authorized using the configurator.tokenMiner function. Only if the caller is authorized, the function execution proceeds.
By adding this access control modifier, we ensure that only authorized addresses can mint and burn LBR tokens, enhancing the security of the contract. You can modify the modifier name and access control mechanism according to your specific authorization requirements.


https://github.com/code-423n4/2023-06-lybra/blob/main/contracts/lybra/pools/LybraRETHVault.sol


In the constructor, the rkPool variable is initialized with a hardcoded address, but there is also a setRkPool function to update it. Consider using one approach consistently to initialize and update the rkPool address.


