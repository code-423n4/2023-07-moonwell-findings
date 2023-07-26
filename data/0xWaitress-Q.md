## [N-1] remove liquidateBorrowVerify in ComtrollerInterface if this is no use
```solidity
    function liquidateBorrowVerify(
        address mTokenBorrowed,
        address mTokenCollateral,
        address liquidator,
        address borrower,
        uint repayAmount,
        uint seizeTokens) virtual external;
```