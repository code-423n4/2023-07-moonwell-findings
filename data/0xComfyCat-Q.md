# Table of Contents

## Low risk findings
1. [[L-01] Indefinite delay risk in proposals incorporating `_reduceReserve` call](#[L-01]-Indefinite-delay-risk-in-proposals-incorporating-`_reduceReserve`-call)
2. [[L-02] WETHRouter `mint` and `redeem` would revert when transfer is paused](#[L-02]-WETHRouter-`mint`-and-`redeem`-would-revert-when-transfer-is-paused)
3. [[L-03] Lack of token decimal validation in mToken initialization](#[L-03]-Lack-of-token-decimal-validation-in-mToken-initialization)
## QA findings
1. [[Q-01] Shorter contract lifespan due to storing timestamps in uint32](#[Q-01]-Shorter-contract-lifespan-due-to-storing-timestamps-in-uint32)
2. [[Q-02] Unused variable `borrowerIndex` in `repayBorrowFresh`](#[Q-02]-Unused-variable-`borrowerIndex`-in-`repayBorrowFresh`)

# [L-01] Indefinite delay risk in proposals incorporating `_reduceReserve` call

The `_reduceReserve` function, which initiates the transfer of tokens out of the mToken contract, inherently necessitates a sufficient balance within the mToken contract to facilitate the transfer.

```
        // Fail gracefully if protocol has insufficient underlying cash
        if (getCashPrior() < reduceAmount) {
            return fail(Error.TOKEN_INSUFFICIENT_CASH, FailureInfo.REDUCE_RESERVES_CASH_NOT_AVAILABLE);
        }
```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L1356-L1359

Potential exists for an ill-intentioned actor to manipulate the cash reserve through borrowing, consequently preventing the execution of the proposal. It is prudent, therefore, to avoid combining the `_reduceReserve` function with other high-stakes calls such as lowering the collateral factor in the same proposal.

# [L-02] WETHRouter `mint` and `redeem` would revert when transfer is paused

The mint and redeem functions of the WETHRouter involve the transfer of mTokens as part of the wrapping process. Consequently, these functions may cease to function if the transfer operation is paused. It is, therefore, recommended that the DApp frontend be designed to handle this particular case seamlessly, enabling the user to mint/redeem as WETH via mToken even in the event of a transfer pause.
```
    function mint(address recipient) external payable {
        weth.deposit{value: msg.value}();

        require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");

        // @audit - Revert if transfer is paused
        IERC20(address(mToken)).safeTransfer(
            recipient,
            mToken.balanceOf(address(this))
        );
    }
    
    function redeem(uint256 mTokenRedeemAmount, address recipient) external {
	    // @audit - Revert if transfer is paused
        IERC20(address(mToken)).safeTransferFrom(
            msg.sender,
            address(this),
            mTokenRedeemAmount
        );
        ...
    }
```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L36-L39
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L46-L50

# [L-03] Lack of token decimal validation in mToken initialization

The design of ChainlinkOracle is such that it does not accommodate the support of underlying tokens that have more than 18 decimals. This necessary verification appears to be absent in the initialization process of mToken.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L26

# [Q-01] Shorter contract lifespan due to storing timestamps in uint32

With uint32, contract would last until 2106

# [Q-02] Unused variable `borrowerIndex` in `repayBorrowFresh`
```
        /* We remember the original borrowerIndex for verification purposes */
        vars.borrowerIndex = accountBorrows[borrower].interestIndex;
```
https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol#L879-L880