# VULN 1 

## [GAS] Use != 0 instead of > 0 for unsigned integer comparison
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 543 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            assertTrue(mUsdc.balanceOf(address(governor)) > 0);


Found in line 544 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            assertTrue(mEth.balanceOf(address(governor)) > 0);


Found in line 278 at 2023-07-moonwell/src/core/Comptroller.sol:

        if (shortfall > 0) {


Found in line 298 at 2023-07-moonwell/src/core/Comptroller.sol:

        if (redeemTokens == 0 && redeemAmount > 0) {


Found in line 348 at 2023-07-moonwell/src/core/Comptroller.sol:

        if (shortfall > 0) {


Found in line 37 at 2023-07-moonwell/src/core/MToken.sol:

        require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");


Found in line 626 at 2023-07-moonwell/src/core/MToken.sol:

        /* If redeemTokensIn > 0: */


Found in line 627 at 2023-07-moonwell/src/core/MToken.sol:

        if (redeemTokensIn > 0) {


Found in line 190 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        bool valid = price > 0 && answeredInRound == roundId;


Found in line 87 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        if (decimalDelta > 0) {


Found in line 102 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        require(answer > 0, "Chainlink price cannot be lower than 0");


Found in line 108 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        if (decimalDelta > 0) {


Found in line 958 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        Double memory ratio = _denominator > 0


Found in line 1235 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        if (_amount > 0 && _amount <= currentTokenHoldings) {

------------------------------------------------------------------------ 

### Mitigation 

Use != 0 instead of > 0 for unsigned integer comparison.










# VULN 2 

## [GAS] ++i/i++ should be unchecked{++i}/unchecked{i++} and ++i costs less gas than i++
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 170 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigsLength; i++) {


Found in line 275 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 334 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 443 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < emissionConfig.length; i++) {


Found in line 706 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 799 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                for (uint256 i = 0; i < emissionConfig.length; i++) {


Found in line 106 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < len; i++) {


Found in line 186 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < len; i++) {


Found in line 574 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < assets.length; i++) {


Found in line 795 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < allMarkets.length; i ++) {


Found in line 815 at 2023-07-moonwell/src/core/Comptroller.sol:

        for(uint i = 0; i < numMarkets; i++) {


Found in line 852 at 2023-07-moonwell/src/core/Comptroller.sol:

        for(uint i = 0; i < numMarkets; i++) {


Found in line 1031 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < mTokens.length; i++) {


Found in line 1040 at 2023-07-moonwell/src/core/Comptroller.sol:

                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {


Found in line 1048 at 2023-07-moonwell/src/core/Comptroller.sol:

                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {


Found in line 74 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 117 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < trustedSendersList.length; i++) {


Found in line 146 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 173 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 394 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        for (uint256 i = 0; i < targets.length; i++) {


Found in line 209 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 240 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < markets.length; index++) {


Found in line 281 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 419 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 983 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1009 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1053 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1112 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1163 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {

------------------------------------------------------------------------ 

### Mitigation 

++i/i++ should be unchecked{++i}/unchecked{i++} when it is not possible for them to overflow, as is the case when used in for and while-loops. Moreover ++i costs less gas than i++, especially when its used in for-loops (--i/i-- too).










# VULN 3 

## [GAS] Don’t initialize variables with default value
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 170 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigsLength; i++) {


Found in line 275 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 334 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 443 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < emissionConfig.length; i++) {


Found in line 706 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 799 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                for (uint256 i = 0; i < emissionConfig.length; i++) {


Found in line 106 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < len; i++) {


Found in line 186 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < len; i++) {


Found in line 574 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < assets.length; i++) {


Found in line 795 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < allMarkets.length; i ++) {


Found in line 815 at 2023-07-moonwell/src/core/Comptroller.sol:

        for(uint i = 0; i < numMarkets; i++) {


Found in line 852 at 2023-07-moonwell/src/core/Comptroller.sol:

        for(uint i = 0; i < numMarkets; i++) {


Found in line 1031 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < mTokens.length; i++) {


Found in line 1040 at 2023-07-moonwell/src/core/Comptroller.sol:

                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {


Found in line 1048 at 2023-07-moonwell/src/core/Comptroller.sol:

                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {


Found in line 81 at 2023-07-moonwell/src/core/MToken.sol:

        uint startingAllowance = 0;


Found in line 74 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 117 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < trustedSendersList.length; i++) {


Found in line 146 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 173 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 394 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        for (uint256 i = 0; i < targets.length; i++) {


Found in line 209 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 240 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < markets.length; index++) {


Found in line 281 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 419 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 983 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1009 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1053 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1112 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1163 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {

------------------------------------------------------------------------ 

### Mitigation 

In such cases, initializing the variables with default values would be unnecessary and can be considered a waste of gas. Additionally, initializing variables with default values can sometimes lead to unnecessary storage operations, which can increase gas costs. For example, if you have a large array of variables, initializing them all with default values can result in a lot of unnecessary storage writes, which can increase the gas costs of your contract.










# VULN 4 

## [GAS] Use Custom Errors
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 30 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

        require(msg.sender == admin, "only the admin may call _becomeImplementation");


Found in line 42 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

        require(msg.sender == admin, "only the admin may call _resignImplementation");


Found in line 158 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(oErr == 0, "exitMarket: getAccountSnapshot failed"); // semi-opaque error code


Found in line 217 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(!mintGuardianPaused[mToken], "mint is paused");


Found in line 236 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(nextTotalSupplies < supplyCap, "market supply cap reached");


Found in line 299 at 2023-07-moonwell/src/core/Comptroller.sol:

            revert("redeemTokens zero");


Found in line 312 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(!borrowGuardianPaused[mToken], "borrow is paused");


Found in line 320 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(msg.sender == mToken, "sender must be mToken");


Found in line 341 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(nextTotalBorrows < borrowCap, "market borrow cap reached");


Found in line 441 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(!seizeGuardianPaused, "seize is paused");


Found in line 474 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(!transferGuardianPaused, "transfer is paused");


Found in line 691 at 2023-07-moonwell/src/core/Comptroller.sol:

    	require(msg.sender == admin, "only admin can set close factor");


Found in line 781 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(mToken.isMToken(), "Must be an MToken"); // Sanity check to make sure its really a MToken


Found in line 796 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(allMarkets[i] != MToken(mToken), "market already added");


Found in line 808 at 2023-07-moonwell/src/core/Comptroller.sol:

    	require(msg.sender == admin || msg.sender == borrowCapGuardian, "only admin or borrow cap guardian can set borrow caps");


Found in line 813 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");


Found in line 826 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin, "only admin can set borrow cap guardian");


Found in line 845 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin || msg.sender == supplyCapGuardian, "only admin or supply cap guardian can set supply caps");


Found in line 850 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");


Found in line 863 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin, "only admin can set supply cap guardian");


Found in line 902 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin, "Unauthorized");


Found in line 912 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");


Found in line 913 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");


Found in line 914 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin || state == true, "only admin can unpause");


Found in line 922 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");


Found in line 923 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");


Found in line 924 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin || state == true, "only admin can unpause");


Found in line 932 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");


Found in line 933 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin || state == true, "only admin can unpause");


Found in line 941 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");


Found in line 942 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin || state == true, "only admin can unpause");


Found in line 950 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == unitroller.admin(), "only unitroller admin can change brains");


Found in line 951 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(unitroller._acceptImplementation() == 0, "change not authorized");


Found in line 960 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(msg.sender == admin, "Unauthorized");


Found in line 1029 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(address(rewardDistributor) != address(0), "No reward distributor configured!");


Found in line 1035 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(markets[address(mToken)].isListed, "market must be listed");


Found in line 1077 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(_locked != 1, "ReentrancyGuard: reentrant call");


Found in line 62 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

        require(msg.sender == admin, "MErc20Delegator::_setImplementation: Caller must be admin");


Found in line 497 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

        require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");


Found in line 32 at 2023-07-moonwell/src/core/MToken.sol:

        require(msg.sender == admin, "only admin may initialize the market");


Found in line 33 at 2023-07-moonwell/src/core/MToken.sol:

        require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");


Found in line 37 at 2023-07-moonwell/src/core/MToken.sol:

        require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");


Found in line 41 at 2023-07-moonwell/src/core/MToken.sol:

        require(err == uint(Error.NO_ERROR), "setting comptroller failed");


Found in line 49 at 2023-07-moonwell/src/core/MToken.sol:

        require(err == uint(Error.NO_ERROR), "setting interest rate model failed");


Found in line 194 at 2023-07-moonwell/src/core/MToken.sol:

        require(mErr == MathError.NO_ERROR, "balance could not be calculated");


Found in line 253 at 2023-07-moonwell/src/core/MToken.sol:

        require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");


Found in line 263 at 2023-07-moonwell/src/core/MToken.sol:

        require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");


Found in line 274 at 2023-07-moonwell/src/core/MToken.sol:

        require(err == MathError.NO_ERROR, "borrowBalanceStored: borrowBalanceStoredInternal failed");


Found in line 320 at 2023-07-moonwell/src/core/MToken.sol:

        require(accrueInterest() == uint(Error.NO_ERROR), "accrue interest failed");


Found in line 331 at 2023-07-moonwell/src/core/MToken.sol:

        require(err == MathError.NO_ERROR, "exchangeRateStored: exchangeRateStoredInternal failed");


Found in line 403 at 2023-07-moonwell/src/core/MToken.sol:

        require(borrowRateMantissa <= borrowRateMaxMantissa, "borrow rate is absurdly high");


Found in line 407 at 2023-07-moonwell/src/core/MToken.sol:

        require(mathErr == MathError.NO_ERROR, "could not calculate block delta");


Found in line 537 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "MINT_EXCHANGE_CALCULATION_FAILED");


Found in line 545 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_TOTAL_SUPPLY_CALCULATION_FAILED");


Found in line 548 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED");


Found in line 616 at 2023-07-moonwell/src/core/MToken.sol:

        require(redeemTokensIn == 0 || redeemAmountIn == 0, "one of redeemTokensIn or redeemAmountIn must be zero");


Found in line 914 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED");


Found in line 917 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED");


Found in line 1013 at 2023-07-moonwell/src/core/MToken.sol:

        require(amountSeizeError == uint(Error.NO_ERROR), "LIQUIDATE_COMPTROLLER_CALCULATE_AMOUNT_SEIZE_FAILED");


Found in line 1016 at 2023-07-moonwell/src/core/MToken.sol:

        require(mTokenCollateral.balanceOf(borrower) >= seizeTokens, "LIQUIDATE_SEIZE_TOO_MUCH");


Found in line 1027 at 2023-07-moonwell/src/core/MToken.sol:

        require(seizeError == uint(Error.NO_ERROR), "token seizure failed");


Found in line 1102 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "exchange rate math error");


Found in line 1203 at 2023-07-moonwell/src/core/MToken.sol:

        require(newComptroller.isComptroller(), "marker method returned false");


Found in line 1308 at 2023-07-moonwell/src/core/MToken.sol:

        require(totalReservesNew >= totalReserves, "add reserves unexpected overflow");


Found in line 1372 at 2023-07-moonwell/src/core/MToken.sol:

        require(totalReservesNew <= totalReserves, "reduce reserves unexpected underflow");


Found in line 1426 at 2023-07-moonwell/src/core/MToken.sol:

        require(newInterestRateModel.isInterestRateModel(), "marker method returned false");


Found in line 1515 at 2023-07-moonwell/src/core/MToken.sol:

        require(_notEntered, "re-entered");


Found in line 149 at 2023-07-moonwell/src/core/MErc20.sol:

        require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");


Found in line 150 at 2023-07-moonwell/src/core/MErc20.sol:

        require(address(token) != underlying, "MErc20::sweepToken: can not sweep underlying token");


Found in line 206 at 2023-07-moonwell/src/core/MErc20.sol:

        require(success, "TOKEN_TRANSFER_IN_FAILED");


Found in line 210 at 2023-07-moonwell/src/core/MErc20.sol:

        require(balanceAfter >= balanceBefore, "TOKEN_TRANSFER_IN_OVERFLOW");


Found in line 241 at 2023-07-moonwell/src/core/MErc20.sol:

        require(success, "TOKEN_TRANSFER_OUT_FAILED");


Found in line 34 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

        require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");


Found in line 62 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

        require(success, "WETHRouter: ETH transfer failed");


Found in line 322 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");


Found in line 417 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(targets.length != 0, "TemporalGovernor: Empty proposal");


Found in line 191 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        require(valid, "CLCOracle: Oracle data is invalid");


Found in line 51 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        require(msg.sender == admin, "only admin may call");


Found in line 102 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        require(answer > 0, "Chainlink price cannot be lower than 0");


Found in line 103 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        require(updatedAt != 0, "Round is in incompleted state");


Found in line 990 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        revert("Unable to find emission token in mToken configs");

------------------------------------------------------------------------ 

### Mitigation 

Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.










# VULN 5 

## [GAS] Use calldata instead of memory for function arguments that do not get mutated
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 21 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

    function _becomeImplementation(bytes memory data) virtual override public {


Found in line 102 at 2023-07-moonwell/src/core/Comptroller.sol:

    function enterMarkets(address[] memory mTokens) override public returns (uint[] memory) {


Found in line 1015 at 2023-07-moonwell/src/core/Comptroller.sol:

    function claimReward(address holder, MToken[] memory mTokens) public {


Found in line 1028 at 2023-07-moonwell/src/core/Comptroller.sol:

    function claimReward(address[] memory holders, MToken[] memory mTokens, bool borrowers, bool suppliers) public {


Found in line 61 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {


Found in line 455 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function delegateTo(address callee, bytes memory data) internal returns (bytes memory) {


Found in line 471 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function delegateToImplementation(bytes memory data) public returns (bytes memory) {


Found in line 482 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {


Found in line 229 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function queueProposal(bytes memory VAA) external whenNotPaused {


Found in line 237 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function executeProposal(bytes memory VAA) public whenNotPaused {


Found in line 266 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {


Found in line 295 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function _queueProposal(bytes memory VAA) private {


Found in line 344 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function _executeProposal(bytes memory VAA, bool overrideDelay) private {

------------------------------------------------------------------------ 

### Mitigation 

Mark data types as calldata instead of memory where possible. This makes it so that the data is not automatically loaded into memory. If the data passed into the function does not need to be changed (like updating values in an array), it can be passed in as calldata. The one exception to this is if the argument must later be passed into another function that takes an argument that specifies memory storage.










# VULN 6 

## [GAS] Cache array length outside of loop
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 275 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 334 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 443 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            for (uint256 i = 0; i < emissionConfig.length; i++) {


Found in line 706 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                for (uint256 i = 0; i < cTokenConfigs.length; i++) {


Found in line 799 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                for (uint256 i = 0; i < emissionConfig.length; i++) {


Found in line 574 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < assets.length; i++) {


Found in line 795 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < allMarkets.length; i ++) {


Found in line 1031 at 2023-07-moonwell/src/core/Comptroller.sol:

        for (uint i = 0; i < mTokens.length; i++) {


Found in line 1040 at 2023-07-moonwell/src/core/Comptroller.sol:

                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {


Found in line 1048 at 2023-07-moonwell/src/core/Comptroller.sol:

                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {


Found in line 74 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 117 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < trustedSendersList.length; i++) {


Found in line 146 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 173 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            for (uint256 i = 0; i < _trustedSenders.length; i++) {


Found in line 394 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        for (uint256 i = 0; i < targets.length; i++) {


Found in line 209 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 240 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < markets.length; index++) {


Found in line 281 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 419 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 983 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1009 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1053 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1112 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {


Found in line 1163 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        for (uint256 index = 0; index < configs.length; index++) {

------------------------------------------------------------------------ 

### Mitigation 

If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).










# VULN 7 

## [GAS] Use assembly to check for address(0)
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 61 at 2023-07-moonwell/src/core/Unitroller.sol:

        if (msg.sender != pendingComptrollerImplementation || pendingComptrollerImplementation == address(0)) {


Found in line 111 at 2023-07-moonwell/src/core/Unitroller.sol:

        if (msg.sender != pendingAdmin || msg.sender == address(0)) {


Found in line 1170 at 2023-07-moonwell/src/core/MToken.sol:

        if (msg.sender != pendingAdmin || msg.sender == address(0)) {


Found in line 58 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        if (secondMultiplier == address(0)) {

------------------------------------------------------------------------ 

### Mitigation 

Using assembly to check for the zero address can result in significant gas savings compared to using a Solidity expression, especially if the check is performed frequently or in a loop. However, it's important to note that using assembly can make the code less readable and harder to maintain, so it should be used judiciously and with caution.










# VULN 8 

## [GAS] Usage of uints/ints smaller than 32 bytes (256 bits) incurs overhead
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 47 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

    uint8 public constant mTokenDecimals = 8; /// all mTokens have 8 decimals


Found in line 31 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

                uint8 decimals_,


Found in line 39 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

        delegateTo(implementation_, abi.encodeWithSignature("initialize(address,address,address,uint256,string,string,uint8)",


Found in line 100 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

        uint8 v, bytes32 r, bytes32 s


Found in line 104 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

                "mintWithPermit(uint256,uint256,uint8,bytes32,bytes32)",


Found in line 31 at 2023-07-moonwell/src/core/MToken.sol:

                        uint8 decimals_) public {


Found in line 29 at 2023-07-moonwell/src/core/MErc20.sol:

                        uint8 decimals_) public {


Found in line 64 at 2023-07-moonwell/src/core/MErc20.sol:

        uint8 v, bytes32 r, bytes32 s


Found in line 55 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    mapping(uint16 => EnumerableSet.Bytes32Set) private trustedSenders;


Found in line 87 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        uint16 chainId,


Found in line 97 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        uint16 chainId,


Found in line 106 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function allTrustedSenders(uint16 chainId)


Found in line 29 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

    uint8 public constant decimals = 18;


Found in line 106 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        uint8 expectedDecimals


Found in line 109 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

            expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),


Found in line 137 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        uint8 expectedDecimals


Found in line 140 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

            expectedDecimals > uint8(0) && expectedDecimals <= uint8(18),


Found in line 168 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        uint8 expectedDecimals


Found in line 170 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        (int256 price, uint8 actualDecimals) = getPriceAndDecimals(


Found in line 182 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

    ) public view returns (int256, uint8) {


Found in line 192 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        uint8 oracleDecimals = AggregatorV3Interface(oracleAddress).decimals();


Found in line 204 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        uint8 priceDecimals,


Found in line 205 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        uint8 expectedDecimals


Found in line 914 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        uint32 _currentTimestamp,


Found in line 919 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        uint32 blockTimestamp = safe32(

------------------------------------------------------------------------ 

### Mitigation 

When using elements that are smaller than 32 bytes, your contract’s gas usage may be higher. This is because the EVM operates on 32 bytes at a time. Therefore, if the element is smaller than that, the EVM must use more operations in order to reduce the size of the element from 32 bytes to the desired size. https://docs.soliditylang.org/en/v0.8.11/internals/layout_in_storage.html. Each operation involving a uint8 costs an extra 22-28 gas (depending on whether the other operand is also a variable of type uint8) as compared to ones involving uint256, due to the compiler having to clear the higher bits of the memory word before operating on the uint8, as well as the associated stack operations of doing so. Use a larger size then downcast where needed.










# VULN 9 

## [GAS] Public Functions to external
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 21 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

    function _becomeImplementation(bytes memory data) virtual override public {


Found in line 36 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

    function _resignImplementation() virtual override public {


Found in line 102 at 2023-07-moonwell/src/core/Comptroller.sol:

    function enterMarkets(address[] memory mTokens) override public returns (uint[] memory) {


Found in line 61 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {


Found in line 298 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function borrowBalanceStored(address account) override public view returns (uint) {


Found in line 307 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function exchangeRateCurrent() override public returns (uint) {


Found in line 317 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function exchangeRateStored() override public view returns (uint) {


Found in line 336 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function accrueInterest() override public returns (uint) {


Found in line 382 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {


Found in line 433 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {


Found in line 272 at 2023-07-moonwell/src/core/MToken.sol:

    function borrowBalanceStored(address account) override public view returns (uint) {


Found in line 319 at 2023-07-moonwell/src/core/MToken.sol:

    function exchangeRateCurrent() override public nonReentrant returns (uint) {


Found in line 329 at 2023-07-moonwell/src/core/MToken.sol:

    function exchangeRateStored() override public view returns (uint) {


Found in line 385 at 2023-07-moonwell/src/core/MToken.sol:

    function accrueInterest() virtual override public returns (uint) {


Found in line 1195 at 2023-07-moonwell/src/core/MToken.sol:

    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {


Found in line 1391 at 2023-07-moonwell/src/core/MToken.sol:

    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {


Found in line 84 at 2023-07-moonwell/src/core/IRModels/JumpRateModel.sol:

    function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {


Found in line 104 at 2023-07-moonwell/src/core/IRModels/JumpRateModel.sol:

    function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {


Found in line 67 at 2023-07-moonwell/src/core/IRModels/WhitePaperInterestRateModel.sol:

    function getBorrowRate(uint cash, uint borrows, uint reserves) override public view returns (uint) {


Found in line 80 at 2023-07-moonwell/src/core/IRModels/WhitePaperInterestRateModel.sol:

    function getSupplyRate(uint cash, uint borrows, uint reserves, uint reserveFactorMantissa) override public view returns (uint) {

------------------------------------------------------------------------ 

### Mitigation 

The following functions could be set external to save gas and improve code quality. External call cost is less expensive than of public functions.










# VULN 10 

## [GAS] Use hardcode address instead address(this)
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 965 at 2023-07-moonwell/src/core/Comptroller.sol:

            token.transfer(admin, token.balanceOf(address(this)));


Found in line 483 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

        (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));


Found in line 70 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.transferAllowed(address(this), src, dst, tokens);


Found in line 500 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.mintAllowed(address(this), minter, mintAmount);


Found in line 556 at 2023-07-moonwell/src/core/MToken.sol:

        emit Transfer(address(this), minter, vars.mintTokens);


Found in line 667 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.redeemAllowed(address(this), redeemer, vars.redeemTokens);


Found in line 706 at 2023-07-moonwell/src/core/MToken.sol:

        emit Transfer(redeemer, address(this), vars.redeemTokens);


Found in line 710 at 2023-07-moonwell/src/core/MToken.sol:

        comptroller.redeemVerify(address(this), redeemer, vars.redeemAmount, vars.redeemTokens);


Found in line 752 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.borrowAllowed(address(this), borrower, borrowAmount);


Found in line 867 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.repayBorrowAllowed(address(this), payer, borrower, repayAmount);


Found in line 970 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.liquidateBorrowAllowed(address(this), address(mTokenCollateral), liquidator, borrower, repayAmount);


Found in line 1012 at 2023-07-moonwell/src/core/MToken.sol:

        (uint amountSeizeError, uint seizeTokens) = comptroller.liquidateCalculateSeizeTokens(address(this), address(mTokenCollateral), actualRepayAmount);


Found in line 1020 at 2023-07-moonwell/src/core/MToken.sol:

        if (address(mTokenCollateral) == address(this)) {


Found in line 1021 at 2023-07-moonwell/src/core/MToken.sol:

            seizeError = seizeInternal(address(this), liquidator, borrower, seizeTokens);


Found in line 1076 at 2023-07-moonwell/src/core/MToken.sol:

        uint allowed = comptroller.seizeAllowed(address(this), seizerToken, liquidator, borrower, seizeTokens);


Found in line 1126 at 2023-07-moonwell/src/core/MToken.sol:

        emit Transfer(borrower, address(this), vars.protocolSeizeTokens);


Found in line 1127 at 2023-07-moonwell/src/core/MToken.sol:

        emit ReservesAdded(address(this), vars.protocolSeizeAmount, vars.totalReservesNew);


Found in line 72 at 2023-07-moonwell/src/core/MErc20.sol:

            msg.sender, address(this),


Found in line 151 at 2023-07-moonwell/src/core/MErc20.sol:

    	uint256 balance = token.balanceOf(address(this));


Found in line 173 at 2023-07-moonwell/src/core/MErc20.sol:

        return token.balanceOf(address(this));


Found in line 189 at 2023-07-moonwell/src/core/MErc20.sol:

        uint balanceBefore = EIP20Interface(underlying_).balanceOf(address(this));


Found in line 190 at 2023-07-moonwell/src/core/MErc20.sol:

        token.transferFrom(from, address(this), amount);


Found in line 209 at 2023-07-moonwell/src/core/MErc20.sol:

        uint balanceAfter = EIP20Interface(underlying_).balanceOf(address(this));


Found in line 38 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

            mToken.balanceOf(address(this))


Found in line 48 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

            address(this),


Found in line 57 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

        weth.withdraw(weth.balanceOf(address(this)));


Found in line 60 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

            value: address(this).balance


Found in line 141 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            msg.sender == address(this),


Found in line 168 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            msg.sender == address(this),


Found in line 190 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            msg.sender == address(this),


Found in line 208 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            msg.sender == oldGuardian || msg.sender == address(this),


Found in line 322 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");


Found in line 146 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

            feed != address(0) && feed != address(this),


Found in line 480 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

                token.balanceOf(address(this))


Found in line 1232 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        uint256 currentTokenHoldings = token.balanceOf(address(this));

------------------------------------------------------------------------ 

### Mitigation 

Instead of using address(this), it is more gas-efficient to pre-calculate and use the hardcoded address. Foundry’s script.sol and solmate’s LibRlp.sol contracts can help achieve this.










# VULN 11 

## [GAS] Functions guaranteed to revert when called by normal users can be marked payable
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 266 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {


Found in line 274 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    function togglePause() external onlyOwner {

------------------------------------------------------------------------ 

### Mitigation 

If a function modifier or require such as onlyOwner/onlyX is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2), DUP1(3), ISZERO(3), PUSH2(3), JUMPI(10), PUSH1(3), DUP1(3), REVERT(0), JUMPDEST(1), POP(2) which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost.










# VULN 12 

## [GAS] Do not calculate constants
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 45 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

    uint256 public constant liquidationIncentive = 1.1e18; /// liquidation incentive is 110%


Found in line 46 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

    uint256 public constant closeFactor = 5e17; /// close factor is 50%, i.e. seize share


Found in line 47 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

    uint8 public constant mTokenDecimals = 8; /// all mTokens have 8 decimals


Found in line 62 at 2023-07-moonwell/src/core/Comptroller.sol:

    uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05


Found in line 65 at 2023-07-moonwell/src/core/Comptroller.sol:

    uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9


Found in line 68 at 2023-07-moonwell/src/core/Comptroller.sol:

    uint internal constant collateralFactorMaxMantissa = 0.9e18; // 0.9


Found in line 20 at 2023-07-moonwell/src/core/IRModels/JumpRateModel.sol:

    uint public constant timestampsPerYear = 60 * 60 * 24 * 365;

------------------------------------------------------------------------ 

### Mitigation 

Due to how constant variables are implemented (replacements at compile-time), an expression assigned to a constant variable is recomputed each time that the variable is used, which wastes some gas.










# VULN 13 

## [GAS] Don’t compare boolean expressions to boolean literals
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 129 at 2023-07-moonwell/src/core/Comptroller.sol:

        if (marketToJoin.accountMembership[borrower] == true) {


Found in line 1038 at 2023-07-moonwell/src/core/Comptroller.sol:

            if (suppliers == true) {


Found in line 1046 at 2023-07-moonwell/src/core/Comptroller.sol:

            if (borrowers == true) {

------------------------------------------------------------------------ 

### Mitigation 

In Solidity, when a boolean expression is compared to a boolean literal, it can result in additional gas costs due to the additional comparison operation that is performed.










# VULN 14 

## [GAS] Using private rather than public for constants, saves gas
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 37 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    uint256 public immutable proposalDelay;


Found in line 40 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    uint256 public immutable permissionlessUnpauseTime;

------------------------------------------------------------------------ 

### Mitigation 

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it’s used, and not adding another entry to the method ID table.










# VULN 15 

## [GAS] With assembly, .call (bool success) transfer can be done gas-optimized
------------------------------------------------------------------------ 

### Proof of concept 

Found in 2023-07-moonwell/src/core/Unitroller.sol (function _acceptAdmin() public returns (uint) {):

     * It returns to the external caller whatever the implementation returns

     * or forwards reverts.

     */

    fallback() external {

        // delegate all other functions to current implementation

        (bool success, ) = comptrollerImplementation.delegatecall(msg.data);



        assembly {

              let free_mem_ptr := mload(0x40)

              returndatacopy(free_mem_ptr, 0, returndatasize())





Found in 2023-07-moonwell/src/core/MErc20Delegator.sol (function delegateTo(address callee, bytes memory data) internal returns (bytes memory) {):

     * @param callee The contract to delegatecall

     * @param data The raw data to delegatecall

     * @return The returned bytes from the delegatecall

     */

    function delegateTo(address callee, bytes memory data) internal returns (bytes memory) {

        (bool success, bytes memory returnData) = callee.delegatecall(data);

        assembly {

            if eq(success, 0) {

                revert(add(returnData, 0x20), returndatasize())

            }

        }



Found in 2023-07-moonwell/src/core/MErc20Delegator.sol (function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {):

     *  There are an additional 2 prefix uints from the wrapper returndata, which we ignore since we make an extra hop.

     * @param data The raw data to delegatecall

     * @return The returned bytes from the delegatecall

     */

    function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {

        (bool success, bytes memory returnData) = address(this).staticcall(abi.encodeWithSignature("delegateToImplementation(bytes)", data));

        assembly {

            if eq(success, 0) {

                revert(add(returnData, 0x20), returndatasize())

            }

        }



Found in 2023-07-moonwell/src/core/MErc20Delegator.sol (function delegateToViewImplementation(bytes memory data) public view returns (bytes memory) {):

     */

    fallback () external payable {

        require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");



        // delegate all other functions to current implementation

        (bool success, ) = implementation.delegatecall(msg.data);



        assembly {

            let free_mem_ptr := mload(0x40)

            returndatacopy(free_mem_ptr, 0, returndatasize())





Found in 2023-07-moonwell/src/core/router/WETHRouter.sol (function redeem(uint256 mTokenRedeemAmount, address recipient) external {):

            "WETHRouter: redeem failed"

        );



        weth.withdraw(weth.balanceOf(address(this)));



        (bool success, ) = payable(recipient).call{

            value: address(this).balance

        }("");

        require(success, "WETHRouter: ETH transfer failed");

    }





Found in 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol (function _executeProposal(bytes memory VAA, bool overrideDelay) private {):

            address target = targets[i];

            uint256 value = values[i];

            bytes memory data = calldatas[i];



            // Go make our call, and if it is not successful revert with the error bubbling up

            (bool success, bytes memory returnData) = target.call{value: value}(

                data

            );



            /// revert on failure with error message if any

            require(success, string(returnData));


------------------------------------------------------------------------ 

### Mitigation 

return data (bool success,) has to be stored due to EVM architecture, but in a usage like below, ‘out’ and ‘outsize’ values are given (0,0), this storage disappears and gas optimization is provided.










# VULN 16 

## [GAS] Empty blocks should be removed or emit something
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 473 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

    function teardown(Addresses addresses, address) public pure {}


Found in line 15 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

    constructor() {}


Found in line 65 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

    receive() external payable {}

------------------------------------------------------------------------ 

### Mitigation 

The code should be refactored such that they no longer exist, or the block should do something useful, such as emitting an event or reverting. If the contract is meant to be extended, the contract should be abstract and the function signatures be added without any default implementation. If the block is an empty if-statement block to avoid doing subsequent checks in the else-if/else conditions, the else-if/else conditions should be nested under the negation of the if-statement, because they involve different classes of checks, which may lead to the introduction of errors when the code is later modified (if(x){}else if(y){...}else{...} => if(!x){if(y){...}else{...}}).










# VULN 17 

## [GAS] Use double require instead of using &&
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 813 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");



Found in line 850 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");



Found in line 33 at 2023-07-moonwell/src/core/MToken.sol:

        require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");


------------------------------------------------------------------------ 

### Mitigation 

Using double require instead of operator && can save more gas. When having a require statement with 2 or more expressions needed, place the expression that cost less gas first.










# VULN 18 

## [GAS] bytes constants are more eficient than string constans
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 44 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

    string public constant name = "MIP00";

------------------------------------------------------------------------ 

### Mitigation 

If the data can fit in 32 bytes, the bytes32 data type can be used instead of bytes or strings, as it is less robust in terms of robustness.










# VULN 19 

## [GAS] Use assembly to write address storage values
------------------------------------------------------------------------ 

### Proof of concept 

Found in lines 25 to 34 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

    constructor(address underlying_,
                ComptrollerInterface comptroller_,
                InterestRateModel interestRateModel_,
                uint initialExchangeRateMantissa_,
                string memory name_,
                string memory symbol_,
                uint8 decimals_,
                address payable admin_,
                address implementation_,
                bytes memory becomeImplementationData) {


Found in lines 61 to 66 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    constructor(
        address wormholeCore,
        uint256 _proposalDelay,
        uint256 _permissionlessUnpauseTime,
        TrustedSender[] memory _trustedSenders
    ) Ownable() {


Found in lines 35 to 39 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

    constructor(
        address baseAddress,
        address multiplierAddress,
        address secondMultiplierAddress
    ) {

------------------------------------------------------------------------ 

### Mitigation 

Use assembly to write address storage values. Here are a few reasons: * Reduced opcode usage: When using assembly, you can directly manipulate storage values using lower-level instructions like sstore (storage store) instead of relying on higher-level Solidity storage assignments. These direct operations typically result in fewer opcode executions, reducing gas costs. * Avoiding unnecessary checks: Solidity storage assignments often involve additional checks and operations, such as enforcing security modifiers or triggering events. By using assembly, you can bypass these additional checks and perform the necessary storage operations directly, resulting in gas savings. * Optimized packing: Assembly provides greater flexibility in packing and unpacking data structures. By carefully arranging and manipulating the storage layout in assembly, you can achieve more efficient storage utilization and minimize wasted storage space. * Fine-grained control: Assembly allows for precise control over gas-consuming operations. You can optimize gas usage by selecting specific instructions and minimizing unnecessary operations or data copying.










# VULN 20 

## [GAS] require() or revert() statements that check input arguments should be at the top of the function (Also restructured some if’s)
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 98 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

            require(


Found in line 145 at 2023-07-moonwell/src/core/Unitroller.sol:

              case 0 { revert(free_mem_ptr, returndatasize()) }


Found in line 30 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

        require(msg.sender == admin, "only the admin may call _becomeImplementation");


Found in line 42 at 2023-07-moonwell/src/core/MErc20Delegate.sol:

        require(msg.sender == admin, "only the admin may call _resignImplementation");


Found in line 236 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(nextTotalSupplies < supplyCap, "market supply cap reached");


Found in line 299 at 2023-07-moonwell/src/core/Comptroller.sol:

            revert("redeemTokens zero");


Found in line 320 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(msg.sender == mToken, "sender must be mToken");


Found in line 341 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(nextTotalBorrows < borrowCap, "market borrow cap reached");


Found in line 441 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(!seizeGuardianPaused, "seize is paused");


Found in line 781 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(mToken.isMToken(), "Must be an MToken"); // Sanity check to make sure its really a MToken


Found in line 813 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(numMarkets != 0 && numMarkets == numBorrowCaps, "invalid input");


Found in line 850 at 2023-07-moonwell/src/core/Comptroller.sol:

        require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");


Found in line 1035 at 2023-07-moonwell/src/core/Comptroller.sol:

            require(markets[address(mToken)].isListed, "market must be listed");


Found in line 497 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

        require(msg.value == 0,"MErc20Delegator:fallback: cannot send value to fallback");


Found in line 507 at 2023-07-moonwell/src/core/MErc20Delegator.sol:

            case 0 { revert(free_mem_ptr, returndatasize()) }


Found in line 32 at 2023-07-moonwell/src/core/MToken.sol:

        require(msg.sender == admin, "only admin may initialize the market");


Found in line 33 at 2023-07-moonwell/src/core/MToken.sol:

        require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");


Found in line 37 at 2023-07-moonwell/src/core/MToken.sol:

        require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");


Found in line 41 at 2023-07-moonwell/src/core/MToken.sol:

        require(err == uint(Error.NO_ERROR), "setting comptroller failed");


Found in line 49 at 2023-07-moonwell/src/core/MToken.sol:

        require(err == uint(Error.NO_ERROR), "setting interest rate model failed");


Found in line 403 at 2023-07-moonwell/src/core/MToken.sol:

        require(borrowRateMantissa <= borrowRateMaxMantissa, "borrow rate is absurdly high");


Found in line 407 at 2023-07-moonwell/src/core/MToken.sol:

        require(mathErr == MathError.NO_ERROR, "could not calculate block delta");


Found in line 537 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "MINT_EXCHANGE_CALCULATION_FAILED");


Found in line 545 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_TOTAL_SUPPLY_CALCULATION_FAILED");


Found in line 548 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "MINT_NEW_ACCOUNT_BALANCE_CALCULATION_FAILED");


Found in line 914 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_ACCOUNT_BORROW_BALANCE_CALCULATION_FAILED");


Found in line 917 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "REPAY_BORROW_NEW_TOTAL_BALANCE_CALCULATION_FAILED");


Found in line 1013 at 2023-07-moonwell/src/core/MToken.sol:

        require(amountSeizeError == uint(Error.NO_ERROR), "LIQUIDATE_COMPTROLLER_CALCULATE_AMOUNT_SEIZE_FAILED");


Found in line 1016 at 2023-07-moonwell/src/core/MToken.sol:

        require(mTokenCollateral.balanceOf(borrower) >= seizeTokens, "LIQUIDATE_SEIZE_TOO_MUCH");


Found in line 1027 at 2023-07-moonwell/src/core/MToken.sol:

        require(seizeError == uint(Error.NO_ERROR), "token seizure failed");


Found in line 1102 at 2023-07-moonwell/src/core/MToken.sol:

        require(vars.mathErr == MathError.NO_ERROR, "exchange rate math error");


Found in line 1203 at 2023-07-moonwell/src/core/MToken.sol:

        require(newComptroller.isComptroller(), "marker method returned false");


Found in line 1308 at 2023-07-moonwell/src/core/MToken.sol:

        require(totalReservesNew >= totalReserves, "add reserves unexpected overflow");


Found in line 1372 at 2023-07-moonwell/src/core/MToken.sol:

        require(totalReservesNew <= totalReserves, "reduce reserves unexpected underflow");


Found in line 1426 at 2023-07-moonwell/src/core/MToken.sol:

        require(newInterestRateModel.isInterestRateModel(), "marker method returned false");


Found in line 1515 at 2023-07-moonwell/src/core/MToken.sol:

        require(_notEntered, "re-entered");


Found in line 203 at 2023-07-moonwell/src/core/MErc20.sol:

                    revert(0, 0)


Found in line 206 at 2023-07-moonwell/src/core/MErc20.sol:

        require(success, "TOKEN_TRANSFER_IN_FAILED");


Found in line 210 at 2023-07-moonwell/src/core/MErc20.sol:

        require(balanceAfter >= balanceBefore, "TOKEN_TRANSFER_IN_OVERFLOW");


Found in line 238 at 2023-07-moonwell/src/core/MErc20.sol:

                    revert(0, 0)


Found in line 241 at 2023-07-moonwell/src/core/MErc20.sol:

        require(success, "TOKEN_TRANSFER_OUT_FAILED");


Found in line 52 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

        require(


Found in line 62 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

        require(success, "WETHRouter: ETH transfer failed");


Found in line 248 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(


Found in line 306 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(valid, reason);


Found in line 322 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(intendedRecipient == address(this), "TemporalGovernor: Incorrect destination");


Found in line 325 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(


Found in line 331 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(


Found in line 352 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(valid, reason); /// ensure VAA parsing verification succeeded


Found in line 355 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            require(


Found in line 359 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            require(


Found in line 370 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(


Found in line 375 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(


Found in line 405 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

            require(success, string(returnData));


Found in line 418 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

        require(


Found in line 139 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        require(


Found in line 191 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

        require(valid, "CLCOracle: Oracle data is invalid");


Found in line 51 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        require(msg.sender == admin, "only admin may call");


Found in line 103 at 2023-07-moonwell/src/core/Oracles/ChainlinkOracle.sol:

        require(updatedAt != 0, "Round is in incompleted state");


Found in line 97 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 104 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 125 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 134 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 152 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 162 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 393 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 399 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 403 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 409 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 421 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

            require(


Found in line 674 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 678 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 718 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 722 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 789 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 793 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        require(


Found in line 990 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

        revert("Unable to find emission token in mToken configs");

------------------------------------------------------------------------ 

### Mitigation 

Checks that involve constants should come before checks that involve state variables, function calls, and calculations. By doing these checks first, the function is able to revert before wasting alot of gas in a function that may ultimately revert in the unhappy case.
