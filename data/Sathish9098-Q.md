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
##

## [L-2] The ``safe32(block.timestamp)`` returns truncated values after year 2106 

### Impact
After the year 2106, the block.timestamp will continue to increment normally, but the truncated timestamp variable will reset to zero after reaching its maximum value (2^32 - 1). As a result, the timestamp variable will behave as if it's cycling every 2^32 seconds (approximately 136 years), causing it to lose track of the actual time

```solidity
FILE: 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

436: supplyGlobalTimestamp: safe32(
                block.timestamp,
                "block timestamp exceeds 32 bits"
            ),

442: borrowGlobalTimestamp: safe32(
                block.timestamp,
                "block timestamp exceeds 32 bits"
            ),

919: uint32 blockTimestamp = safe32(
            block.timestamp,
            "block timestamp exceeds 32 bits"
        );

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L436

### Recommended Mitigation
When even handling ``block.timestamp`` use at least ``uint128 `` to avoid unexpected behaviors 

##

## [L-3] Don't trust single ``oracle`` for ``price ``

### Impact
Currently protocol only uses ``Chainlink`` Oracle  for prices 

You should not trust a single oracle for price calculations. There are a number of reasons why this is the case

- ``Single oracles can be attacked``: If a single oracle is attacked, it could be compromised and provide incorrect price information. This could lead to significant losses for users who rely on the price information from the oracle.
- ``Single oracles can be unreliable``: Even if a single oracle is not attacked, it could still be unreliable. This could be due to technical problems, such as network outages or hardware failures. It could also be due to human error.
- ``Single oracles can be biased``: A single oracle could be biased towards providing certain price information. This could be due to a number of factors, such as the oracle's own financial interests or the interests of its owners.

### Recommended Mitigation
Using multiple oracles for price calculations is to use a ``weighted average``. A ``weighted average`` is a method of combining the price ``information`` from ``multiple oracles``. The weight of each oracle is based on its ``reliability``. This approach can help to ensure that the price information is ``accurate`` and ``reliable``, even if ``some of the oracles are unreliable``

##

## [L-4] Lack of integer validations in _setEmissionCap() function leads unexpected behavior 

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

## [L-5] Owner may set less ``total supply limit`` in updateMarketSupplyIndexInternal() 

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

## [L-6] ``MultiRewardDistributor`` contract inherited the ``Pausable.sol`` but not actually Pausable

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

## [L-7] ``_emissionConfig.supplierIndices[_supplier]`` in some cases this will return default uint256 value 0 

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

## [L-8] ``calculateSupplyRewardsForUser()``, ``calculateBorrowRewardsForUser()`` functions does not explicitly check for the existence of the ``_supplier`` address in ``_emissionConfig.supplierIndices`` Mapping

### Impact
If the _emissionConfig.supplierIndices mapping should only contain valid addresses that have been added beforehand, it might be necessary to add additional validation to ensure that _supplier is a valid address before accessing its index in the mapping. It directly accesses ``userSupplyIndex`` from the mapping, assuming that the address exists.

### POC

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L827C2-L855

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L865-L899

### Recommended Mitigation
Validate that the _supplier address exists in the set before accessing its index in the mapping

##

## [L-9] There is no guarantee that the `` _user`` address can receive ETH

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

##

## [L-10]  Using ``token.balanceOf(address(this))`` every time this will reduce the contract performance 

### Impact

Using ``token.balanceOf(address(this))`` every time can potentially reduce the contract's performance. Frequent external calls to other contracts, such as reading the balance of the token contract, can be expensive in terms of gas costs and execution time. This can lead to higher transaction costs and slower contract execution.

### POC

```solidity
FILE: Breadcrumbs2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

480:  token.balanceOf(address(this))

1232: uint256 currentTokenHoldings = token.balanceOf(address(this));

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1232

### Recommended Mitigation
Consider State variables instead of ``token.balanceOf(address(this))``

##

## [L-11] Project has NPM Dependency which uses a vulnerable version : @openzeppelin

### Impact

Project mostly uses openzeppelin 4.9.0. There is latest version available 4.9.3

There are known issues in this version

 - Improper Input Validation
 - Missing Authorization

### POC

