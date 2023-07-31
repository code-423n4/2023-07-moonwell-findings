1. Approach taken in evaluating the codebase

Focused on the oracle contracts (Chainlink Oracle and Chainlink Composite Oracle) as well as Comptroller. 

2. Codebase quality analysis

Codebase is written well; proper events are emitted, errors are explicit, there are some failsafe mechanisms like pause and emergency rescue. 

Reentrancy modifier is created in Comptroller but not used (the last few lines). Only ReentrancyGuard from Openzeppelin in MultiRewardDistributer.sol is used once. 

Supply cap and borrow cap is checked rigorously so that even if the supply cap is changed, the protocol will not break

```
            require(nextTotalSupplies < supplyCap, "market supply cap reached");
```

3. Centralization risks

A lot of power is given to the admin and the pauseGuardian. For the pauseGuardian in Comptroller.sol, he can pause most functions, such as borrow, transfer, seize.

```
    function _setBorrowPaused(MToken mToken, bool state) public returns (bool) {
        require(markets[address(mToken)].isListed, "cannot pause a market that is not listed");
        require(msg.sender == pauseGuardian || msg.sender == admin, "only pause guardian and admin can pause");
        require(msg.sender == admin || state == true, "only admin can unpause");


        borrowGuardianPaused[address(mToken)] = state;
        emit ActionPaused(mToken, "Borrow", state);
        return state;
    }
```

The admin has more power, in that he holds the power to unpause the protocol. The admin also has the power to withdraw all funds in the contract using `_resueFunds`, and thus a malicious admin can potentially run away with funds.

```
    function _rescueFunds(address _tokenAddress, uint _amount) external {
        require(msg.sender == admin, "Unauthorized");


        IERC20 token = IERC20(_tokenAddress);
        // Similar to mTokens, if this is uint.max that means "transfer everything"
        if (_amount == type(uint).max) {
            token.transfer(admin, token.balanceOf(address(this)));
        } else {
            token.transfer(admin, _amount);
        }
    }
```

The admin also controls the borrow and supply caps, liquidation incentives, collateral factor and more. A malicious admin can absolutely brick the whole protocol if he wanted to.

```
    function _setMarketSupplyCaps(MToken[] calldata mTokens, uint[] calldata newSupplyCaps) external {
        require(msg.sender == admin || msg.sender == supplyCapGuardian, "only admin or supply cap guardian can set supply caps");


        uint numMarkets = mTokens.length;
        uint numSupplyCaps = newSupplyCaps.length;


        require(numMarkets != 0 && numMarkets == numSupplyCaps, "invalid input");


        for(uint i = 0; i < numMarkets; i++) {
            supplyCaps[address(mTokens[i])] = newSupplyCaps[i];
            emit NewSupplyCap(mTokens[i], newSupplyCaps[i]);
        }
    }
```
There is a lot of centralization risks going on in the protocol, so its is the protocol responsibility not to lose the private keys of the admin account or not have a rouge admin.

4. Mechanism review

For the Chainlink oracle, this line in ChainlinkCompositeOracle.sol is pretty dubious at first

```
        bool valid = price > 0 && answeredInRound == roundId;
```

Normally the check goes `require(answeredInRound >= roundID, "round not complete")`, but this time is a strict equality sign. Not sure if having a strict equality will result in any issue

Oracles also assume that USD-denominated feeds store answers at 8 decimals, but that is not the case for all USD feeds, such as AMPL/USD which returns 18 decimals. 

```
        // Chainlink USD-denominated feeds store answers at 8 decimals
        uint256 decimalDelta = uint256(18).sub(feed.decimals());
        // Ensure that we don't multiply the result by 0
        if (decimalDelta > 0) {
            return uint256(answer).mul(10 ** decimalDelta);
        } else {
            return uint256(answer);
        }
```

Somehow this issue is negated with the else statement, but just keep in mind that not all tokens are 18 decimals and not all USD-denominated pair are 8 decimals. Also, the decimals might change anytime by Chainlink, so it is not ideal to rely on historical data.

### Time spent:
8 hours