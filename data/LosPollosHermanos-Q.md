# [LOW 1] `MultiRewardDistributor._rescueFounds` is an easy way to rug pull and could make the project less thrustworthy.
## Description
`MultiRewardDistributor._rescueFunds` allows `admin` to withdraw any amount of funds at any time. This might repell potential investors.
## Mitigation
Consider adding a third part multi-sig which receives funds rather then the `admin`.
## Affected Code
[`_rescueFunds` function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L471)

# [LOW 2] `MultiRewardDistributor.sendReward` could send remainings even if it does not have enough founds
## Description
The `MultiRewardDistributor.sendReward` function could process remaining rewards even if `_amount > currentTokenHoldings`. This approach prevents contract from freezing funds and could be benefitial for users, since they would get a part of their reward immidiately.
## Mitigation
In `MultiRewardDistributor.sendReward` in [else statement](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1239) consider doing `token.safeTransfer(_user, token.balanceOf(address(this))` and return `_amount - previousBalance` where `previousBalance` is `token.balanceOf(address(this))` before `safeTransfer` 
## Affected Code 
[`sendReward` function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1214)

# [LOW 3] `Comptroller._setMarketBorrowCaps` and `Comptroller.setMarketSupplyCaps` could accidentaly be called with a `mtoken` without a market.
# Descritpion
`Comptroller._setMarketBorrowCaps` and `Comptroller.setMarketSupplyCaps` do not make any check that a `mtoken` has a registered market.
# Mitigation
In both functions check that markets for supplied `mtokens` exist by requireing `markets[mTokens[i]].isListed` for each token.
# Affected Code
[`_setMarketBorrowCaps`](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L807C14-L807C33)
[`_setMarketSupplyCaps`](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L844)

# [LOW 4] `Comptroller._rescueFunds` does not use `safeTransfer`
## Description
Using `safeTransfer` instead of `transfer` is considered a better security practise. Even thought `_rescueFounds` is used only in case of emergency it is advised to use `safeTransfer`
## Mitigation 
Replace `transfer` with `safeTransfer` in `Comptroller._rescueFunds`
## Affected Code
[Instance 1](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L965)
[Instance 2](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L967)

# [LOW 5] Adding too many `mTokens` to a market can cause transactions to fail
## Description
Adding too many `mTokens` to a market can cause a unbounded for loop which will force transactiosn to fail due to high gas usage.
## Mitigation
Add a limit for the number of `mTokens` supported.
## Affected Code

# [LOW 6] Incorrect `_endTime` check in `MultiRewardDistributor._addEmissionConfig`
## Description
`MultiRewardDistributor._addEmissionConfig()` checks that `_endTime` is in future by asserting that `_endTime > block.timestamp + 1`. This assertion should be modified to `_endTime > block.timestamp`.
## Mitigation
Modify a require statement in `MultiRewardDistributor._addEmissionConfig()` from `_endTime > block.timestamp + 1` to `_endTime > block.timestamp`
## Affected Code
[Incorrect check](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L410)

# [LOW 7] `marketSupplySpeed <= emissionCap` invariant can be broken.
## Description
`emissionCap` in `MultiRewardDistrubitor` can be decreased and therefore break the invariant of `marketSupplySpeed <= emissionCap`
## Mitigation
Add a line in `MultiRewardDistributor.calculateNewIndex()` so that if supply speed is bigger than no reawrds are accrued.
## Affected Code 
[`calculateNewIndex` function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L912)

# [NC 1] `MultiRewardDistributor.sendReward` is inconsistent with the project's naming convention
## Description
In this project, internal functions' names end with `Internal` (eg. `disburseBorrowerRewardsInternal`, `updateMarketBorrowIndexInternal`). `sendReward` is an internal function, but does not match this convention.
## Mitigation
Consider renaming `sendReward` to `sendRewardInternal`
## Affected Code
[`sendReward` function](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1214)

# [NC 2] Common logic of `JumpRateModel` and `WhitePaperIntrestRateModel` could be extracted
## Description
WhitePaperIntrestRateModel & JumpRateModel vary only at `getBorrowRate` function. 
## Mitigation
Consider moving common logic to another contract and inherit from it.
## Affected Code
[`WhitePaperIntrestRateModel`](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L12)
[`JumpRateModel`](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/JumpRateModel.sol#L12)

# [NC 3] Checking enums against integers in `Comptroller` is missleading.
## Description
Assertions made with enums and integers make code less readable.
## Mitigation
Consider changing all instances of `allowed != 0` to `allowed != Error.NO_ERROR`
## Affected Code
[Instance 1](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L166)
[Instance 2](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L252)

# [NC 4] `ERROR.NO_ERROR` is missleading
## Description
Name `ERROR` makes reader think that the code is not doing something that it should; however, `ERROR.NO_ERROR` is used when the code behaves properly. 
At first `ERROR.NO_ERROR` resembles any error that does not have a corresponding enum value yet.
## Mitigation
Consider renaming `ERROR` to `STATUS`, and begin any error value with `ERROR_`
## Affected Code
[ERROR enum](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/ErrorReporter.sol#L5)

# [NC 5] `MarketRewardDistributor.calcualteNewIndex()` will fail in the future
## Description
`MarketRewardDistributor.calcualteNewIndex()` will fail after the year 2106 due to casting of `block.timestamp`
## Mitigation
Consider casting `block.timestamp` to a bigger integer type.
## Affected Code
[Bad casting](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L919)

# [NC 6] Unnecessary casting in `MarketRewardDistributor.updateMarketSupplyIndexInternal()`
## Description
`MToken` already is a `MTokenInterface` so casting is unneccessary.
## Mitigation
Remove casting to `MTokenInterface` from `MarketRewardDistributor.updateMarketSupplyIndexInternal()`
## Affected Code
[Unnecessary casting](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1006)
[MToken interfaces](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MToken.sol#L16)