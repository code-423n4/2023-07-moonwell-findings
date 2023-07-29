1. Although this modifier is defined in the contract, it is not used in any of the contract's functions. 

`./src/core/Comptroller.sol:1075:    modifier nonReentrant() {  `

    modifier nonReentrant() {
        // On the first call to nonReentrant, _notEntered will be true
        require(_locked != 1, "ReentrancyGuard: reentrant call");

        // Any calls to nonReentrant after this point will fail
        _locked = 1;

        _;

        // By storing the original value once again, a refund is triggered (see
        // https://eips.ethereum.org/EIPS/eip-2200)
        _locked = 0;
    }



2. The redeemAllowed function is marked as public and view, which suggests that it should be a read-only function that doesn't modify the contract state. However, it calls updateCompSupplyIndex and distributeSupplierComp, both of which can modify the contract state.
```solidity
function redeemAllowed(address mToken, address redeemer, uint redeemTokens) public view returns (uint) {
    if (!markets[mToken].isListed) {
        return uint(Error.MARKET_NOT_LISTED);
    }

    // Check if redemption is paused
    if (redeemGuardianPaused[mToken]) {
        return uint(Error.REDEEM_GUARDIAN_PAUSED);
    }

    // Keep the flywheel moving
    Exp memory borrowIndex = Exp({mantissa: MToken(mToken).borrowIndex()});
    updateCompSupplyIndex(mToken);
    distributeSupplierComp(mToken, redeemer, false, borrowIndex.mantissa);

    return uint(Error.NO_ERROR);
}
```
