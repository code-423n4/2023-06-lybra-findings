a) esLBBoost.sol
   there is no check in addLockSetting function to check if lockSetting is available for same duration, 
   and hence there is a possibility to add LBR lock settings for same window with different miningBoost.

   Existing:
     esLBRLockSettings.push(esLBRLockSetting(30 days, 20 * 1e18));
     esLBRLockSettings.push(esLBRLockSetting(90 days, 30 * 1e18));
     esLBRLockSettings.push(esLBRLockSetting(180 days, 50 * 1e18));
     esLBRLockSettings.push(esLBRLockSetting(365 days, 100 * 1e18));

   and could add
    esLBRLockSettings.push(esLBRLockSetting(30 days, 15 * 1e18));
    esLBRLockSettings.push(esLBRLockSetting(30 days, 100 * 1e18));

   As there is no function to remove the record added by mistake, it remains as option to select.

b) setLockStatus accepts index as parameter to select the lock settings. As there is no check for bounds on the incoming parameter, passing values greater than size of lockSetting array will throw error. 


c) It is not evident from the code base, how the LybraProxy and LybraProxyAdmin are being used.
   There are no upgradeable contracts found.