{
  "name": "openzeppelin-solidity",
  "version": "4.9.0",

### Recommended Mitigation 
Use latest version v4.9.3

##

## [L-12] Insufficient coverage

```
- What is the overall line coverage percentage provided by your tests?: 80%

```
### Impact
The code base, the 80% line coverage percentage means that 80% of the lines of code in the contract have been executed by the tests. This is a good start, but it is not enough to ensure that the contract is fully tested.

### Recommended Mitigation
Achieve 100% Percent lines coverage to avoid risks 

##

## [L-13] Update codes to avoid Unexpected behavior 

There are warnings in the codebase. Try to rectify all warnings to avoid unexpected behaviors in protocol 

```

Warning (2519): Warning: This declaration shadows an existing declaration.
   --> src/core/Governance/deprecated/MoonwellArtemisGovernor.sol:349:9:
    |
349 |         ProposalState state = state(proposalId);
    |         ^^^^^^^^^^^^^^^^^^^
Note: The shadowed declaration is here:
   --> src/core/Governance/deprecated/MoonwellArtemisGovernor.sol:372:5:
    |
372 |     function state(uint proposalId) public view returns (ProposalState) {
    |     ^ (Relevant source part starts here and spans across multiple lines).


Warning (3628): Warning: This contract has a payable fallback function, but no receive ether function. Consider adding a receive ether function.
  --> src/core/MErc20Delegator.sol:11:1:
   |
11 | contract MErc20Delegator is MTokenInterface, MErc20Interface, MDelegatorInterface {
   | ^ (Relevant source part starts here and spans across multiple lines).
Note: The payable fallback function is defined here.
   --> src/core/MErc20Delegator.sol:496:5:
    |
496 |     fallback () external payable {
    |     ^ (Relevant source part starts here and spans across multiple lines).


Warning (2462): Warning: Visibility for constructor is ignored. If you want the contract to be non-deployable, making it "abstract" is sufficient.
   --> src/core/Governance/deprecated/MoonwellArtemisGovernor.sol:259:5:
    |
259 |     constructor(
    |     ^ (Relevant source part starts here and spans across multiple lines).


Warning (2462): Warning: Visibility for constructor is ignored. If you want the contract to be non-deployable, making it "abstract" is sufficient.
  --> src/core/MLikeDelegate.sol:19:3:
   |
19 |   constructor() public MErc20Delegate() {}
   |   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Warning (2072): Warning: Unused local variable.
  --> test/integration/LiveSystem.t.sol:66:9:
   |
66 |         uint256 startingTokenBalance = token.balanceOf(sender);
   |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^

```

##

## [L-14] Token transfer to address(0) should be avoided

### Impact
The external function sendReward() ignores the use of address(0).Need to check address(0) even when using ``safeTransfer``. ``safeTransfer`` is a function that is designed to prevent certain types of errors from occurring when transferring tokens. However, it does not protect against all possible errors.

### POC
```solidity
FILE: 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

function sendReward(
        address payable _user,
        uint256 _amount,
        address _rewardToken
    ) internal nonReentrant returns (uint256) {
        // Short circuit if we don't have anything to send out
        if (_amount == 0) {
            return _amount;
        }

        // If pause guardian is active, bypass all token transfers, but still accrue to local tally
        if (paused()) {
            return _amount;
        }

        IERC20 token = IERC20(_rewardToken);

        // Get the distributor's current balance
        uint256 currentTokenHoldings = token.balanceOf(address(this));

        // Only transfer out if we have enough of a balance to cover it (otherwise just accrue without sending)
        if (_amount > 0 && _amount <= currentTokenHoldings) {
            // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
            token.safeTransfer(_user, _amount);
            return 0;
        } else {
            // If we've hit here it means we weren't able to emit the reward and we should emit an event
            // instead of failing.
            emit InsufficientTokensToEmit(_user, _rewardToken, _amount);

            // By default, return the same amount as what's left over to send, we accrue reward but don't send them out
            return _amount;
        }
    }

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1237

### Recommended Mitigation
Add ``address(0)`` check before calling ``safeTransfer()`` function 

```
require(_user!=address(0), "Zero Address");

```
##

## [L-15] Deprecated Comment

### Impact

The comment ```/// @dev this branch should never get called as native tokens are not supported on this deployment`` is outdated and does not match the actual implementation. As mentioned earlier, nativeToken is not used in the contract, so the branch is indeed getting called and can be problematic.

### POC

```solidity
FILE: 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol

62: if (keccak256(abi.encodePacked(symbol)) == nativeToken) { /// @dev this branch should never get called as native tokens are not supported on this deployment

```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Oracles/ChainlinkOracle.sol#L62

##

## [L-16] 











