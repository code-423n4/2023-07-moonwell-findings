#LL-01 Checks for zero address in _acceptImplementation and _acceptAdmin functions are unnecessary
A message can never be sent by the zero address and both functions. The first check of both statements that include the check for the zero address would already fail in case the zero address was set as pending. https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L61, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Unitroller.sol#L111. The check for the zero address can be removed.

#LL-02 Using underscore prefix for external/public functions
Bot findings highlight that "Non-external/public variable and function names should begin with an underscore". But in the codebase, there is also underscores used as a prefix for external/public functions. Examples: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L160, https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L412. This should be cleaned up.

#LL-03 Same code blocks have different indentations for comments in MErc20.sol
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L194-L202 and https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L194-L202 that are identical but have different indentations for comments. This should be cleaned up.

#LL-04 Typo in MErc20Delegator.sol
It should be "setter" instead of "settor": https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegator.sol#L48.