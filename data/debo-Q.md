## [L-01] SWC-123 Requirement violation.

## Description
```txt
A requirement was violated in a nested call and the call was reverted as a result. 
Keywords: comptroller
```

## POC
Vulnerable Code
```sol
// https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L57-L111
    Comptroller public comptroller; /// we can't make this immutable because we are using proxies
...
    function initialize(
        address _comptroller,
        address _pauseGuardian
    ) external initializer {
        // Sanity check the params
        require(
            _comptroller != address(0),
            "Comptroller can't be the 0 address!"
        );
        require(
            _pauseGuardian != address(0),
            "Pause Guardian can't be the 0 address!"
        );

        comptroller = Comptroller(payable(_comptroller));

    }

// https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L281-L291
        for (uint256 index = 0; index < configs.length; index++) {
            MarketEmissionConfig storage emissionConfig = configs[index];

            // Calculate our new global supply index
            IndexUpdate memory supplyUpdate = calculateNewIndex(
                emissionConfig.config.supplyEmissionsPerSec,
                emissionConfig.config.supplyGlobalTimestamp,
                emissionConfig.config.supplyGlobalIndex,
                emissionConfig.config.endTime,
                calcData.marketData.totalMTokens
            );
```

## Remediation
```txt
Make sure valid inputs are provided to the nested call (for instance, via passed arguments).
```