# Non-Critical Findings

## [N-01] Unused Imports

The [`mip00.sol`](https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.so) test contract imported the following files:

1. `ITransparentUpgradeableProxy` in the statement `import {TransparentUpgradeableProxy, ITransparentUpgradeableProxy} from "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol"`.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L6

2. `TimelockController` in the statement`import {TimelockController} from "@openzeppelin/contracts/governance/TimelockController.sol"`.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L7

3. `IVotes` in the statement `import {IVotes} from "@openzeppelin/contracts/governance/extensions/GovernorVotes.sol"`.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L9

4. `IERC20` in the statement `import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol"`.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L10

5. `TimelockProposal` in the satement `import {TimelockProposal} from "@test/proposals/proposalTypes/TimelockProposal.sol"`.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L15

6. `IWormhole` in the statement`import {IWormhole} from "@protocol/core/Governance/IWormhole.sol";`
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L25

7. `FaucetTokenWithPermit` in the statement `import {FaucetTokenWithPermit} from "@test/helper/FaucetToken.sol"`
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L33

8. `MockWeth` in the statement `import {MockWeth} from "@test/mock/MockWeth.sol"`
- https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L34

But these file were not used in the contract consider removing the import declarartion of these files as this is generally a bad coding practice and aslo these un-used imports pollute the `mip00.sol` contract namespace.



## [N-02]  Missing events in sensitive functions
When sensitive changes are made to the contracts, like changes in state variables events ought to be emitted, but in some functions changes were made to state and no event was emitted to corroborate the changes.

### 1 Instance
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L274-#L290

In the `togglePause()` function the values of the state variables `guardianPauseAllowed` and `lastPauseTime` were changed. The code should be refactored to emit an event that represents the changes made to state. Adding an event will facilitate offchain monitoring when changing system parameters.


## [N-03] Unnecessary use of `payable` keyword.
The `payable` keyword is used to create payable addresses for the transfer of the blockchain's native tokens. In transfer involving ERC-20 tokens the address does not have to be type converted to a payable address type. The function [`sendRewards()`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1214-#L1247) in the [MultiRewardDistributor.sol](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol) has an argument `-user` of type `address payable` but this `-user` argument was only used in the transfer of ERC-20 token therefore using the `payable` keyword in this instance is redundant also in the invocation of the sendRewards function on [line 1194-1198](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1194-#L1198) and [line 1084-1088](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1084-#L1088) the paremeters `_borrower` and `_supplier` were type converted to type payable address before been passed to the `sendRewards()` function which is unnecessary.

