# L-01 Checks for zero address in _acceptImplementation and _acceptAdmin functions are unnecessary
A message can never be sent by the zero address and both functions. The first check of both statements that include the check for the zero address would already fail in case the zero address was set as pending. https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L61, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L111. The check for the zero address can be removed.

# L-02 Using underscore prefix for external/public functions
Bot findings highlight that "Non-external/public variable and function names should begin with an underscore". But in the codebase, there is also underscores used as a prefix for external/public functions. Examples: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L160, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L412. This should be cleaned up.

# L-03 Same code blocks have different indentations for comments in MErc20.sol
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L194-L202 and https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L194-L202 that are identical but have different indentations for comments. This should be cleaned up.

# L-04 Typo in MErc20Delegator.sol
It should be "setter" instead of "settor": https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L48.

# L-05 Development comments should be removed from codebase
Here is one example: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L71-L72. This should be documented in accompanying documents but not be left in code.

# L-06 TemporalGovernor.sol should potentially also emit TrustedSenderUpdated events for setting the trusted senders in the constructor
Setting and unsetting trusted senders via the respective functions of TemporalGovernor.sol emits TrustedSenderUpdated events (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L151-L154, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L178-L181). The constructor does not (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L74-L78), but could to have complete transparency from the start, who the trusted senders are (e.g. for off-chain risk monitoring).

# L-07 Unnecessary check for _amount > 0 in sendReward function of MultiRewardDistributor.sol
There is already a check that returns 0 if _amount is equal to 0 (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1220). The datatype (uint256) does not allow negative values so the additional check for _amount > 0 (https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1235) is unnecessary and can be removed.

# L-08 Unnecessary cast of comptroller.admin() to address in MultiRewardDistributor.sol
Here a cast is done to address: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L126. The same check is done a few lines lower without casting to address: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L136. The cast to address can be removed for keep consistency.

# L-09 "Set the new values in storage" comments within view functions of MultiRewardDistributor.sol
The following comments should be removed as it is only view functions: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L348, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L363.

# L-10 Inconsistency between contracts how timestampsPerYear constant is encoded
JumpRateModel.sol represents the state variable as an integer multiplication: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/JumpRateModel.sol#L20. WhitePaperInterestRateModel.sol represents the constant as a single integer (multiplication result): https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L20. The same representation should be used across all contracts for consistency. The integer multiplication should be preferred. 