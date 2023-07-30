# QA Report
## Summary
Total 07 Low and 03 Non-Critical
### Low Risk Issues
- [L-01] CloseFactor unbounded
- [L-02] `utilizationRate()` could DOS in both `WhitePaperInterestRateModel.sol` and `JumpRateModel.sol`
- [L-03] Lack of check for stale values in `Comptroller.sol#_setCollateralFactor()`, `Comptroller.sol#_setLiquidationIncentive` and  `Comptroller.sol#__setCloseFactor`
- [L-04] Uncommented fields in a struct
- [L-05] `MToken.sol#transfer()` and `MToken.sol#transferFrom()` missing `accrueInterest()` first
- [L-06] The `redeemTokens` calculation for `redeemFresh()` is recommended to use `round up`
- [L-07] `MErc20.sol#sweepToken()` The underlying address is invalid for a token with a double address

## [L-01] CloseFactor unbounded
### Description:
In `Comptroller.sol`, it is mentioned that `closeFactorMantissa` should be greater than `closeFactorMinMantissa` and less than `closeFactorMaxMantissa`. But in `_setCloseFactor`, these are not checked, meaning `closeFactorMantissa` can be set to a value outside the boundaries defined by the protocol.
### PROOF OF CONCEPT:
[File: 2023-07-moonwell/src/core/Comptroller.sol](https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol)
```
L61-L65
    // closeFactorMantissa must be strictly greater than this value
    uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05


    // closeFactorMantissa must not exceed this value
    uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9
```
```
L689-L698
    function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {
        // Check caller is admin
        require(msg.sender == admin, "only admin can set close factor");


        uint oldCloseFactorMantissa = closeFactorMantissa;
        closeFactorMantissa = newCloseFactorMantissa;
        emit NewCloseFactor(oldCloseFactorMantissa, closeFactorMantissa);


        return uint(Error.NO_ERROR);
    }
```
### Recommendation:
Add checks in `_setCloseFactor` to ensure `closeFactorMantissa` is greater than `closeFactorMinMantissa` and less than `closeFactorMaxMantissa`.
Eg:
```

    function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {
        // Check caller is admin
        require(msg.sender == admin, "only admin can set close factor");
+       require(closeFactorMaxMantissa >= newCloseFactorMantissa, "Close factor greater than maximum close factor");
+       require(closeFactorMinMantissa <= newCloseFactorMantissa, "Close factor smaller than minimum close factor");
        uint oldCloseFactorMantissa = closeFactorMantissa;
        closeFactorMantissa = newCloseFactorMantissa;
        emit NewCloseFactor(oldCloseFactorMantissa, closeFactorMantissa);


        return uint(Error.NO_ERROR);
    }

```

