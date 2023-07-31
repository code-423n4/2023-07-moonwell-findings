# QA Report

## Summary

| id | title |
| --- | --- |
| [L-01](#l-01-guardian-can-fast-track-every-proposal) | `guardian` can fast track every proposal |
| [L-02](#l-02-temporalgovernor-can-get-the-same-address-on-different-chains) | `TemporalGovernor` can get the same address on different chains |
| [L-03](#l-03-temporalgovernorgrantguardianspause-can-be-called-after-guardian-is-revoked) | `TemporalGovernor::grantGuardiansPause` can be called after `guardian` is revoked |
| [L-04](#l-04-no-cap-on-how-many-emissionconfigs-there-can-be-for-a-market) | No cap on how many `emissionConfig`s there can be for a market |
| [L-05](#l-05-neither-_setmarketborrowcaps-or-_setmarketsupplycaps-checks-if-market-is-listed) | L-05 Neither `_setMarketBorrowCaps` or `_setMarketSupplyCaps` checks if market is listed |
| [L-06](#l-06-lack-of-tests-for-supplycap) | Lack of tests for `supplyCap` |
| [S-01](#s-01-chainlinkcompositeoraclegetderivedpricethreeoracles-is-very-specialized) | `ChainlinkCompositeOracle::getDerivedPriceThreeOracles` is very specialized |
| [R-01](#r-01-multirewarddistributorcalculatenewindex-parameter-_currenttimestamp-is-confusing) | `MultiRewardDistributor::calculateNewIndex` parameter `_currentTimestamp` is confusing |
| [R-02](#r-02-unnecessary-_amount-0-check) | unnecessary `_amount> 0` check |
| [R-03](#r-03-multirewarddistributor-indexupdate-struct-is-unnecessary) | `MultiRewardDistributor`: `IndexUpdate` struct is unnecessary |
| [R-04](#r-04-parameter-_mtokendata-for-multirewarddistributorcalculateborrowrewardsforuser-is-unnecessary) | parameter `_mTokenData` for `MultiRewardDistributor::calculateBorrowRewardsForUser` is unnecessary |
| [NC-01](#nc-01-wrong-grammatical-number-in-grantguardianspause) | wrong grammatical number in `grantGuardiansPause` |
| [NC-02](#nc-02-erroneous-docs) | erroneous docs |
| [NC-03](#nc-03-weird-word-describing-the-future) | weird word describing the future: |
| [NC-04](#nc-04-forgotten-ctoken-references) | forgotten `cToken` references |
| [NC-05](#nc-05-incorrect-comments) | incorrect comments |
| [NC-06](#nc-06-comment-slash-off-by-one) | comment slash off by one |

## Low

### L-01 `guardian` can fast track every proposal
In `TemporalGovernor` the `guardian` has a special permission that they can `fastTrackProposalExecution` which will [bypass the normal `queueProposal` flow](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L354-L367):

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L261-L268
```solidity
File: core/Governance/TemporalGovernor.sol

261:    /// @notice Allow the guardian to process a VAA when the
262:    /// Temporal Governor is paused this is only for use during
263:    /// periods of emergency when the governance on moonbeam is
264:    /// compromised and we need to stop additional proposals from going through.
265:    /// @param VAA The signed Verified Action Approval to process
266:    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
267:        _executeProposal(VAA, true); /// override timestamp checks and execute
268:    }
```

This as the comment suggest should only be done in emergencies.

However there is nothing indicating that the `VAA` in question is supposed to be fast tracked. Hence a `guardian` can always fast track any `VAA`s that they please, bypassing the `queueProposal` functionality.

#### Recommendation
Consider adding another parameter to the `payload` indicating if the `VAA` is intended to be fast tracked or not.

### L-02 `TemporalGovernor` can get the same address on different chains
In `mip00.sol` the deploy and configuration proceedure for the `TemporalGovernor` is detailed:

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L103-L109
```solidity
File: test/proposals/mips/mip00.sol

103:            /// this will be the governor for all the contracts
104:            TemporalGovernor governor = new TemporalGovernor(
105:                addresses.getAddress("WORMHOLE_CORE"),
106:                proposalDelay,
107:                permissionlessUnpauseTime,
108:                trustedSenders
109:            );
```

`new` will invoke the `CREATE` opcode. This only relies on the `msg.sender` and `nonce`. Since the `deploy` call executes a certain amount of calls when it executes the deploy of `TemporalGovernor` it will have certain `nonce`. Hence the address of `TemporalGovernor` will be dependent on the account that executes it. If this is run by a new account the address of `TemporalGovernor` can be the same if it is executed by the same new account on another chain.

This is important because if two `TemporalGovernor` contracts on different chains gets the same address they can execute each others proposals:

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L320-L322
```solidity
File: core/Governance/TemporalGovernor.sol

320:        // Very important to check to make sure that the VAA we're processing is specifically designed
321:        // to be sent to this contract
322:        require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");
```

#### Recommendation
Pay attention to which accounts and how the execution of the deploy script is done. Perhaps use `CREATE2` with a random `salt` to deploy `TemporalGovernor` to guarantee it gets a different address on each chain.

### L-03 `TemporalGovernor::grantGuardiansPause` can be called after `guardian` is revoked

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L187-L198
```solidity
File: Governance/TemporalGovernor.sol

187:    /// @notice grant the guardians the pause ability
188:    function grantGuardiansPause() external {
189:        require(
190:            msg.sender == address(this),
191:            "TemporalGovernor: Only this contract can update grant guardian pause"
192:        );
193:
194:        guardianPauseAllowed = true;
195:        lastPauseTime = 0;
196:
197:        emit GuardianPauseGranted(block.timestamp);
198:    }
```

This doesn't do any thing bad other than set `guardianPauseAllowed=true`, which is set to false in [`revokeGuardian`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L213).

#### Recommendation
Consider only allowing calls to `grantGuardiansPause` when `guardian != address(0)`.

### L-04 No cap on how many `emissionConfig`s there can be for a market

Too many `emissionConfig`s could lead to out of gas errors when updating reward indexes. Since every operation on an `mToken` triggers this the whole market would be DoSed if too many `emissionConfig`s would be added.

#### Recommendation
Consider adding a way to remove an `emissionConfig`

### L-05 Neither `_setMarketBorrowCaps` or `_setMarketSupplyCaps` checks if market is listed

When adding a cap on borrow/supply `admin` or `borrow/supplyCapGuardian` can call [`_setMarketBorrowCaps`])(https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L807-L819) or [`_setMarketSupplyCaps`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L844-L856) respectively.

However in none of the there is any check that the market (`MToken`) for which they add the supply/borrow cap is actually listed. Hence `admin` or `guardian` could by mistake send the wrong address and then think that they configured the cap for that market when they didn't.

#### Recommendation
Add a check that the `MToken` is listed when setting borrow/supply caps.

### L-06 Lack of tests for `supplyCap`

M̀oonwell introduces a new feature to the `Comptroller`: `supplyCap` per market. This limits how much can be minted to that market. There is however no tests for this new feature. Test guarantee that the feature works as expected and will continue to work as expected when doing changes to the code. As such they are a core guarantee for code and protocol safety.

####
Consider adding a few tests for the `supplyCap` feature

## Suggestions

### S-01 `ChainlinkCompositeOracle::getDerivedPriceThreeOracles` is very specialized

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol#L157-L159
```solidity
File: Oracles/ChainlinkCompositeOracle.sol

157:        return
158:            ((firstPrice * secondPrice * thirdPrice) / scalingFactor)
159:                .toUint256();
```

Here the three prices for the oracles are multiplied together. This requires a setup of prices like this:
First one is stated to be `ETH`/`USD`

so there would have to be oracles:

`ETH`/`USD`, `A`/`ETH`, `B`/`A` to get the price `B`/`USD`

This is very limited which chainlink prices support "chaining" like this, the one used in integration test seems to one of the few:

`ETH`/`USD`, `stETH`/`ETH`, `wstETH`/`stETH` on Arbitrum.

#### Recommendations
Consider adding the ability to either multiply or divide by the last one, as that would give greater flexibility in which feeds can be used.


## Refactor

### R-01 `MultiRewardDistributor::calculateNewIndex` parameter `_currentTimestamp` is confusing

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L906

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L914
```solidity
File: MultiRewardDistributor/MultiRewardDistributor.sol

906:     * @param _currentTimestamp The current index timestamp

914:        uint32 _currentTimestamp,
```

`_currentTimestamp` implies _now_ when it is however the last index timestamp. Consider renaming it to `lastIndexTimestamp`.


### R-02 Unnecessary `_amount> 0` check

In `sendReward`, the function sort circuit if `_amount` to be sent is `0`, later however it is checked that this `amount` is `>0` which there is no way it cannot be:

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1219-L1222

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1235
```solidity
File: MultiRewardDistributor/MultiRewardDistributor.sol

1219:        // Short circuit if we don't have anything to send out
1220:        if (_amount == 0) {
1221:            return _amount;
1222:        }

			 // @audit due to check above there is no way _amount cannot be >0
1235:        if (_amount > 0 && _amount <= currentTokenHoldings) {
```

Consider removing the `_amount > 0` check since it is always true.

### R-03 `MultiRewardDistributor`: `IndexUpdate` struct is unnecessary

`IndexUpdate` is used to return the result of `calculateNewIndex`.

However in `calculateNewIndex` the same value for `IndexUpdate.newTimestamp` is always used, `block.timestamp`:

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L949-L953

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L967
```solidity

949:            return
950:                IndexUpdate({
951:                    newIndex: _currentIndex,
952:                    newTimestamp: blockTimestamp
953:                });

967:        return IndexUpdate({newIndex: newIndex, newTimestamp: blockTimestamp});
```

Hence, `calculateNewIndex` could simply return `newIndex` (`uin224`). Then in `updateMarketSupplyIndexInternal` and `updateMarketBorrowIndexInternal` the `supply/borrowGlobalTimestamp` can be set directly to `block.timestamp`.

### R-04 parameter `_mTokenData` for `MultiRewardDistributor::calculateBorrowRewardsForUser` is unnecessary

The struct `MTokenData` has two fields: [`mTokenBalance` and `borrowBalanceStored`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributorCommon.sol#L50-L53). However only [one of them is used](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L887) in `calculateBorrowRewardsForUser`.

Therefore it is confusing that the whole struct is passed to the function as that implies that both fields would be used.

Consider just passing `borrowBalanceStored` directly instead of the struct.

## Non critical / Informational

### NC-01 wrong grammatical number in `grantGuardiansPause`

`grantGuardiansPause` grants `guardian` pause. Since there is only one `guardian` the name should be singular not plural: **`grantGuardianPause`**.

### NC-02 erroneous docs

https://github.com/code-423n4/2023-07-moonwell/blob/main/docs/TEMPORALGOVERNOR.md
> Guardian Pause: The guardian has the ability to pause the contract temporarily. When the guardian pauses the contract, all proposal executions are halted. The contract can only be unpaused by a governance proposal after a specified time delay.

This is not true. A governance proposal cannot unpause the `TemporalGovernor`. Unpause can only be performed by `governor` through [`togglePause`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L274-L290) or by the [`permissionlessUnpause`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L242-L259) after a delay. The only way governance could unpause is by calling [`revokeGuardian`](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol#L205-L221) which is final. This would require the `guardian` to execute it though since only `guardian` is allowed to execute commands while the contract is paused.

### NC-03 weird word describing the future

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L788-L792
```solidity
File: MultiRewardDistributor/MultiRewardDistributor.sol

788:        // Must be older than our existing end time AND the current block
789:        require(
790:            _newEndTime > currentEndTime,
791:            "_newEndTime MUST be > currentEndTime"
792:        );
```

`older` here is confusing. Either say `Must be further in the future` or simply `larger`


### NC-04 forgotten `cToken` references

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L842

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L847

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L881
```solidity
File: MultiRewardDistributor/MultiRewardDistributor.sol

842:        // Calculate change in the cumulative sum of the reward per cToken accrued

843:        // Calculate reward accrued: cTokenAmount * accruedPerCToken

844:        // Calculate change in the cumulative sum of the reward per cToken accrued
```

### NC-05 incorrect comments

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L835-L840

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L874-L879
```solidity
File: MultiRewardDistributor/MultiRewardDistributor.sol

835:        // If our user's index isn't set yet, set to the current global supply index
836:        if (
837:            userSupplyIndex == 0 && _globalSupplyIndex >= initialIndexConstant
838:        ) {
839:            userSupplyIndex = initialIndexConstant; //_globalSupplyIndex;
840:        }

...

874:        // If our user's index isn't set yet, set to the current global borrow index
875:        if (
876:            userBorrowIndex == 0 && _globalBorrowIndex >= initialIndexConstant
877:        ) {
878:            userBorrowIndex = initialIndexConstant; //userBorrowIndex = _globalBorrowIndex;
879:        }
```

If the index is not yet set it's set to `initialIndexConstant` not the global index as the comment suggests

### NC-06 comment slash off by one

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L984-L988
```solidity

   /** <-- slash only 3 whitespaces in
     * @notice Call out to the reward distributor to update its borrow index and this user's index too
     * @param mToken The market to synchronize indexes for
     * @param borrower The borrower to whom rewards are going
     */
```

The slash is only 3 not 4 whitespaces in.