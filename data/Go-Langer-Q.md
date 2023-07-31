[L] (mToken.mint(msg.value) reverts due to missing implementation in mToken contract

There is currently no implementation for the mint function in the MToken contract.

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/router/WETHRouter.sol#L34


Which means the following line will revert in the WETHRouter contract from the mint function:

```
        require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");
```

Mitigation:

Please implement the mint function in the MToken contract.