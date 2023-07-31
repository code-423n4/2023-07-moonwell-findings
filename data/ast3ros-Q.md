# [L-1] Missing min and max checking when setting closeFactorMantissa

There is no min and max checking when setting `closeFactorMantissa`. The admin can mistakenly set it to wrong number that break the operation of the protocol. For example if it is set to 0, no one can liquidate. If it set to more than 1, the liquidation and accounting process will be wrong.

https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/Comptroller.sol#L689-L698

# [L-2] Key parameters are not set in constructor in Comptroller contract

Some key parameters, such as `closeFactorMantissa` and `liquidationIncentiveMantissa`, are not set in the constructor. This can cause the protocol to operate incorrectly if the admin forgets to call the function to set them later.

It is advisable to set default values for these parameters in the constructor, so that they are initialized when the contract is deployed. This can prevent potential errors and ensure the proper functioning of the protocol.

https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/Comptroller.sol#L70-L72
https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/ComptrollerStorage.sol#L40
https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/ComptrollerStorage.sol#L45


# [L-3] protocolSeizeShareMantissa are not set in initialize in MToken contract

`protocolSeizeShareMantissa` is not set in the `initialize` function. This can cause the protocol to operate incorrectly if the admin forgets to call the function to set it later.
It is advisable to set default values for these parameters in the `initialize` function, so that they are initialized when the contract is deployed. This can prevent potential errors and ensure the proper functioning of the protocol.

https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/MErc20.sol#L23-L36
https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/MTokenInterfaces.sol#L81

# [N-1] Unupdated naming

The current code uses the variable name `blockDelta` to store the difference between the current and the previous block timestamps. This can be misleading, as it implies that the variable represents the number of blocks, not the time elapsed.

A better name for this variable would be `blockTimeStampDelta`, as it clearly indicates that it is the delta of the block timestamps. This can avoid confusion and make the code more readable and understandable.

https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/MToken.sol#L405-L407

# [N-2] Consider using descriptive constants instead of value

1e18 value should be replaced with constant to improve readability.

https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/IRModels/WhitePaperInterestRateModel.sol#L38-L39
https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/IRModels/WhitePaperInterestRateModel.sol#L57
https://github.com/code-423n4/2023-07-moonwell/blob/657fb2bcdb25ef44a4826b08390677c3fd41b92d/src/core/IRModels/WhitePaperInterestRateModel.sol#L83-L84