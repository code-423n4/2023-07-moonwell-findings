#### Gas Findings

[Gas-1] ++i costs less gas than i++, especially when it's used in for-loops
Instances (9):
```
Comptroller.sol
106:         for (uint i = 0; i < len; i++) {
186:         for (uint i = 0; i < len; i++) {
574:         for (uint i = 0; i < assets.length; i++) {
795:         for (uint i = 0; i < allMarkets.length; i ++) {
815:         for(uint i = 0; i < numMarkets; i++) {
852:         for(uint i = 0; i < numMarkets; i++) {
1031:        for (uint i = 0; i < mTokens.length; i++) {
1040:        for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
1048:        for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
```

 