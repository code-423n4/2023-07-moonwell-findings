## L-01 Checks for zero address in _acceptImplementation and _acceptAdmin functions are unnecessary
A message can never be sent by the zero address and both functions. The first check of both statements that include the check for the zero address would already fail in case the zero address was set as pending. https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L61, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L111. The check for the zero address can be removed.

## L-02 Using underscore prefix for external/public functions
Bot findings highlight that "Non-external/public variable and function names should begin with an underscore". But in the codebase, there is also underscores used as a prefix for external/public functions. Examples: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L160, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L412. This should be cleaned up.

## L-03 Same code blocks have different indentations for comments in MErc20.sol
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L194-L202 and https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L194-L202 that are identical but have different indentations for comments. This should be cleaned up.

## L-04 Typo in MErc20Delegator.sol
It should be "setter" instead of "settor": https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L48.

## L-05 Development comments should be removed from codebase
Here is one example: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L71-L72. This should be documented in accompanying documents but not be left in code.

## L-06 TemporalGovernor.sol should potentially also emit TrustedSenderUpdated events for setting the trusted senders in the constructor
Setting and unsetting trusted senders via the respective functions of TemporalGovernor.sol emits TrustedSenderUpdated events (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L151-L154, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L178-L181). The constructor does not (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L74-L78), but could to have complete transparency from the start, who the trusted senders are (e.g. for off-chain risk monitoring).

## L-07 Unnecessary check for _amount > 0 in sendReward function of MultiRewardDistributor.sol
There is already a check that returns 0 if _amount is equal to 0 (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1220). The datatype (uint256) does not allow negative values so the additional check for _amount > 0 (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1235) is unnecessary and can be removed.

## L-08 Unnecessary cast of comptroller.admin() to address in MultiRewardDistributor.sol
Here a cast is done to address: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L126. The same check is done a few lines lower without casting to address: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L136. The cast to address can be removed for keep consistency.

## L-09 "Set the new values in storage" comments within view functions of MultiRewardDistributor.sol
The following comments should be removed as it is only view functions: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L348, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L363.

## L-10 Inconsistency between contracts how timestampsPerYear constant is encoded
JumpRateModel.sol represents the state variable as an integer multiplication: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/JumpRateModel.sol#L20. WhitePaperInterestRateModel.sol represents the constant as a single integer (multiplication result): https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L20. The same representation should be used across all contracts for consistency. The integer multiplication should be preferred.

## L-11 Typo in WhitePaperInterestRateModel
It should be "timestamp" instead of "timestmp": https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L61, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L65.

## L-12 setTrustedSenders and unSetTrustedSenders of TemporalGovernor.sol should only emit events for actually added/removed trusted senders.
The add() and remove functions of EnumerableSet return true if actually a change to the set was made (https://docs.openzeppelin.com/contracts/4.x/api/utils#EnumerableSet-add-struct-EnumerableSet-Bytes32Set-bytes32-, https://docs.openzeppelin.com/contracts/4.x/api/utils#EnumerableSet-remove-struct-EnumerableSet-Bytes32Set-bytes32-. But the logic of TemporalGovernor.sol does not evaluate these return values and always emits the events (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L151-L155, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L178-L182). This should be corrected, and only when the add() or remove() functions returns true, the respective event should be emitted. With the current implementation, off-chain monitoring cannot differentiate whether a trusted sender was actually added or removed from the set.

## L-13 calculateSupplyRewardsForUser() and calculateBorrowRewardsForUser() functions of MultiRewardDistributor.sol contains commented-out critical code:

calculateSupplyRewardsForUser() contains:
```Solidity
userSupplyIndex = initialIndexConstant; //_globalSupplyIndex;
```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L839

calculateBorrowRewardsForUser() contains:
```Solidity
userBorrowIndex = initialIndexConstant; //userBorrowIndex = _globalBorrowIndex;
```
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L878

The commented-out code should be removed. It could get changed again and make it into production accidentally, which would change the accounting logic.

Change the error messages to the following to be more differentiated according to the "<" used for the check (>= should fail) and harmonize them regarding their wording (e.g., Cannot vs. Can't):

## L-14 Error messages in MultiRewardDistributor.sol regarding supply/borrow emissions and supply speed are copy pasted and, in parts, incorrect
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L401 should be: "Cannot set supply emissions per second equal to or higher than the emission cap!"
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L405 should be: "Cannot set borrow emissions per second equal to or higher than the emission cap!"
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L676 should be: "Cannot set supply speed equal to current supply speed!"
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L680C13-L680C77 should be: "Cannot set supply speed equal to or higher than the emission cap!"

## L-15 Wrong reference of "delegator" in _setImplementation() comments where it should be "delegate" in MErc20Delegator.sol
The comment should be "Called by the admin to update the implementation of the delegate" instead of "Called by the admin to update the implementation of the delegator" (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L56).