The require() check inside the mintAllowed() function checks to see if nextTotalSupplies < supplyCap, not allowing the user to be approved to mint.

While this should be appropriate if the nextTotalSupplies is greater than the supply cap, it explicitly uses the < symbol. Is the user not allowed to mind if nextTotalSupplies == supplyCap? If that were the case he should replace "<" with "<=".

I also found this inside the borrowAllowed() function with the same idea, except for nextTotalBorrows < borrowCap. Forgive me if my explanation is poor, I am a beginner.

Recommendation: If users are allowed to borrow or mint when we are == to the cap and not explicitly less than it, we should replace "<" with "<="

Instances:

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L236C1-L236C1
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L341C13-L341C13