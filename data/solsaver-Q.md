## Unnecessary `mul` and `div` with the same value in `JumpRateModel.sol`

### Instance 1:

```
File: src/core/IRModels/JumpRateModel.sol

53:        baseRatePerTimestamp = baseRatePerYear.mul(1e18).div(timestampsPerYear).div(1e18);
54:        multiplierPerTimestamp = multiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
55:        jumpMultiplierPerTimestamp = jumpMultiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol#L53-L55

### Suggested Update for Instance 1:

The above can be updated to the following, and all the unit tests still pass.

```
        uint baseRatePerTimestamp = baseRatePerYear.div(timestampsPerYear);
        uint multiplierPerTimestamp = multiplierPerYear.div(timestampsPerYear);
        uint jumpMultiplierPerTimestamp = jumpMultiplierPerYear.div(timestampsPerYear);
```
### Instance 2:

```
File: src/core/IRModels/WhitePaperInterestRateModel.sol

38:        baseRatePerTimestamp = baseRatePerYear.mul(1e18).div(timestampsPerYear).div(1e18);
39:        multiplierPerTimestamp = multiplierPerYear.mul(1e18).div(timestampsPerYear).div(1e18);
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/WhitePaperInterestRateModel.sol#L38-L39

### Suggested Update for Instance 2:

The above can be updated to the following, and all the unit tests still pass.

```
        baseRatePerTimestamp = baseRatePerYear.div(timestampsPerYear);
        multiplierPerTimestamp = multiplierPerYear.div(timestampsPerYear);
```

## Rewrite `timestampsPerYear` as its individual components

```
File: src/core/IRModels/WhitePaperInterestRateModel.sol

20:    uint public constant timestampsPerYear = 31536000;
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/WhitePaperInterestRateModel.sol#L20

### Suggested Update

The above can be updated to the following, just as it was done here https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol#L20

```
    timestampsPerYear = 60 * 60 * 24 * 365;
```

## Incorect `@param deadline` documentation

### Instance 1:

```
File: src/core/MErc20.sol

55:     * @param deadline The amount of the underlying asset to supply
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20.sol#L55

### Instance 2:

```
File: src/core/MErc20Delegator.sol

55:     * @param deadline The amount of the underlying asset to supply
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol#L91

### Suggested Update

For all the above case, it should probably be:

```
    * @param deadline The deadline when the transaction becomes invalid
```

## Mismatching error message for condition in `require` statement

## Instance 1

The message states that the "supply reward speed cannot be **higher** than the emission cap".
But the condition ensures that that the "supply reward speed is **less** than the emission cap, that is, it should not be **higher or equal** to the emission cap"

```
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

399:        require(
400:            _supplyEmissionPerSec < emissionCap,
401:            "Cannot set a supply reward speed higher than the emission cap!"
402:        );
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L399-L402

### Suggested Update for Instance 1:

The above can be updated to the following:

```
        require(
            _supplyEmissionPerSec < emissionCap,
            "Cannot set supply reward that breaches the emission cap!"
        );
```


## Instance 2

The message states that the "borrow speed cannot be **higher** than the emission cap".
But the condition ensures that that the "borrow speed is **less** than the emission cap, that is, it should not be **higher or equal** to the emission cap"

```
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

403:        require(
404:            _borrowEmissionsPerSec < emissionCap,
405:            "Cannot set a borrow reward speed higher than the emission cap!"
406:        );
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L403-L406

### Suggested Update for Instance 2:

The above can be updated to the following:

```
        require(
            _supplyEmissionPerSec < emissionCap,
            "Cannot set borrow reward that breaches the emission cap!"
        );
```

## Instance 3

The message states that the "supply reward speed cannot be **higher** than the emission cap".
But the condition ensures that that the "new supply reward speed is **less** than the emission cap, that is, it should not be **higher or equal** to the emission cap"

```
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

678:        require(
679:            _newSupplySpeed < emissionCap,
680:            "Cannot set a supply reward speed higher than the emission cap!"
681:        );
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L678-L681

### Suggested Update for Instance 3:

The above can be updated to the following:

```
        require(
            _newSupplySpeed < emissionCap,
            "Cannot set a supply reward speed that breaches the emission cap!"
        );
```

## Instance 4

The message states that the "borrow reward speed cannot be **higher** than the emission cap".
But the condition ensures that that the "new borrow reward speed is **less** than the emission cap, that is, it should not be **higher or equal** to the emission cap"

```
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol

722:        require(
723:            _newBorrowSpeed < emissionCap,
724:            "Cannot set a borrow reward speed higher than the emission cap!"
725:        );
```

Link: https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L722-L725

### Suggested Update for Instance 4:

The above can be updated to the following:

```
        require(
            _newSupplySpeed < emissionCap,
            "Cannot set a borrow reward speed that breaches the emission cap!"
        );
```