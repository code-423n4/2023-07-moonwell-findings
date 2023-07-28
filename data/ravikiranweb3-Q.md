1)  Comptroller::setMarketBorrowCaps()
    Set Borrow caps for markets that are listed. 
    require(markets[address(mToken)].isListed,"Invalid token");
    
2)  Comptroller::_setMarketSupplyCaps()  
    Set Supply caps for markets that are listed.
    require(markets[address(mToken)].isListed,"Invalid token");

3)  chainlinkCompositeOracle is using a floating pragma and also older version of compiler different from 
    other contract files.

4)  Comptroller::nonReentrant is an unused modifier and so is the _locked storage variable.

5) ChainlinkOracle::setAdmin should check for zero address for the parameter before updating the state 
   variable.


6) ChainlinkCompositeOracle::getDerivedPrice() There may be tokens with 0 decimals for which the oracle 
   will not work.


7) Comptroller::mintAllowed should be until the supply cap.
     require(nextTotalSupplies < supplyCap, "market supply cap reached");
  should be 
     require(nextTotalSupplies <= supplyCap, "market supply cap reached");