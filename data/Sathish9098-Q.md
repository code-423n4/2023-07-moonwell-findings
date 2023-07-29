# LOW FINDINGS

##

## [L-1] ``_newEndTime`` should be in the future. ``_endTime > block.timestamp + 1`` should be used instead of ``_newEndTime > block.timestamp`` 

### Impact

Using ``_newEndTime > block.timestamp`` to check if the new end time is in the future is not sufficient because it allows the new end time to be set to the current block timestamp, which is not what you'd want in most cases.To guarantee that ``_newEndTime`` is in the future, you should use the comparison ``_newEndTime > block.timestamp + 1`` instead. This ensures that the new end time is at least one second greater than the current block timestamp, making it a valid future time.

```solidity
FILE: Breadcrumbs2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

793: require(
794:            _newEndTime > block.timestamp,
795:            "_newEndTime MUST be > block.timestamp"
796:        );

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L793-L796

### Recommended Mitigation
Change the _newEndTime check same like _addEmissionConfig() function . This will ensures the valid future time.

```solidity

require(
            _endTime > block.timestamp + 1,
            "The _endTime parameter must be in the future!"
        );


```
## [L-2] Lack of integer validations in _setEmissionCap() function leads unexpected behavior 

### Impact
Lack of integer validations in the _setEmissionCap() function can indeed lead to unexpected behavior and potential vulnerabilities due to human errors.Setting a very low emission cap unintentionally could limit the contract's functionality or prevent it from functioning as intended. It could also have adverse effects on token economics if the emission cap is set too low

### POC

```solidity
FILE: Breadcrumbs2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

function _setEmissionCap(
        uint256 _newEmissionCap
    ) external onlyComptrollersAdmin {
        uint256 oldEmissionCap = emissionCap;

        emissionCap = _newEmissionCap;

        emit NewEmissionCap(oldEmissionCap, _newEmissionCap);
    }

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L512-L520

### Recommended Mitigation

- Add grater than zero check

```
require(_newEmissionCap > 0, ``Not a zero ``);

```
- Add Min and Max Values check 

```
require(_newEmissionCap > MIN && _newEmissionCap < MAX, ``Not a zero ``);

```

##

## [L-3] Owner may set less ``total supply limit`` in updateMarketSupplyIndexInternal() 

### Impact

There is no explicit check for the total supply limit of the MToken (_mToken). This lack of validation could indeed lead to potential issues if the owner mistakenly sets the total supply to a value less than the actual circulating supply or if the total supply is incorrectly changed.

### POC

```solidity
FILE: Breadcrumbs2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

 function updateMarketSupplyIndex(
        MToken _mToken
    ) external onlyComptrollerOrAdmin {
        updateMarketSupplyIndexInternal(_mToken);
    }

function updateMarketSupplyIndexInternal(MToken _mToken) internal {
        MarketEmissionConfig[] storage configs = marketConfigs[
            address(_mToken)
        ];

        uint256 totalMTokens = MTokenInterface(_mToken).totalSupply();

        // Iterate over all market configs and update their indexes + timestamps
        for (uint256 index = 0; index < configs.length; index++) {
            MarketEmissionConfig storage emissionConfig = configs[index];

            // Go calculate our new values
            IndexUpdate memory supplyUpdate = calculateNewIndex(
                emissionConfig.config.supplyEmissionsPerSec,
                emissionConfig.config.supplyGlobalTimestamp,
                emissionConfig.config.supplyGlobalIndex,
                emissionConfig.config.endTime,
                totalMTokens
            );

            // Set the new values in storage
            emissionConfig.config.supplyGlobalIndex = supplyUpdate.newIndex;
            emissionConfig.config.supplyGlobalTimestamp = supplyUpdate
                .newTimestamp;
            emit GlobalSupplyIndexUpdated(
                _mToken,
                emissionConfig.config.emissionToken,
                supplyUpdate.newIndex,
                supplyUpdate.newTimestamp
            );
        }
    }

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L536-L540

### Recommended Mitigation
Consider implementing checks in the function to validate that the _mToken's total supply is within a reasonable range and not below the current circulating supply

```solidity

uint256 currentTotalSupply = MTokenInterface(_mToken).totalSupply();
require(currentTotalSupply >= totalMTokens, "Invalid total supply: Total supply cannot be decreased.");

```

##

## [L-4] ``MultiRewardDistributor`` contract inherited the ``Pausable.sol`` but not actually Pausable

### Impact
MultiRewardDistributor inherits from Pausable.sol but does not actually implement the pausable functionality, it can lead to confusion and potential security risks. The Pausable pattern is used to add a pausing mechanism to a smart contract, allowing the contract owner to pause and unpause certain functions temporarily. It's used to handle emergency situations, bugs, or unexpected behavior.

### POC

```solidity
FILE: 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

44: Pausable,

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L44

### Recommended Mitigation
 ``whenNotPaused`` can be used to pause and resume certain critical operations

##

## [L-5] ``_emissionConfig.supplierIndices[_supplier]`` in some cases this will return default uint256 value 0 

### Impact
 If the ``_supplier`` is a valid address but has not yet interacted with the contract or does not have an associated index in the mapping, accessing ``_emissionConfig.supplierIndices[_supplier]`` will return the default value for ``uint256``, which is ``0``. Depending on how the contract uses this index, it may lead to incorrect calculations or undesired behavior.

### POC

```solidity
FILE: 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

833: uint256 userSupplyIndex = _emissionConfig.supplierIndices[_supplier];

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L833

### Recommended Mitigation
One way to do this is to use the ``.contains()`` function provided by a mapping library like OpenZeppelin's

##

## [L-6] ``calculateSupplyRewardsForUser()``, ``calculateBorrowRewardsForUser()`` functions does not explicitly check for the existence of the ``_supplier`` address in ``_emissionConfig.supplierIndices`` Mapping

### Impact
If the _emissionConfig.supplierIndices mapping should only contain valid addresses that have been added beforehand, it might be necessary to add additional validation to ensure that _supplier is a valid address before accessing its index in the mapping. It directly accesses ``userSupplyIndex`` from the mapping, assuming that the address exists.

### POC

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L827C2-L855

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L865-L899

### Recommended Mitigation
Validate that the _supplier address exists in the set before accessing its index in the mapping

##

## [L-7] There is no guarantee that the `` _user`` address can receive ETH

### Impact
There is no guarantee that ``_user`` can receive ETH directly in the ``sendReward`` function, especially if ``_user`` is a contract address or an externally owned address that does not implement a fallback function to receive ETH.

Sending ETH directly to a contract or an externally owned address without a payable fallback function will result in a transaction failure, and the transfer will not be successful. There is no address(0) checks

Before sending ETH, you can check if _user is a contract address using the isContract function. If it's a contract, you can check if it has a payable fallback function to receive ETH. If it does, proceed with the ETH transfer. If it doesn't, handle the situation accordingly (e.g., log an event, revert the transaction, etc.).

### POC

```solidity
FILE: Breadcrumbs2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

1215: address payable _user,

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1215C9-L1215C31

### Recommended Mitigation 
``sendEthReward`` function checks if ``_user`` is a contract and whether it has a payable fallback function before sending ETH.






abi.encodepacked may be hash collisions 

lack of sanity checks when dealing integers 

