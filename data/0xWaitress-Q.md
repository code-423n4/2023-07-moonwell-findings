## [L-1] anyone can claimRewards for others which is undesirable for some users causing tax issue.

claimReward can be called by anyone to trigger reward collection for any particular user. This is fine with security but may cause taxation issue or make some user/contract to collect tokens at the time when the owner does not deem appropriate to call.

```solidity
    function claimReward(address[] memory holders, MToken[] memory mTokens, bool borrowers, bool suppliers) public {
```

Recommendation:
Consider only allow the msg.sender to claimReward, or add a reward allowance delegation function.

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