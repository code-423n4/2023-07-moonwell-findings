## QA
---

### Layout Order [1]

- The best-practices for layout within a contract is the following order: state variables, events, modifiers, constructor and functions.

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

```solidity
// place this struct before the constructor
67:    struct CTokenAddresses {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol

```solidity
// place this modifier right before the constructor
50:    modifier onlyAdmin() {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol

```solidity
// place this event after state variables
15:    event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp, uint jumpMultiplierPerTimestamp, uint kink);
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/WhitePaperInterestRateModel.sol

```solidity
// place this event after state variables
15:    event NewInterestParams(uint baseRatePerTimestamp, uint multiplierPerTimestamp);
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

```solidity
// falback should come right after the constructor
496:    fallback () external payable {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol

```solidity
// place these structs with other state variables.
481:    struct MintLocalVars {
597:    struct RedeemLocalVars {
738:    struct BorrowLocalVars {
847:    struct RepayBorrowLocalVars {
1052:    struct SeizeInternalLocalVars {

// place this modifier before the constructor
1514:    modifier nonReentrant() {
```



https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol

```solidity
// place these state variables before the event declarations.
62:    uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05
65:    uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9
68:    uint internal constant collateralFactorMaxMantissa = 0.9e18; // 0.9

// place this modifier before the constructor
1075:    modifier nonReentrant() {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

```solidity
// place these modifiers before the constructor
124:    modifier onlyComptrollersAdmin() {
133:    modifier onlyComptrollerOrAdmin() {
143:    modifier onlyEmissionConfigOwnerOrAdmin(
161:    modifier onlyPauseGuardianOrAdmin() {
```

---

### Function Visibility [2]

- Order of Functions: Ordering helps readers identify which functions they can call and to find the constructor and fallback definitions easier. Functions should be grouped according to their visibility and ordered: constructor, receive function (if exists), fallback function (if exists), external, public, internal, private. Within a grouping, place the view and pure functions last.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol

```solidity
// place these external function before all the other ones.
118:    function setUnderlyingPrice(
135:    function setDirectPrice(address asset, uint256 price) external onlyAdmin {
144:    function setFeed(string calldata symbol, address feed) external onlyAdmin {
167:    function assetPrices(address asset) external view returns (uint256) {
173:    function setAdmin(address newAdmin) external onlyAdmin {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol

```solidity
// place this receive right after the constructor
65:    receive() external payable {}
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

```solidity
// external functions coming after public ones
326:    function getCash() override external view returns (uint) {
350:    function seize(address liquidator, address borrower, uint seizeTokens) override external returns (uint) {
359:    function sweepToken(EIP20NonStandardInterface token) override external {
372:    function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {
392:    function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {
402:    function _acceptAdmin() override external returns (uint) {
412:    function _addReserves(uint addAmount) override external returns (uint) {
422:    function _reduceReserves(uint reduceAmount) override external returns (uint) {
443:    function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {

// internal function coming before public ones
455:    function delegateTo(address callee, bytes memory data) internal returns (bytes memory) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol

```solidity
// these public functions are coming before some external ones and after some internal ones.
26:    function initialize(ComptrollerInterface comptroller_,
272:    function borrowBalanceStored(address account) override public view returns (uint) {
319:    function exchangeRateCurrent() override public nonReentrant returns (uint) {
329:    function exchangeRateStored() override public view returns (uint) {
385:    function accrueInterest() virtual override public returns (uint) {
1195:    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {
1391:    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {


// place these internal functions right all the other ones since there are no private functions in this contract
68:    function transferTokens(address spender, address src, address dst, uint tokens) internal returns (uint) {
228:    function getBlockTimestamp() virtual internal view returns (uint) {
283:    function borrowBalanceStoredInternal(address account) internal view returns (MathError, uint) {
340:    function exchangeRateStoredInternal() virtual internal view returns (MathError, uint) {
471:    function mintInternal(uint mintAmount) internal nonReentrant returns (uint, uint) {
498:    function mintFresh(address minter, uint mintAmount) internal returns (uint, uint) {
571:    function redeemInternal(uint redeemTokens) internal nonReentrant returns (uint) {
587:    function redeemUnderlyingInternal(uint redeemAmount) internal nonReentrant returns (uint) {
615:    function redeemFresh(address payable redeemer, uint redeemTokensIn, uint redeemAmountIn) internal returns (uint) {
728:    function borrowInternal(uint borrowAmount) internal nonReentrant returns (uint) {
750:    function borrowFresh(address payable borrower, uint borrowAmount) internal returns (uint) {
821:    function repayBorrowInternal(uint repayAmount) internal nonReentrant returns (uint, uint) {
837:    function repayBorrowBehalfInternal(address borrower, uint repayAmount) internal nonReentrant returns (uint, uint) {
865:    function repayBorrowFresh(address payer, address borrower, uint repayAmount) internal returns (uint, uint) {
942:    function liquidateBorrowInternal(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal nonReentrant returns (uint, uint) {
968:    function liquidateBorrowFresh(address liquidator, address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal returns (uint, uint) {
1074:    function seizeInternal(address seizerToken, address liquidator, address borrower, uint seizeTokens) internal returns (uint) {
1234:    function _setReserveFactorFresh(uint newReserveFactorMantissa) internal returns (uint) {
1263:    function _addReservesInternal(uint addAmount) internal nonReentrant returns (uint) {
1281:    function _addReservesFresh(uint addAmount) internal returns (uint, uint) {
1342:    function _reduceReservesFresh(uint reduceAmount) internal returns (uint) {
1407:    function _setInterestRateModelFresh(InterestRateModel newInterestRateModel) internal returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

```solidity
// place these external function before all the public and internal functions
96:    function isTrustedSender(
106:    function allTrustedSenders(uint16 chainId)
137:    function setTrustedSenders(
164:    function unSetTrustedSenders(
188:    function grantGuardiansPause() external {
205:    function revokeGuardian() external {
229:    function queueProposal(bytes memory VAA) external whenNotPaused {
242:    function permissionlessUnpause() external whenPaused {
266:    function fastTrackProposalExecution(bytes memory VAA) external onlyOwner {
274:    function togglePause() external onlyOwner {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol

```solidity
// place this fallback right after the constructor
136:    fallback() external {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol

```solidity
// public functions coming before some external ones and after some internal ones
102:    function enterMarkets(address[] memory mTokens) override public returns (uint[] memory) {
516:    function getAccountLiquidity(address account) public view returns (uint, uint, uint) {
542:    function getHypotheticalAccountLiquidity(
665:    function _setPriceOracle(PriceOracle newOracle) public returns (uint) {
880:    function _setPauseGuardian(address newPauseGuardian) public returns (uint) {
901:    function _setRewardDistributor(MultiRewardDistributor newRewardDistributor) public {
911:    function _setMintPaused(MToken mToken, bool state) public returns (bool) {
921:    function _setBorrowPaused(MToken mToken, bool state) public returns (bool) {
931:    function _setTransferPaused(bool state) public returns (bool) {
940:    function _setSeizePaused(bool state) public returns (bool) {
949:    function _become(Unitroller unitroller) public {


// internal functions coming before some public and external ones.
121:    function addToMarketInternal(MToken mToken, address borrower) internal returns (Error) {
263:    function redeemAllowedInternal(address mToken, address redeemer, uint redeemTokens) internal view returns (uint) {
528:    function getAccountLiquidityInternal(address account) internal view returns (Error, uint, uint) {
563:    function getHypotheticalAccountLiquidityInternal(
794:    function _addMarketInternal(address mToken) internal {
978:    function updateAndDistributeSupplierRewardsForToken(address mToken, address supplier) internal {
989:    function updateAndDistributeBorrowerRewardsForToken(address mToken, address borrower) internal {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

```solidity
// externan functions coming after some public ones, should come before
333:    function getCurrentEmissionCap() external view returns (uint256) {
382:    function _addEmissionConfig(
471:    function _rescueFunds(
493:    function _setPauseGuardian(
512:    function _setEmissionCap(
536:    function updateMarketSupplyIndex(
549:    function disburseSupplierRewards(
564:    function updateMarketSupplyIndexAndDisburseSupplierRewards(
577:    function updateMarketBorrowIndex(
590:    function disburseBorrowerRewards(
605:    function updateMarketBorrowIndexAndDisburseBorrowerRewards(
628:    function _pauseRewards() external onlyPauseGuardianOrAdmin {
635:    function _unpauseRewards() external onlyComptrollersAdmin {
659:    function _updateSupplySpeed(
703:    function _updateBorrowSpeed(
747:    function _updateOwner(
775:    function _updateEndTime(
```

---

### natSpec missing [3]

Some functions are missing @params or @returns. Specification Format.” These are written with a triple slash (///) or a double asterisk block(/** ... */) directly above function declarations or statements to generate documentation in JSON format for developers and end-users. It is recommended that Solidity contracts are fully annotated using NatSpec for all public interfaces (everything in the ABI). These comments contain different types of tags:
- @title: A title that should describe the contract/interface @author: The name of the author (for contract, interface) 
- @notice: Explain to an end user what this does (for contract, interface, function, public state variable, event) 
- @dev: Explain to a developer any extra details (for contract, interface, function, state variable, event) 
- @param: Documents a parameter (just like in doxygen) and must be followed by parameter name (for function, event)
- @return: Documents the return variables of a contract’s function (function, public state variable)
- @inheritdoc: Copies all missing tags from the base function and must be followed by the contract name (for function, public state variable)
- @custom…: Custom tag, semantics is application-defined (for everywhere)

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

```solidity
43:  contract mip00 is Proposal, CrossChainProposal, ChainIds, Configs {
63:    constructor() {
67:    struct CTokenAddresses {
249:    function afterDeploy(Addresses addresses, address) public {
319:    function build(Addresses addresses) public {
462:    function run(Addresses addresses, address) public {
466:    function printCalldata(Addresses addresses) public {
473:    function teardown(Addresses addresses, address) public pure {}
475:    function validate(Addresses addresses, address deployer) public {



// @param missing
77:    function deploy(Addresses addresses, address) public {

```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol

```solidity
// @param and @return missing
47:    function latestRoundData()

// @return missing
90:    function calculatePrice(
103:    function getDerivedPrice(
166:    function getPriceAndScale(

```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

```solidity
443:    function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol

```solidity
// @return missing
228:    function getBlockTimestamp() virtual internal view returns (uint) {
385:    function accrueInterest() virtual override public returns (uint) {

// @param missing
750:    function borrowFresh(address payable borrower, uint borrowAmount) internal returns (uint) {
1195:    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {
1219:    function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {
1234:    function _setReserveFactorFresh(uint newReserveFactorMantissa) internal returns (uint) {
1506:    function doTransferOut(address payable to, uint amount) virtual internal;

// @param and @return missing
1499:    function doTransferIn(address from, uint amount) virtual internal returns (uint);

481:    struct MintLocalVars {
597:    struct RedeemLocalVars {
738:    struct BorrowLocalVars {
847:    struct RepayBorrowLocalVars {
1052:    struct SeizeInternalLocalVars {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

```solidity
61:    constructor(
344:    function _executeProposal(bytes memory VAA, bool overrideDelay) private {

// @return missing
86:    function isTrustedSender(
96:    function isTrustedSender(

// @param missing
295:    function _queueProposal(bytes memory VAA) private {
412:    function _sanityCheckPayload(
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol

```solidity
33:    constructor() {

// @param and @return missing
39:    function _setPendingImplementation(address newPendingImplementation) public returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol

```solidity
70:    constructor() {
263:    function redeemAllowedInternal(address mToken, address redeemer, uint redeemTokens) internal view returns (uint) {
497:    struct AccountLiquidityLocalVars {
911:    function _setMintPaused(MToken mToken, bool state) public returns (bool) {
921:    function _setBorrowPaused(MToken mToken, bool state) public returns (bool) {
931:    function _setTransferPaused(bool state) public returns (bool) {
940:    function _setSeizePaused(bool state) public returns (bool) {
949:    function _become(Unitroller unitroller) public {
1064:    function getBlockTimestamp() public view returns (uint) {


// @return missing
394:    function liquidateBorrowAllowed(
434:    function seizeAllowed(

// @param missing
516:    function getAccountLiquidity(address account) public view returns (uint, uint, uint) {
528:    function getAccountLiquidityInternal(address account) internal view returns (Error, uint, uint) {
665:    function _setPriceOracle(PriceOracle newOracle) public returns (uint) {


```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

```solidity
71:    struct CurrentMarketData {
77:    struct CalculatedData {
88:    function initialize(
133:    modifier onlyComptrollerOrAdmin() {
143:    modifier onlyEmissionConfigOwnerOrAdmin(
161:    modifier onlyPauseGuardianOrAdmin() {

// @return missing
184:    function getCurrentOwner(

// @param and @return missing
197:    function getAllMarketConfigs(
218:    function getConfigForMarket(
231:    function getOutstandingRewardsForUser(
256:    function getOutstandingRewardsForUser(
382:    function _addEmissionConfig(

// @return missing
333:    function getCurrentEmissionCap() external view returns (uint256) {
340:    function getGlobalSupplyIndex(
355:    function getGlobalBorrowIndex(
827:    function calculateSupplyRewardsForUser(
865:    function calculateBorrowRewardsForUser(
912:    function calculateNewIndex(
976:    function fetchConfigByEmissionToken(
1214:    function sendReward(
```

---

### State variable and function names [4]

- Variables should be named according to their specifications
- private and internal `variables` should preppend with `underline`
- private and internal `functions` should preppend with `underline`
- constant state variables should be UPPER_CASE

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

```solidity
// constant state variables should be UPPER_CASE
44:    string public constant name = "MIP00";
45:    uint256 public constant liquidationIncentive = 1.1e18; /// liquidation incentive is 110%
46:    uint256 public constant closeFactor = 5e17; /// close factor is 50%, i.e. seize share
47:    uint8 public constant mTokenDecimals = 8; /// all mTokens have 8 decimals
50:    uint256 public constant proposalDelay = 5 minutes;
53:    uint256 public constant permissionlessUnpauseTime = 30 days;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkOracle.sol

```solidity
// private and internal `variables` should preppend with `underline`
23:    mapping (address => uint256) internal prices;
27:    mapping (bytes32 => AggregatorV3Interface) internal feeds;

// private and internal `functions` should preppend with `underline`
74:    function getPrice(MToken mToken) internal view returns (uint256 price) {
97:    function getChainlinkPrice(
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol

```solidity
//  constant state variables should be UPPER_CASE
29:    uint8 public constant decimals = 18;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/JumpRateModel.sol

```solidity
//  constant state variables should be UPPER_CASE
20:    uint public constant timestampsPerYear = 60 * 60 * 24 * 365;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/IRModels/WhitePaperInterestRateModel.sol

```solidity
//  constant state variables should be UPPER_CASE
20:    uint public constant timestampsPerYear = 31536000;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol

```solidity
68:    function transferTokens(address spender, address src, address dst, uint tokens) internal returns (uint) {
228:    function getBlockTimestamp() virtual internal view returns (uint) {
283:    function borrowBalanceStoredInternal(address account) internal view returns (MathError, uint) {
340:    function exchangeRateStoredInternal() virtual internal view returns (MathError, uint) {
471:    function mintInternal(uint mintAmount) internal nonReentrant returns (uint, uint) {
498:    function mintFresh(address minter, uint mintAmount) internal returns (uint, uint) {
571:    function redeemInternal(uint redeemTokens) internal nonReentrant returns (uint) {
587:    function redeemUnderlyingInternal(uint redeemAmount) internal nonReentrant returns (uint) {
615:    function redeemFresh(address payable redeemer, uint redeemTokensIn, uint redeemAmountIn) internal returns (uint) {
728:    function borrowInternal(uint borrowAmount) internal nonReentrant returns (uint) {
750:    function borrowFresh(address payable borrower, uint borrowAmount) internal returns (uint) {
821:    function repayBorrowInternal(uint repayAmount) internal nonReentrant returns (uint, uint) {
837:    function repayBorrowBehalfInternal(address borrower, uint repayAmount) internal nonReentrant returns (uint, uint) {
865:    function repayBorrowFresh(address payer, address borrower, uint repayAmount) internal returns (uint, uint) {
942:    function liquidateBorrowInternal(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal nonReentrant returns (uint, uint) {
968:    function liquidateBorrowFresh(address liquidator, address borrower, uint repayAmount, MTokenInterface mTokenCollateral) internal returns (uint, uint) {
1074:    function seizeInternal(address seizerToken, address liquidator, address borrower, uint seizeTokens) internal returns (uint) {
1493:    function getCashPrior() virtual internal view returns (uint);
1499:    function doTransferIn(address from, uint amount) virtual internal returns (uint);
1506:    function doTransferOut(address payable to, uint amount) virtual internal;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

```solidity
55:    mapping(uint16 => EnumerableSet.Bytes32Set) private trustedSenders;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol

```solidity
// remove the prefixed underline from public functions
39:    function _setPendingImplementation(address newPendingImplementation) public returns (uint) {
59:    function _acceptImplementation() public returns (uint) {
86:    function _setPendingAdmin(address newPendingAdmin) public returns (uint) {
109:    function _acceptAdmin() public returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol

```solidity
//  constant state variables should be UPPER_CASE
62:    uint internal constant closeFactorMinMantissa = 0.05e18; // 0.05
65:    uint internal constant closeFactorMaxMantissa = 0.9e18; // 0.9
68:    uint internal constant collateralFactorMaxMantissa = 0.9e18; // 0.9

// private and internal `functions` should preppend with `underline`
121:    function addToMarketInternal(MToken mToken, address borrower) internal returns (Error) {
263:    function redeemAllowedInternal(address mToken, address redeemer, uint redeemTokens) internal view returns (uint) {
528:    function getAccountLiquidityInternal(address account) internal view returns (Error, uint, uint) {
563:    function getHypotheticalAccountLiquidityInternal(
989:    function updateAndDistributeBorrowerRewardsForToken(address mToken, address borrower) internal {

// remove the prefixed underline from public and external functions
665:    function _setPriceOracle(PriceOracle newOracle) public returns (uint) {
689:    function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {
707:    function _setCollateralFactor(MToken mToken, uint newCollateralFactorMantissa) external returns (uint) {
748:    function _setLiquidationIncentive(uint newLiquidationIncentiveMantissa) external returns (uint) {
772:    function _supportMarket(MToken mToken) external returns (uint) {
807:    function _setMarketBorrowCaps(MToken[] calldata mTokens, uint[] calldata newBorrowCaps) external {
825:    function _setBorrowCapGuardian(address newBorrowCapGuardian) external {
844:    function _setMarketSupplyCaps(MToken[] calldata mTokens, uint[] calldata newSupplyCaps) external {
862:    function _setSupplyCapGuardian(address newSupplyCapGuardian) external {
880:    function _setPauseGuardian(address newPauseGuardian) public returns (uint) {
901:    function _setRewardDistributor(MultiRewardDistributor newRewardDistributor) public {
911:    function _setMintPaused(MToken mToken, bool state) public returns (bool) {
921:    function _setBorrowPaused(MToken mToken, bool state) public returns (bool) {
931:    function _setTransferPaused(bool state) public returns (bool) {
940:    function _setSeizePaused(bool state) public returns (bool) {
949:    function _become(Unitroller unitroller) public {
959:    function _rescueFunds(address _tokenAddress, uint _amount) external {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol

```solidity
// constant state variables should be UPPER_CASE
63:    uint224 public constant initialIndexConstant = 1e36;

// private and internal `functions` should preppend with `underline`
827:    function calculateSupplyRewardsForUser(
865:    function calculateBorrowRewardsForUser(
912:    function calculateNewIndex(
976:    function fetchConfigByEmissionToken(
1001:    function updateMarketSupplyIndexInternal(MToken _mToken) internal {
1041:    function disburseSupplierRewardsInternal(
1101:    function updateMarketBorrowIndexInternal(MToken _mToken) internal {
1147:    function disburseBorrowerRewardsInternal(
1214:    function sendReward(
```

---

### Version [5]

- Pragma versions should be standardized and avoid floating pragma `( ^ )`.

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

```solidity
// lock this by removing the ^ for a safer version.
2:  pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol

```solidity
// lock this by removing the ^ for a safer version.
2:  pragma solidity ^0.8.0;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

```solidity
// lock this by removing the ^ for a safer version.
2:  pragma solidity ^0.8.0;
```

---

### Observations [6]

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

```solidity
// remove this test
4:  import "@forge-std/Test.sol";
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

```solidity
61:    function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {
82:    function mint(uint mintAmount) override external returns (uint) {
97:    function mintWithPermit(
117:    function redeem(uint redeemTokens) override external returns (uint) {
128:    function redeemUnderlying(uint redeemAmount) override external returns (uint) {
138:    function borrow(uint borrowAmount) override external returns (uint) {
148:    function repayBorrow(uint repayAmount) override external returns (uint) {
159:    function repayBorrowBehalf(address borrower, uint repayAmount) override external returns (uint) {
172:    function liquidateBorrow(address borrower, uint repayAmount, MTokenInterface mTokenCollateral) override external returns (uint) {
183:    function transfer(address dst, uint amount) override external returns (bool) {
195:    function transferFrom(address src, address dst, uint256 amount) override external returns (bool) {
208:    function approve(address spender, uint256 amount) override external returns (bool) {
219:    function allowance(address owner, address spender) override external view returns (uint) {
229:    function balanceOf(address owner) override external view returns (uint) {
240:    function balanceOfUnderlying(address owner) override external returns (uint) {
251:    function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {
260:    function borrowRatePerTimestamp() override external view returns (uint) {
269:    function supplyRatePerTimestamp() override external view returns (uint) {
278:    function totalBorrowsCurrent() override external returns (uint) {
288:    function borrowBalanceCurrent(address account) override external returns (uint) {
298:    function borrowBalanceStored(address account) override public view returns (uint) {
307:    function exchangeRateCurrent() override public returns (uint) {
317:    function exchangeRateStored() override public view returns (uint) {
326:    function getCash() override external view returns (uint) {
336:    function accrueInterest() override public returns (uint) {
350:    function seize(address liquidator, address borrower, uint seizeTokens) override external returns (uint) {
359:    function sweepToken(EIP20NonStandardInterface token) override external {
372:    function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {
382:    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {
392:    function _setReserveFactor(uint newReserveFactorMantissa) override external returns (uint) {
402:    function _acceptAdmin() override external returns (uint) {
412:    function _addReserves(uint addAmount) override external returns (uint) {
422:    function _reduceReserves(uint reduceAmount) override external returns (uint) {
433:    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {
443:    function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol

```solidity
136:    function transfer(address dst, uint256 amount) override external nonReentrant returns (bool) {
147    function transferFrom(address src, address dst, uint256 amount) override external nonReentrant returns (bool) {
159:    function approve(address spender, uint256 amount) override external returns (bool) {
172:    function allowance(address owner, address spender) override external view returns (uint256) {
191:    function balanceOfUnderlying(address owner) override external returns (uint) {
204:    function getAccountSnapshot(address account) override external view returns (uint, uint, uint, uint) {
236:    function borrowRatePerTimestamp() override external view returns (uint) {
244:    function supplyRatePerTimestamp() override external view returns (uint) {
252:    function totalBorrowsCurrent() override external nonReentrant returns (uint) {
262:    function borrowBalanceCurrent(address account) override external nonReentrant returns (uint) {
272:    function borrowBalanceStored(address account) override public view returns (uint) {
319:    function exchangeRateCurrent() override public nonReentrant returns (uint) {
329:    function exchangeRateStored() override public view returns (uint) {
376:    function getCash() override external view returns (uint) {
385:    function accrueInterest() virtual override public returns (uint) {
1048:    function seize(address liquidator, address borrower, uint seizeTokens) override external nonReentrant returns (uint) {
1145:    function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {
1168:    function _acceptAdmin() override external returns (uint) {
1195:    function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {
1219:    function _setReserveFactor(uint newReserveFactorMantissa) override external nonReentrant returns (uint) {
1326:    function _reduceReserves(uint reduceAmount) override external nonReentrant returns (uint) {
1391:    function _setInterestRateModel(InterestRateModel newInterestRateModel) override public returns (uint) {
1443:    function _setProtocolSeizeShare(uint newProtocolSeizeShareMantissa) override external nonReentrant returns (uint) {
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol

```solidity
// place the override after the function's visibility, not before
102:    function enterMarkets(address[] memory mTokens) override public returns (uint[] memory) {
154:    function exitMarket(address mTokenAddress) override external returns (uint) {
215:    function mintAllowed(address mToken, address minter, uint mintAmount) override external returns (uint) {
251:    function redeemAllowed(address mToken, address redeemer, uint redeemTokens) override external returns (uint) {
292:    function redeemVerify(address mToken, address redeemer, uint redeemAmount, uint redeemTokens) override pure external {
310:    function borrowAllowed(address mToken, address borrower, uint borrowAmount) override external returns (uint) {
366:    function repayBorrowAllowed(
394:    function liquidateBorrowAllowed(
434:    function seizeAllowed(
472:    function transferAllowed(address mToken, address src, address dst, uint transferTokens) override external returns (uint) {
629:    function liquidateCalculateSeizeTokens(address mTokenBorrowed, address mTokenCollateral, uint actualRepayAmount) override external view returns (uint, uint) {
```

