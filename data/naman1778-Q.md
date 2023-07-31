## [N-01] Use a modifier for access control

Consider using a modifier to implement access control instead of inlining the condition/requirement in the function’s body.

    File: src/core/Comptroller.sol	

    665: function _setPriceOracle(PriceOracle newOracle) public returns (uint) {
    666:     // Check caller is admin
    667:     if (msg.sender != admin) {
    668:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_PRICE_ORACLE_OWNER_CHECK);
    669:     }

    689: function _setCloseFactor(uint newCloseFactorMantissa) external returns (uint) {
    690:     // Check caller is admin
    691: 	require(msg.sender == admin, "only admin can set close factor");

    707: function _setCollateralFactor(MToken mToken, uint newCollateralFactorMantissa) external returns (uint) {
    708:     // Check caller is admin
    709:     if (msg.sender != admin) {
    710:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_COLLATERAL_FACTOR_OWNER_CHECK);
    711:     }

    748: function _setLiquidationIncentive(uint newLiquidationIncentiveMantissa) external returns (uint) {
    749:     // Check caller is admin
    750:     if (msg.sender != admin) {
    751:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_LIQUIDATION_INCENTIVE_OWNER_CHECK);
    752:     }

    772: function _supportMarket(MToken mToken) external returns (uint) {
    773:     if (msg.sender != admin) {
    774:         return fail(Error.UNAUTHORIZED, FailureInfo.SUPPORT_MARKET_OWNER_CHECK);
    775:     }

    807: function _setMarketBorrowCaps(MToken[] calldata mTokens, uint[] calldata newBorrowCaps) external {
    808: 	require(msg.sender == admin || msg.sender == borrowCapGuardian, "only admin or borrow cap guardian can set borrow caps");

    825: function _setBorrowCapGuardian(address newBorrowCapGuardian) external {
    826:     require(msg.sender == admin, "only admin can set borrow cap guardian");

    844: function _setMarketSupplyCaps(MToken[] calldata mTokens, uint[] calldata newSupplyCaps) external {
    845:     require(msg.sender == admin || msg.sender == supplyCapGuardian, "only admin or supply cap guardian can set supply caps");

    862: function _setSupplyCapGuardian(address newSupplyCapGuardian) external {
    863:     require(msg.sender == admin, "only admin can set supply cap guardian");

    880: function _setPauseGuardian(address newPauseGuardian) public returns (uint) {
    881:     if (msg.sender != admin) {

    901: function _setRewardDistributor(MultiRewardDistributor newRewardDistributor) public {
    902:     require(msg.sender == admin, "Unauthorized");

    959: function _rescueFunds(address _tokenAddress, uint _amount) external {
    960:     require(msg.sender == admin, "Unauthorized");

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol

    File: src/core/Unitroller.sol	

    39: function _setPendingImplementation(address newPendingImplementation) public returns (uint) {
    40: 
    41:     if (msg.sender != admin) {
    42:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_PENDING_IMPLEMENTATION_OWNER_CHECK);
    43:     }

    86: function _setPendingAdmin(address newPendingAdmin) public returns (uint) {
    87:     // Check caller = admin
    88:     if (msg.sender != admin) {
    89:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_PENDING_ADMIN_OWNER_CHECK);
    90:     }

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol

    File: src/core/Governance/TemporalGovernor.sol	

    137: function setTrustedSenders(
    138:     TrustedSender[] calldata _trustedSenders
    139: ) external {
    140:     require(
    141:         msg.sender == address(this),
    142:         "TemporalGovernor: Only this contract can update trusted senders"
    143:     );

    164: function unSetTrustedSenders(
    165:     TrustedSender[] calldata _trustedSenders
    166: ) external {
    167:     require(
    168:         msg.sender == address(this),
    169:         "TemporalGovernor: Only this contract can update trusted senders"
    170:     );

    188: function grantGuardiansPause() external {
    189:     require(
    190:         msg.sender == address(this),
    191:         "TemporalGovernor: Only this contract can update grant guardian pause"
    192:     );

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

    File: src/core/MToken.sol	

    26: function initialize(ComptrollerInterface comptroller_,
    27:                     InterestRateModel interestRateModel_,
    28:                     uint initialExchangeRateMantissa_,
    29:                     string memory name_,
    30:                     string memory symbol_,
    31:                     uint8 decimals_) public {
    32:     require(msg.sender == admin, "only admin may initialize the market");

    1145: function _setPendingAdmin(address payable newPendingAdmin) override external returns (uint) {
    1146:     // Check caller = admin
    1147:     if (msg.sender != admin) {
    1148:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_PENDING_ADMIN_OWNER_CHECK);
    1149:     }

    1168: function _acceptAdmin() override external returns (uint) {
    1169:     // Check caller is pendingAdmin and pendingAdmin ≠ address(0)
    1170:     if (msg.sender != pendingAdmin || msg.sender == address(0)) {
    1171:         return fail(Error.UNAUTHORIZED, FailureInfo.ACCEPT_ADMIN_PENDING_ADMIN_CHECK);
    1172:     }

    1195: function _setComptroller(ComptrollerInterface newComptroller) override public returns (uint) {
    1196:     // Check caller is admin
    1197:     if (msg.sender != admin) {
    1198:         return fail(Error.UNAUTHORIZED, FailureInfo.SET_COMPTROLLER_OWNER_CHECK);
    1199:     }

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MToken.sol

    File: src/core/MErc20.sol	

    148: function sweepToken(EIP20NonStandardInterface token) override external {
    149:     require(msg.sender == admin, "MErc20::sweepToken: only admin can sweep tokens");

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20.sol

    File: src/core/MErc20Delegator.sol	

    61: function _setImplementation(address implementation_, bool allowResign, bytes memory becomeImplementationData) override public {
    62:     require(msg.sender == admin, "MErc20Delegator::_setImplementation: Caller must be admin");

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

## [N-02] Contract files should define a locked compiler version

Contracts should be deployed with the same compiler version and flags that they have been tested with thoroughly. Locking the pragma helps to ensure that contracts do not accidentally get deployed using, for example, an outdated compiler version that might introduce bugs that affect the contract system negatively.

    File: src/core/Governance/TemporalGovernor.sol	

    2: pragma solidity ^0.8.0;

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

    File: src/core/Oracles/ChainlinkCompositeOracle.sol	

    2: pragma solidity ^0.8.0;

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol

    File: test/proposals/mips/mip00.sol	

    2: pragma solidity ^0.8.0;

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

## [N-03] Lack of address(0) checks in the constructor

Zero-address check should be used in the constructors, to avoid the risk of setting smth as address(0) at deploying time.

    File: src/core/Governance/TemporalGovernor.sol	

    61: constructor(

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Governance/TemporalGovernor.sol

    File: src/core/MErc20Delegator.sol	

    25: constructor(address underlying_,

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MErc20Delegator.sol

    File: src/core/Oracles/ChainlinkCompositeOracle.sol	

    35: constructor(

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Oracles/ChainlinkCompositeOracle.sol

## [N-04] Empty/Unused Function Parameters

Empty or unused function parameters should be commented out as a better and declarative way to silence runtime warning messages. As an example, the following function may have these parameters refactored to:

    File: test/proposals/mips/mip00.sol	

    77: function deploy(Addresses addresses, address) public {

    249: function afterDeploy(Addresses addresses, address) public {

    462: function run(Addresses addresses, address) public {

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol

