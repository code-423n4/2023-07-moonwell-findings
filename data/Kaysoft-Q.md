## [NC-1] Use the Interface Reference for parameters instead of address

File: 
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L62

```
onstructor(
        address wormholeCore,//@audit use the interface IWormhole instead of address.
        uint256 _proposalDelay,
        uint256 _permissionlessUnpauseTime,
        TrustedSender[] memory _trustedSenders
    ) Ownable() {
-  wormholeBridge = IWormhole(wormholeCore); 
...
```
#### Recommended Mitigation Steps
Consider using the interface as the parameter as shown below.
```
onstructor(
+        IWormhole wormholeCore 
        uint256 _proposalDelay,
        uint256 _permissionlessUnpauseTime,
        TrustedSender[] memory _trustedSenders
    ) Ownable() {
...
```

## [NC-2] The `_sanityCheckPayload()` function should be invoked earlier in the `_executeProposal()` function.
In order to follow the check-effects-interaction pattern, the `_sanityCheckPayload()` function should be invoked at the upper part of the function before the state updates that were done.
Moving the `_sanityCheckPayload()` up will even allow transactions that does not meet the validation fail earlier before exhausting more gas from other operations.

File: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L392
```
function _executeProposal(bytes memory VAA, bool overrideDelay) private {
...//some codes omitted for clarity
queuedTransactions[vm.hash].executed = true;

        address[] memory targets; /// contracts to call
        uint256[] memory values; /// native token amount to send
        bytes[] memory calldatas; /// calldata to send
        (, targets, values, calldatas) = abi.decode(
            vm.payload,
            (address, address[], uint256[], bytes[])
        );

        /// Interaction (s)

        _sanityCheckPayload(targets, values, calldatas);//@audit sanity checks should be done earlier to follow check-effect.
...//some codes omitted for clarity
}
```

#### Recommended Mitigation steps
Consider moving the `_sanityCheckPayload(targets, values, calldatas);` function above the `queuedTransactions[vm.hash].executed = true;` line.

## [NC-3] Consider removing commented code
Commented codes sometimes indicates unfinished work

File: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MToken.sol#L125

#### Recommended Mitigation Steps
Consider removing the commented

## [NC-4] Consider removing empty code block or emit event
Consider removing empty code block or emit event

File: https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20Delegate.sol#L15

#### Recommended Mitigation Steps
Consider removing the empty code block or emit event

## [NC-5] Emit event for the `mint` and the `redeem()` functions of WethRouter.sol contract.
Events are needed by frontend applications and monitoring tools.
The `mint` and the `redeem()` functions of WethRouter.sol contract do not any event.

Files: 
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/router/WETHRouter.sol#L31
- https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/router/WETHRouter.sol#L45

#### Recommended Mitigation Steps
Consider adding events to the `mint` and the `redeem()` functions of WethRouter.sol contract.

