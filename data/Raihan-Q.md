# low severity 

# Summary
|      | issue | instance |
|------|-------|----------|
|[L-01]| Use of abi.encodePacked with dynamic types inside keccak256|3|
|[L-02]| Missing Event for initialize|3|
|[L-03]| Contracts are not using their OZ upgradeable counterparts|14|
|[L-04]| Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when casting from various type int/uint values|2|
|[L-05]| Some tokens may revert when zero value transfers are made|2|
|[L-06]| Signature use at deadlines should be allowed|1|
|[L-07]| Missing checks for address(0x0) when assigning values to address state variables|1|
|[L-08]| Missing contract-existence checks before low-level calls|2|
|[L-09]| Gas griefing/theft is possible on unsafe external call|1|
|[L-10]| Unbounded loop|3|
|[L-11]| Use safeTransferOwnership instead of transferOwnership function|1|
|[L-12]| Missing checks for approve()’s return status|1|

# Detials

## [L-01] Use of abi.encodePacked with dynamic types inside keccak256
abi.encodePacked should not be used with dynamic types when passing the result to a hash function such as keccak256. Use abi.encode instead, which will pad items to 32 bytes, to [prevent any hash collisions](https://docs.soliditylang.org/en/latest/abi-spec.html#non-standard-packed-mode).

```solidity
File: src/core/Oracles/ChainlinkOracle.sol
46         nativeToken = keccak256(abi.encodePacked(_nativeToken));

150        feeds[keccak256(abi.encodePacked(symbol))] = AggregatorV3Interface(

161        return feeds[keccak256(abi.encodePacked(symbol))];
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkOracle.sol#L46

## [L-02] Missing Event for initialize
Description: Events help non-contract tools to track changes, and events prevent users from being surprised by changes Issuing event-emit during initialization is a detail that many projects skip

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol
88    function initialize(
        address _comptroller,
        address _pauseGuardian
    ) external initializer {
        // Sanity check the params
        require(
            _comptroller != address(0),
            "Comptroller can't be the 0 address!"
        );
        require(
            _pauseGuardian != address(0),
            "Pause Guardian can't be the 0 address!"
        );

        comptroller = Comptroller(payable(_comptroller));

        require(
            comptroller.isComptroller(),
            "Can't bind to something that's not a comptroller!"
        );

        pauseGuardian = _pauseGuardian;
        emissionCap = 100e18;
    }
```    
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L88-L111


```solidity
File: src/core/MErc20.sol
23  function initialize(address underlying_,
                        ComptrollerInterface comptroller_,
                        InterestRateModel interestRateModel_,
                        uint initialExchangeRateMantissa_,
                        string memory name_,
                        string memory symbol_,
                        uint8 decimals_) public {
        // MToken initialize does the bulk of the work
        super.initialize(comptroller_, interestRateModel_, initialExchangeRateMantissa_, name_, symbol_, decimals_);

        // Set underlying and sanity check it
        underlying = underlying_;
        EIP20Interface(underlying).totalSupply();
    }
```    
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L36

```solidity
File: src/core/MToken.sol
26  function initialize(ComptrollerInterface comptroller_,
                        InterestRateModel interestRateModel_,
                        uint initialExchangeRateMantissa_,
                        string memory name_,
                        string memory symbol_,
                        uint8 decimals_) public {
        require(msg.sender == admin, "only admin may initialize the market");
        require(accrualBlockTimestamp == 0 && borrowIndex == 0, "market may only be initialized once");

        // Set initial exchange rate
        initialExchangeRateMantissa = initialExchangeRateMantissa_;
        require(initialExchangeRateMantissa > 0, "initial exchange rate must be greater than zero.");

        // Set the comptroller
        uint err = _setComptroller(comptroller_);
        require(err == uint(Error.NO_ERROR), "setting comptroller failed");

        // Initialize block timestamp and borrow index (block timestamp mocks depend on comptroller being set)
        accrualBlockTimestamp = getBlockTimestamp();
        borrowIndex = mantissaOne;

        // Set the interest rate model (depends on block timestamp / borrow index)
        err = _setInterestRateModelFresh(interestRateModel_);
        require(err == uint(Error.NO_ERROR), "setting interest rate model failed");

        name = name_;
        symbol = symbol_;
        decimals = decimals_;

        // The counter starts true to prevent changing it from zero to non-zero (i.e. smaller cost/refund)
        _notEntered = true;
    }
```    
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MToken.sol#L26-L57

Recommendation: Add Event-Emit


## [L-03] Contracts are not using their OZ upgradeable counterparts
Description
The non-upgradeable standard version of OpenZeppelin's library, such as `Ownable`, `Pausable`, `Address`, `Context`, `SafeERC20`, `ERC1967Upgrade` etc, are inherited / used by both the proxy and the implementation contracts.

As a result, when attempting to use the upgrades plugin mentioned, the following errors are raised:

```
Error: Contract `FiatTokenV1` is not upgrade safe

contracts/v1/FiatTokenV1.sol:58: Variable `totalSupply_` is assigned an initial value
  Move the assignment to the initializer
  https://zpl.in/upgrades/error-004

contracts/v1/Pausable.sol:49: Variable `paused` is assigned an initial value
  Move the assignment to the initializer
  https://zpl.in/upgrades/error-004

contracts/v1/Ownable.sol:28: Contract `Ownable` has a constructor
  Define an initializer instead
  https://zpl.in/upgrades/error-001

contracts/util/Address.sol:186: Use of delegatecall is not allowed
  https://zpl.in/upgrades/error-002
       
```
Having reviewed these errors, none had any adversarial impact:

`totalSupply_` and `paused` are explictly assigned the default values `0` and `false`
the implementation contracts utilises the internal `_transferOwnership()` in the initializer, thus transferring ownership to `newOwner` regardless of who the current owner is
`Address's` `delegatecall` is only used by the `ERC1967Upgrade` contract. Comparing both the `Address` and `ERC1967Upgrade` contracts against their upgradeable counterparts show similar behaviour (differences are some refactoring done to shift the delegatecall into the `ERC1967Upgrade` contract).
Nevertheless, it would be safer to use the upgradeable versions of the library contracts to avoid unexpected behaviour.

note : this is missed from bots
```solidity
File: src/core/MErc20.sol
import "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20.sol#L4

```solidity
File: src/core/Governance/TemporalGovernor.sol
import {EnumerableSet} from "@openzeppelin/contracts/utils/structs/EnumerableSet.sol";
import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
import {Pausable} from "@openzeppelin/contracts/security/Pausable.sol";
import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L4-L7


```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol
import {SafeCast} from "@openzeppelin/contracts/utils/math/SafeCast.sol";
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L4

```solidity
File: src/core/router/WETHRouter.sol
import {SafeERC20} from "@openzeppelin/contracts/token/ERC20/utils/SafeERC20.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L3-L4

```solidity
File: test/proposals/mips/mip00.sol
import {TransparentUpgradeableProxy, ITransparentUpgradeableProxy} from "@openzeppelin/contracts/proxy/transparent/TransparentUpgradeableProxy.sol";
import {TimelockController} from "@openzeppelin/contracts/governance/TimelockController.sol";
import {ProxyAdmin} from "@openzeppelin/contracts/proxy/transparent/ProxyAdmin.sol";
import {IVotes} from "@openzeppelin/contracts/governance/extensions/GovernorVotes.sol";
import {IERC20} from "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/test/proposals/mips/mip00.sol#L6-L11

### Recommended Mitigation Steps
Where applicable, use the contracts from `@openzeppelin/contracts-upgradeable` instead of `@openzeppelin/contracts`.


## [L-04] Consider using OpenZeppelin's SafeCast library to prevent unexpected overflows when casting from various type int/uint values


```solidity
File: src/core/Oracles/ChainlinkCompositeOracle.sol
113        int256 scalingFactor = int256(10 ** uint256(expectedDecimals)); /// calculate expected decimals for end quote

145        int256 scalingFactor = int256(10 ** uint256(expectedDecimals * 2)); /// calculate expected decimals for end quote

```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Oracles/ChainlinkCompositeOracle.sol#L113


## [L-05] Some tokens may revert when zero value transfers are made
In spite of the fact that EIP-20 [states](https://github.com/ethereum/EIPs/blob/46b9b698815abbfa628cd1097311deee77dd45c5/EIPS/eip-20.md?plain=1#L116) that zero-valued transfers must be accepted, some tokens, such as LEND will revert if this is attempted, which may cause transactions that involve other tokens (such as batch operations) to fully revert. Consider skipping the transfer if the amount is zero, which will also save gas.
```solidity
File: src/core/router/WETHRouter.sol
46     IERC20(address(mToken)).safeTransferFrom(
            msg.sender,
            address(this),
            mTokenRedeemAmount
        );
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L46

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol
478      token.safeTransfer(
                comptroller.admin(),
                token.balanceOf(address(this))
            );
483      token.safeTransfer(comptroller.admin(), _amount);
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L478

## [L‑06] Signature use at deadlines should be allowed
According to EIP-2612, signatures used on exactly the deadline timestamp are supposed to be allowed. While the signature may or may not be used for the exact EIP-2612 use case (transfer approvals), for consistency's sake, all deadlines should follow this semantic. If the timestamp is an expiration rather than a deadline, consider whether it makes more sense to include the expiration timestamp as a valid timestamp, as is done for deadlines.


```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol
409  require(
            _endTime > block.timestamp + 1,
            "The _endTime parameter must be in the future!"
        );
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L409-412



## [L-07] Missing checks for address(0x0) when assigning values to address state variables

note : this is missed from bots
```solidity
File: src/core/MErc20Delegator.sol
52        admin = admin_;
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L52

## [L-08] Missing contract-existence checks before low-level calls
Low-level calls return success if there is no code present at the specified address. In addition to the zero-address checks, add a check to verify that <address>.code.length > 0

```solidity
File: src/core/Governance/TemporalGovernor.sol
400      (bool success, bytes memory returnData) = target.call{value: value}(
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Governance/TemporalGovernor.sol#L400

```solidity
File: src/core/router/WETHRouter.sol
59        (bool success, ) = payable(recipient).call{
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L59


## [L-09] Gas griefing/theft is possible on unsafe external call
return data (bool success,) has to be stored due to EVM architecture, if in a usage like below, 'out' and 'outsize' values are given (0,0) . Thus, this storage disappears and may come from external contracts a possible Gas griefing/theft problem is avoided

```solidity
File: src/core/router/WETHRouter.sol
59        (bool success, ) = payable(recipient).call{
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/router/WETHRouter.sol#L59

## [L-10] Unbounded loop
New items are pushed into the following arrays but there is no option to pop them out. Currently, the array can grow indefinitely. E.g. there’s no maximum limit and there’s no functionality to remove array values.
If the array grows too large, calling relevant functions might run out of gas and revert. Calling these functions could result in a DOS condition.

```solidity
File: src/core/Comptroller.sol
140        accountAssets[borrower].push(mToken);

798        allMarkets.push(MToken(mToken));
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/Comptroller.sol#L140

```solidity
File: src/core/MultiRewardDistributor/MultiRewardDistributor.sol
462       MarketEmissionConfig storage newConfig = configs.push();
          newConfig.config = config;
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L462-L463

## [L-11] Use safeTransferOwnership instead of transferOwnership function
```solidity
File: test/proposals/mips/mip00.sol
260        proxyAdmin.transferOwnership(governor);
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/test/proposals/mips/mip00.sol#L260

## [L-12] Missing checks for approve()’s return status
Some tokens, such as Tether (USDT) return false rather than reverting if the approval fails. Use OpenZeppelin’s safeApprove(), which reverts if there’s a failure, instead

```solidity
File: src/core/MErc20Delegator.sol
26        _weth.approve(address(_mToken), type(uint256).max);
```
https://github.com/code-423n4/2023-07-moonwell/blob/4aaa7d6767da3bc42e31c18ea2e75736a4ea53d4/src/core/MErc20Delegator.sol#L26