### Mitigation
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1214-#L1247
In the function declaration above the `payable` keyword in the `_user` argument declaration can be safely removed then the type conversion of [line 1195](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1194-#L1198) and [line 1085](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1084-#L1088) can also be removed. The diff below shows how the code could be refactored.
```diff
    function sendReward(
-       address payable _user,  //@audit user doesn't have to be a payable address to receive erc20 tokens
+       address _user,
        uint256 _amount,
        address _rewardToken
    ) internal nonReentrant returns (uint256) {
            .
            .
            .
            .
    }

    uint256 pendingRewards = sendReward(
-       payable(_borrower),
+       _borrower,
        emissionConfig.borrowerRewardsAccrued[_borrower],
        emissionConfig.config.emissionToken
    );

    uint256 unsendableRewards = sendReward(
-       payable(_supplier),
+       _supplier,
        emissionConfig.supplierRewardsAccrued[_supplier],
        emissionConfig.config.emissionToken
    );

```



## [N-04] Use multiple require() and if statements instead of &&
Using multiple require() statements rather than using boolean AND (&&) in multiple check combinations could help improve code readability and makes it easier to debug.

### 7 Instances
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L813
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L850
```solidity
file: /src/core/Comptroller.sol
813:    require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");
850:    require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");
```
The above line of code could be refactored as shown below to aid readability:
```diff
-   require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");

+   require(numMarkets != 0, "Invalid input");
+   require(numMarkets == numBorrowCaps, "invalid input");  
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L418-#L422
```solidity
file: /src/core/Governance/TemporalGovernor.sol
    require(
        targets.length == values.length &&
            targets.length == calldatas.length,
        "TemporalGovernor: Arity mismatch for payload"
    );
```
The above lines of code could be refactored as shown below to aid readability:
```diff
-    require(
-        targets.length == values.length &&
-            targets.length == calldatas.length,
-        "TemporalGovernor: Arity mismatch for payload"
-    );

+    require(targets.length == values.length, "TemporalGovernor: Arity mismatch for payload");
+    require(targets.length == calldatas.length, "TemporalGovernor: Arity mismatch for payload");

```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L33
```solidity
file: /src/core/MToken.sol
33:    require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");
```
The above line of code could be refactored as shown below to aid readability:
```diff
-   require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");

+   require(accrualBlockTimestamp == 0, "market may only be initialized once" ); 
+   require(borrowIndex == 0, "market may only be initialized once");
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L108-#L111
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L139-#L142
```solidity
file: src/core/Oracles/ChainlinkCompositeOracle.sol
108:    require(
109:        expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
110:        "CLCOracle: Invalid expected decimals"
111:    );
139:    require(
140:        expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
141:        "CLCOracle: Invalid expected decimals"
142:    );
```
The above lines of code could be refactored as shown below to aid readability:
```diff
-   require(
-       expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),
-       "CLCOracle: Invalid expected decimals"
-   );

+   require(expectedDecimals > uint8(0), "CLCOracle: Invalid expected decimals");
+   require(expectedDecimals <= uint8(18),"CLCOracle: Invalid expected decimals");
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol#L145-#L148
```solidity
file: src/core/Oracles/ChainlinkOracle.sol
145:    require(
146:        feed != address(0) && feed != address(this),
147:        "invalid feed address"
148:    );
```
The above lines of code could be refactored as shown below to aid readability:
```diff
-    require(
-        feed != address(0) && feed != address(this),
-        "invalid feed address"
-    );

+    require(feed != address(0), "invalid feed address"); 
+    require(feed != address(this), "invalid feed address");
```

## [N-05] Empty bytes check is missing
When you're developing smart contracts in Solidity, it's of utmost importance to carefully validate the inputs of your functions. This validation process is particularly critical for bytes parameters, especially when they hold essential data like addresses, identifiers, or raw information that the contract relies on for its operations.

Neglecting to check for empty bytes can give rise to unexpected and undesirable outcomes within your contract. For instance, certain operations might encounter failures, produce inaccurate results, or unnecessarily consume gas when performed with empty bytes. Additionally, overlooking input validation may potentially expose your contract to malicious activities, including the exploitation of unhandled edge cases.

To safeguard against these issues, it is crucial to consistently validate that bytes parameters are not empty whenever your contract's logic necessitates their usage. By implementing such validations, you can significantly reduce the risks associated with erroneous behavior and enhance the overall security and reliability of your smart contract.

### 2 Instances
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol#L471-#L473
```solidity
file: /src/core/MErc20Delegator.sol
471:    function delegateToImplementation(bytes memory data) public returns (bytes memory) {
472:        return delegateTo(implementation, data);
473:    }
```
The `delegateToImplementation()` function above has bytes memory parameter `data` which takes in a value of type bytes memory and passes it as an argument to the `delegateTo()` function which performs a delegate call. You should implement a check for the length of `data` so as to avoid performing a delegate call with empty bytes values, a probable exploitation of unhandled edge cases and any other unexpected scenarios. 
The code should be refactored as shown by the diff below:
```diff
    function delegateToImplementation(bytes memory data) public returns (bytes memory) {
+       if (data.length == 0)
+           revert("Empty data");
        return delegateTo(implementation, data);
    }
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol#L482-#L490
```solidty
file: /src/core/MErc20Delegator.sol
482:    function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {
483:        (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));
484:        assembly {
485:            if eq(success, 0) {
486:                revert(add(returnData, 0x20), returndatasize())
487:            }
488:        }
489:        return abi.decode(returnData, (bytes));
490:    }
```
The `delegateToViewImplementation()` function performs a staticcall so checking the length of the bytes value is important as it helps prevent the possibility of reading a wrong value if an empty bytes of data is mistakenly given as argument to the function. 
The code should be refactored as shown by the diff below:
```diff
    function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {
+       if (data.length == 0)
+           revert("Empty data");
        (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));
        assembly {
            if eq(success, 0) {
                revert(add(returnData, 0x20), returndatasize())
            }
        }
        return abi.decode(returnData, (bytes));
    }

```


## [N-06] Ternary operation is better in some situations than if-else statement
The normal if / else statement can be written in a shorthand way using the ternary operator. It increases readability and reduces the number of lines of code.

### 3 Instances
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L81-#L86
```solidity
file: src/core/MToken.sol
81:    uint startingAllowance = 0;
82:    if (spender == src) {
83:        startingAllowance = type(uint).max;
84:    } else {
85:        startingAllowance = transferAllowances[src][spender];
86:    }
```
The if-else statement above could be converted to a ternary statement to enhance its simplicity and readability and also reduce the number of lines of code.
The code could be refactored as shown in the diff below:
```diff
-    uint startingAllowance = 0;
-    if (spender == src) {
-        startingAllowance = type(uint).max;
-    } else {
-       startingAllowance = transferAllowances[src][spender];
-    }

+   uint startingAllowance = spender == src ? type(uint).max : transferAllowances[src][spender];

```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L633-#L637
```solidity
file: src/core/MToken.sol
633:    if (redeemTokensIn == type(uint).max) {
634:        vars.redeemTokens = accountTokens[redeemer];
635:    } else {
636:        vars.redeemTokens = redeemTokensIn;
637:    }
```
The if-else statement above could be converted to a ternary statement to enhance its simplicity and readability and also reduce the number of lines of code.
The code could be refactored as shown in the diff below:
```diff
-    if (redeemTokensIn == type(uint).max) {
-        vars.redeemTokens = accountTokens[redeemer];
-    } else {
-        vars.redeemTokens = redeemTokensIn;
-    }

+    vars.redeemTokens = redeemTokensIn == type(uint).max ? accountTokens[redeemer] : redeemTokensIn;
```
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L889-#L892
```solidity
file: src/core/MToken.sol
889:    if (repayAmount == type(uint).max) {
890:        vars.repayAmount = vars.accountBorrows;
891:    } else {
892:       vars.repayAmount = repayAmount;
893:    }
```
The if-else statement above could be converted to a ternary statement to enhance its simplicity and readability and also reduce the number of lines of code.
The code could be refactored as shown in the diff below:
```diff
-   if (repayAmount == type(uint).max) {
-       vars.repayAmount = vars.accountBorrows;
-   } else {
-      vars.repayAmount = repayAmount;
-   }

+   vars.repayAmount = repayAmount == type(uint).max ? vars.accountBorrows : repayAmount;
```

## [N-07]  else-block not required (6 instances was found by the bots)
One level of nesting can be removed by not having an else block when the if-block returns.

### 1 Instance
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1235-#L1246
```solidity
file: /src/core/MultiRewardDistributor/MultiRewardDistributor.sol
1235:    if (_amount > 0 && _amount <= currentTokenHoldings) {
1236:        // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
1237:        token.safeTransfer(_user, _amount);
1238:        return 0;
1239:    } else {
1240:        // If we've hit here it means we weren't able to emit the reward and we should emit an event
1241:        // instead of failing.
1242:        emit InsufficientTokensToEmit(_user, _rewardToken, _amount);
1243:
1244:        // By default, return the same amount as what's left over to send, we accrue reward but don't send them out
1245:        return _amount;
1246:    }
```
In the if-else statement above we could remove the else block since the if block returns 0 when its conditional is true. The code should be refactored as shown in the diff below:

```diff
    if (_amount > 0 && _amount <= currentTokenHoldings) {
        // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
        token.safeTransfer(_user, _amount);
        return 0;
    }
-   } else {
-       // If we've hit here it means we weren't able to emit the reward and we should emit an event
-       // instead of failing.
-       emit InsufficientTokensToEmit(_user, _rewardToken, _amount);
-
-       // By default, return the same amount as what's left over to send, we accrue reward but don't send them out
-       return _amount;
-   }

+   // If we've hit here it means we weren't able to emit the reward and we should emit an event
+   // instead of failing.
+   emit InsufficientTokensToEmit(_user, _rewardToken, _amount);
+
+   // By default, return the same amount as what's left over to send, we accrue reward but don't send them out
+   return _amount;

```
#### Please note that this instance was not reported by the bots.


## [N-08]  Variable names that consist of all capital letters should be reserved for constant/immutable variables 
According to solidity style guide the naming convetion for [argument of functions](https://docs.soliditylang.org/en/v0.8.20/style-guide.html#function-argument-names) is mixedCase while capital letters should be reserved for [constants](https://docs.soliditylang.org/en/v0.8.20/style-guide.html#constants). There are functions in the [`TemporalGovernor.sol`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol) contract whose arguments do not follow this standard and this could lead to confusions or mistakes while audting or implementing changes to the contract. The contract should be refactored and these be corrected.

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L229
```solidity
file: core/Governance/TemporalGovernor.sol
229:    function queueProposal(bytes memory VAA) external whenNotPaused {
230:        _queueProposal(VAA);
231:    }
```
The diff below shows how the `queueProposal()` function could be refactored:
```diff
-    function queueProposal(bytes memory VAA) external whenNotPaused {
-        _queueProposal(VAA);
-    }

+    function queueProposal(bytes memory vaa) external whenNotPaused {
+        _queueProposal(vaa);
+    }
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L237
```solidity
file: core/Governance/TemporalGovernor.sol
237:    function executeProposal(bytes memory VAA) public whenNotPaused {
238:        _executeProposal(VAA, false);
239:    }
```
The diff below shows how the `executeProposal()` function could be refactored:
```diff
-    function executeProposal(bytes memory VAA) public whenNotPaused {
-        _executeProposal(VAA, false);
-    }

+    function executeProposal(bytes memory vaa) public whenNotPaused {
+        _executeProposal(vaa, false);
+    }
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L266
```solidity
file: core/Governance/TemporalGovernor.sol
266:    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
267:        _executeProposal(VAA, true); /// override timestamp checks and execute
268:    }
```
The diff below shows how the `fastTrackProposalExecution()` function could be refactored:
```diff
-    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
-        _executeProposal(VAA, true); /// override timestamp checks and execute
-    }

+    function fastTrackProposalExecution(bytes memory vaa) external onlyOwner {
+        _executeProposal(vaa, true); /// override timestamp checks and execute
+    }
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L295
```solidity
file: core/Governance/TemporalGovernor.sol
295:    function _queueProposal(bytes memory VAA) private {
296:    /// Checks
297:
298:    // This call accepts single VAAs and headless VAAs
299:    (
300:        IWormhole.VM memory vm,
301:        bool valid,
302:        string memory reason
303:    ) = wormholeBridge.parseAndVerifyVM(VAA);
.
.
.
340:
341:        emit QueuedTransaction(intendedRecipient, targets, values, calldatas);
342:    }
```
The diff below shows how the `_queueProposal()` function could be refactored:
```diff
-   function _queueProposal(bytes memory VAA) private {
+   function _queueProposal(bytes memory vaa) private {
    /// Checks

    // This call accepts single VAAs and headless VAAs
    (
        IWormhole.VM memory vm,
        bool valid,
        string memory reason
-    ) = wormholeBridge.parseAndVerifyVM(VAA);
+   ) = wormholeBridge.parseAndVerifyVM(vaa);
                .
                .
                .
                .
        emit QueuedTransaction(intendedRecipient, targets, values, calldatas);
    }
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L344
```soldity
344:    function _executeProposal(bytes memory VAA, bool overrideDelay) private {
345:        // This call accepts single VAAs and headless VAAs
346:        (
347:            IWormhole.VM memory vm,
348:            bool valid,
349:            string memory reason
350:        ) = wormholeBridge.parseAndVerifyVM(VAA);
                    .
                    .
                    .
408:    }
```
The diff below shows how the `_executeProposal()` function could be refactored:
```diff
-   function _executeProposal(bytes memory VAA, bool overrideDelay) private {
+   function _executeProposal(bytes memory vaa, bool overrideDelay) private {
        // This call accepts single VAAs and headless VAAs
        (
            IWormhole.VM memory vm,
            bool valid,
            string memory reason
-       ) = wormholeBridge.parseAndVerifyVM(VAA);
+       ) = wormholeBridge.parseAndVerifyVM(VAA);
                    .
                    .
                    .
    }
```
#### Please note this instances were not included in the bots reports. 


## [N-09] Call `block.timestamp` directly instead of using function call
`block.timestamp` is a global variable therefore it can be used at anytime by any function of the contract so there is no reason you should defined a function to return `block.timestamp` when it can be accessed directly. If it was required for test, the function should be removed and anywhere it was invoked should be refactored to call `block.timestamp` directly.

### Instances and Mitigation
In the contract below the function `getBlockTimestamp()` should be removed.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1064-#L1066

In the contract below the function `getBlockTimestamp()` should be removed.
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L228-#L230



## [N-10] Creating unnecessary variable
In some instance the code could be refactored to not use some variable that were defined. It is good that when emitting an event that includes a new and an old value, you should emit both values for credibility and openness to users but in some instances you can avoid declaring a new variable to cache the old value you could Instead, emit the event, then save the new value in the variable.

### Instances
- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L493-#L406
```solidity    
file: src/core/MultiRewardDistributor/MultiRewardDistributor.sol
    function _setPauseGuardian(
        address _newPauseGuardian
    ) external onlyPauseGuardianOrAdmin {
        require(
            _newPauseGuardian != address(0),
            "Pause Guardian can't be the 0 address!"
        );
        address currentPauseGuardian = pauseGuardian;
        pauseGuardian = _newPauseGuardian;
        emit NewPauseGuardian(currentPauseGuardian, _newPauseGuardian);
    }
```
In the `_setPauseGuardian()` function above the is no need to declare the `currentPauseGuardian` variable as we can emit the event with `pauseGuardian` and `_newPauseGuardian` then assign the `_newPauseGuardian` value to `pauseGuardian`. The diff below shows how this could be done:

```diff
    function _setPauseGuardian(
        address _newPauseGuardian
    ) external onlyPauseGuardianOrAdmin {
        require(
            _newPauseGuardian != address(0),
            "Pause Guardian can't be the 0 address!"
        );

-       address currentPauseGuardian = pauseGuardian;

-       pauseGuardian = _newPauseGuardian;

-       emit NewPauseGuardian(currentPauseGuardian, _newPauseGuardian);

+       emit NewPauseGuardian(pauseGuardian, _newPauseGuardian);
+       pauseGuardian = _newPauseGuardian;   
    }
```

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L512-#L520
```solidity
    function _setEmissionCap(
        uint256 _newEmissionCap
    ) external onlyComptrollersAdmin {
        uint256 oldEmissionCap = emissionCap;
        emissionCap = _newEmissionCap;
        emit NewEmissionCap(oldEmissionCap, _newEmissionCap);
    }
```
In the `_setEmissionCap()` function above the is no need to declare the `oldEmissionCap` variable as we can emit the event with `emissionCap` and `_newEmissionCap` then assign the `_newEmissionCap` value to `emissionCap`. The diff below shows how this could be done:
```diff
    function _setEmissionCap(
        uint256 _newEmissionCap
    ) external onlyComptrollersAdmin {
-       uint256 oldEmissionCap = emissionCap;

-       emissionCap = _newEmissionCap;

-       emit NewEmissionCap(oldEmissionCap, _newEmissionCap);

+       emit NewEmissionCap(emissionCap, _newEmissionCap);

+       emissionCap = _newEmissionCap;      
    }
```


## [N-11] Unecessary statements

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L292#L301
```solidity
292:    function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {
293:        // Shh - currently unused
294:        mToken;     //@audit what's the purpose of this line?
295:        redeemer;   //@audit what's the purpose of this line?
296:
297:        // Require tokens is zero or amount is also zero
298:        if (redeemTokens == 0 && redeemAmount > 0) {
299:            revert("redeemTokens zero");
300:        }
301:    }
``` 
Line 294 and line 295 of the `redeemVerify()` function above are totally unnecessary as they have no effect on the execution of the function you should either remove them completly or put them in comments

- https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L366-#L384
```solidity
366:    function repayBorrowAllowed(
367:        address mToken,
368:        address payer,
369:        address borrower,
370:        uint repayAmount) override external returns (uint) {
371:        // Shh - currently unused
372:        payer;      //@audit what is the purpose of this line
373:        borrower;      //@audit what is the purpose of this line
374:        repayAmount;      //@audit what is the purpose of this line
375:
376:        if (!markets[mToken].isListed) {
377:            return uint(Error.MARKET_NOT_LISTED);
378:        }
379:
380:        // Keep the flywheel moving
381:        updateAndDistributeBorrowerRewardsForToken(mToken, borrower);
382:
383:        return uint(Error.NO_ERROR);
384:    }
```
Line 372, line 373 and line 374 of the `repayBorrowAllowed()` function above are totally unnecessary as they have no effect on the execution of the function you should either remove them completly or put them in comments
