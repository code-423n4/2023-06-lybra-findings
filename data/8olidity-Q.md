## The function does not exist in the WBETH interface
The LybraWBETHVault contract comment wrote that the WBETH address is 0xae78736Cd615f374D3085123A210448E74Fc6393 , but in the  https://etherscan.io/address/0xae78736cd615f374d3085123a210448e74fc6393#readContract , the contract does not have the `deposit` and `exchangeRatio` functions written in the interface.

```solidity
interface IWBETH {
    function exchangeRatio() external view returns (uint256);

    function deposit(address referral) external payable;
}

contract LybraWBETHVault is LybraPeUSDVaultBase {
    //WBETH = 0xae78736Cd615f374D3085123A210448E74Fc6393
```

Annotation error or interface error