## [L-02] `utilizationRate()` could DOS in both `WhitePaperInterestRateModel.sol` and `JumpRateModel.sol`
### Description:
DOS in `utilizationRate()` in an edge case (i.e when `(cash + borrows - reserves) == 0)`
Take a look at [WhitePaperInterestRateModel.sol#utilizationRate()](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/WhitePaperInterestRateModel.sol#L51-L58) and [JumpRateModel.sol#utilizationRate()](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/IRModels/JumpRateModel.sol#L68-L75)
```
    function utilizationRate(uint cash, uint borrows, uint reserves) public pure returns (uint) {
        // Utilization rate is 0 when there are no borrows
        if (borrows == 0) {
            return 0;
        }


        return borrows.mul(1e18).div(cash.add(borrows).sub(reserves));
    }
```
If the denominator in any case equates 0, i.e `(cash + borrows - reserves) == 0` the execution reverts due to a division by zero.
### Recommendation:
The calculation of the denominator in this case, `cash + borrows - reserves` should first be checked not to equate to zero before attempting the division.
The function could be changed to
```
    function utilizationRate(
        uint256 cash,
        uint256 borrows,
        uint256 reserves
    ) public pure returns (uint256) {
        // Utilization rate is 0 when there are no borrows
        if (borrows == 0) {
            return 0;
        }

        if ((cash + borrows - reserves) != 0){
        return borrows.mul(1e18).div(cash.add(borrows).sub(reserves));
    }
return 0
    }
```

## [L-03] Lack of check for stale values in `Comptroller.sol#_setCollateralFactor()`, `Comptroller.sol#_setLiquidationIncentive` and  `Comptroller.sol#__setCloseFactor`
### Description:
`Comptroller.sol#_setCollateralFactor()` use to sets the collateralFactor for a market. So `newCollateralFactorMantissa` and `oldCollateralFactorMantissa`
### Recommendation:
[Comptroller.sol#_setCollateralFactor()](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L707-L740)
```
    function _setCollateralFactor(MToken mToken, uint newCollateralFactorMantissa) external returns (uint) {
        // Check caller is admin
        if (msg.sender != admin) {
            return fail(Error.UNAUTHORIZED, FailureInfo.SET_COLLATERAL_FACTOR_OWNER_CHECK);
        }


        // Verify market is listed
        Market storage market = markets[address(mToken)];
        if (!market.isListed) {
            return fail(Error.MARKET_NOT_LISTED, FailureInfo.SET_COLLATERAL_FACTOR_NO_EXISTS);
        }


        Exp memory newCollateralFactorExp = Exp({mantissa: newCollateralFactorMantissa});


        // Check collateral factor <= 0.9
        Exp memory highLimit = Exp({mantissa: collateralFactorMaxMantissa});
        if (lessThanExp(highLimit, newCollateralFactorExp)) {
            return fail(Error.INVALID_COLLATERAL_FACTOR, FailureInfo.SET_COLLATERAL_FACTOR_VALIDATION);
        }


        // If collateral factor != 0, fail if price == 0
        if (newCollateralFactorMantissa != 0 && oracle.getUnderlyingPrice(mToken) == 0) {
            return fail(Error.PRICE_ERROR, FailureInfo.SET_COLLATERAL_FACTOR_WITHOUT_PRICE);
        }


        // Set market's collateral factor to new collateral factor, remember old value
        uint oldCollateralFactorMantissa = market.collateralFactorMantissa;
+       if (newCollateralFactorMantissa != oldCollateralFactorMantissa)
        market.collateralFactorMantissa = newCollateralFactorMantissa;


        // Emit event with asset, old collateral factor, and new collateral factor
        emit NewCollateralFactor(mToken, oldCollateralFactorMantissa, newCollateralFactorMantissa);


        return uint(Error.NO_ERROR);
    }
```
Add `if (newCollateralFactorMantissa != oldCollateralFactorMantissa)` under [Line 733](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L733)
Similar in [Comptroller.sol#_setLiquidationIncentive](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L748-L764) and  [Comptroller.sol#_setCloseFactor](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L689-L698)

## [L-04] Uncommented fields in a struct
Consider adding comments for all the fields in a struct to improve the readability of the codebase.
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L497-L508
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L71-L75
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L77-L80


## [L-05] `MToken.sol#transfer()` and `MToken.sol#transferFrom()` missing `accrueInterest()` first
When the user performs `transfer()` and `transferFrom()`, it will check if there is enough collateral. However, the current transfer is not executed first `accrueInterest()`, so the amount owed by the user may be less than the actual amount owed, resulting in a transfer that would otherwise be impossible.

It is recommended to execute `accrueInterest()` first when transferring
```
File:
    function transfer(address dst, uint256 amount) override external nonReentrant returns (bool) {
+++     accrueInterest();
        return transferTokens(msg.sender, msg.sender, dst, amount) == uint(Error.NO_ERROR);
    }
...SKIP...

    function transferFrom(address src, address dst, uint256 amount) override external nonReentrant returns (bool) {
+++     accrueInterest();
        return transferTokens(msg.sender, src, dst, amount) == uint(Error.NO_ERROR);
    }
```


## [L-06] The `redeemTokens` calculation for `redeemFresh()` is recommended to use `round up`
According to the ERC4626 standard, for protocol security, the tokens fetched by the asset calculation should be round up  when redeeming
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MToken.sol#L659
```
                (vars.mathErr, vars.redeemTokens) = divScalarByExpTruncate(redeemAmountIn, Exp({mantissa: vars.exchangeRateMantissa})); // @audit should be round up
```

## [L-07] `MErc20.sol#sweepToken()` The underlying address is invalid for a token with a double address
[MErc20.sol#sweepToken()](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MErc20.sol#L145-L153) will restrict the transfer of `underlying`. But currently it is a comparison address, this restriction will be invalid for some tokens with proxy address, we suggest to check wether the balance of `underlying` has changed.
https://github.com/d-xo/weird-erc20/#multiple-token-addresses
```
FIle: 2023-07-moonwell/src/core/MErc20.sol
     * @notice A public function to sweep accidental ERC-20 transfers to this contract. Tokens are sent to admin (timelock)
     * @param token The address of the ERC-20 token to sweep
     */
    function sweepToken(EIP20NonStandardInterface token) override external {
        require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");
        require(address(token) != underlying, "MErc20::sweepToken: can not sweep underlying token");
        uint256 balance = token.balanceOf(address(this));
+      uint256 underlyingBalance = underlying.balanceOf(address(this));
        token.transfer(admin, balance);
+       require(underlying.balanceOf(address(this)) == underlyingBalance,"can not sweep underlying token");
+       emit SweepToken(address(token));
    }
```
### Non-Critical Issues
- [N-01] SOLIDITY VERSION 0.8.19 CAN BE USED
- [N-02] Include the project name and development team information in the contract to increase the popularity and trust of users in the project
- [N-03] Use SMTChecker
## [N-01] SOLIDITY VERSION 0.8.19 CAN BE USED
Most contracts are using 0.8.15 or lower. Consider updating to the latest version 0.8.19 to ensure the compiler contains the latest security fixes.
Using the more updated version of Solidity can enhance security. As described in https://github.com/ethereum/solidity/releases, Version 0.8.19 is the latest version of Solidity, which "contains a fix for a long-standing bug that can result in code that is only used in creation code to also be included in runtime bytecode". To be more secured and more future-proofed, please consider using Version 0.8.19 for the following contracts.
https://github.com/ethereum-optimism/optimism/blob/382d38b7d45bcbf73cb5e1e3f28cbd45d24e8a59/packages/contracts-bedrock/contracts/L1/OptimismPortal.sol#L2

## [N-02] Include the project name and development team information in the contract to increase the popularity and trust of users in the project
### Recommendation: Use form like FraxFinance project
https://github.com/FraxFinance/frxETH-public/blob/7f7731dbc93154131aba6e741b6116da05b25662/src/sfrxETH.sol#L4-L24

## [N-03] Use SMTChecker
The highest tier of smart contract behavior assurance is formal mathematical verification. All assertions that are made are guaranteed to be true across all inputs → The quality of your asserts is the quality of your verification

https://twitter.com/0xOwenThurm/status/1614359896350425088?t=dbG9gHFigBX85Rv29lOjIQ&s=19
