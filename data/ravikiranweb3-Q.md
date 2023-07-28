1)  Comptroller::setMarketBorrowCaps()
    Set Borrow caps for markets that are listed. 
    require(markets[address(mToken)].isListed,"Invalid token");
    
2)  Comptroller::_setMarketSupplyCaps()  
    Set Supply caps for markets that are listed.
    require(markets[address(mToken)].isListed,"Invalid token");

3)  chainlinkCompositeOracle is using a floating pragma and also older version of compiler different from 
    other contract files.

4)  Comptroller::nonReentrant is an unused modifier and so is the _locked storage variable.