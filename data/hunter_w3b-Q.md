## [L-01] Possible reentrancy with callback on transfer tokens

The following functions don't apply the CEI pattern. It's possible to reenter after the transfer if the token has some kind of callback functionality (e.g. ERC777/ERC1155).

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L477-L484

```solidity
   if (_amount == type(uint256).max) {
            token.safeTransfer(
                comptroller.admin(),
                token.balanceOf(address(this))
            );
        } else {
            token.safeTransfer(comptroller.admin(), _amount);
        }
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L1235-L1238

```solidity
        if (_amount > 0 && _amount <= currentTokenHoldings) {
            // Ensure we use SafeERC20 to revert even if the reward token isn't ERC20 compliant
            token.safeTransfer(_user, _amount);
            return 0;
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L36-L39

```solidity
  IERC20(address(mToken)).safeTransfer(
            recipient,
            mToken.balanceOf(address(this))
        );
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L46-L50

```solidity
        IERC20(address(mToken)).safeTransferFrom(
            msg.sender,
            address(this),
            mTokenRedeemAmount
        );
```

## [L-02] Large transfers may not work with some `ERC20` tokens

Some IERC20 implementations (e.g UNI, COMP) may fail if the valued transferred is larger than uint96. [Source](https://github.com/d-xo/weird-erc20#revert-on-large-approvals--transfers)

Some IERC20 implementations may fail if the value transferred is larger than uint96. Such scenarios can lead to unexpected behavior or contract failures. It is essential to address this issue to ensure the contract functions as intended with different ERC20 tokens.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L36-L39

```solidity
  IERC20(address(mToken)).safeTransfer(
            recipient,
            mToken.balanceOf(address(this))
        );
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L46-L50

```solidity
  IERC20(address(mToken)).safeTransferFrom(
            msg.sender,
            address(this),
            mTokenRedeemAmount
        );
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L962-L968

```solidity
 IERC20 token = IERC20(_tokenAddress);
        // Similar to mTokens, if this is uint.max that means "transfer everything"
        if (_amount == type(uint).max) {
            token.transfer(admin, token.balanceOf(address(this)));
        } else {
            token.transfer(admin, _amount);
        }
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L475-L484

```solidity
     IERC20 token = IERC20(_tokenAddress);
        // Similar to mTokens, if this is uint256.max that means "transfer everything"
        if (_amount == type(uint256).max) {
            token.safeTransfer(
                comptroller.admin(),
                token.balanceOf(address(this))
            );
        } else {
            token.safeTransfer(comptroller.admin(), _amount);
        }
```

safeTransfer and transfer functions are used without checks for large transfer amounts.

If the transfer amount exceeds uint96, the transfer may fail, leading to unexpected behavior or contract errors.

## [L-03] Return values of `approve` not checked

By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L26

```solidity
    _weth.approve(address(_mToken), type(uint256).max);
```

## [L-04] Missing Reward `Distributor` Address Check

In the `Comptroller.sol::claimReward` function, the code checks whether the `rewardDistributor` contract address is not equal to zero (address(0)) before proceeding with the reward distribution. However, this check only ensures that the `rewardDistributor` is set, but it does not verify whether the contract address points to a trusted RewardDistributor contract.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L1028-L1053

```solidity
function claimReward(address[] memory holders, MToken[] memory mTokens, bool borrowers, bool suppliers) public {
        require(address(rewardDistributor) != address(0), "No reward distributor configured!");

        for (uint i = 0; i < mTokens.length; i++) {

            // Safety check that the supplied mTokens are active/listed
            MToken mToken = mTokens[i];
            require(markets[address(mToken)].isListed, "market must be listed");

            // Disburse supply side
            if (suppliers == true) {
                rewardDistributor.updateMarketSupplyIndex(mToken);
                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
                    rewardDistributor.disburseSupplierRewards(mToken, holders[holderIndex], true);
                }
            }

            // Disburse borrow side
            if (borrowers == true) {
                rewardDistributor.updateMarketBorrowIndex(mToken);
                for (uint holderIndex = 0; holderIndex < holders.length; holderIndex++) {
                    rewardDistributor.disburseBorrowerRewards(mToken, holders[holderIndex], true);
                }
            }
        }
    }

```

If the rewardDistributor address is set to an untrusted or malicious contract, it could lead to the distribution of rewards to unauthorized addresses or result in funds being sent to an attacker-controlled contract.

## [L-05] External calls in an `unbounded` loop can result in a DoS

Consider limiting the number of iterations in loops that make external calls, as just a single one of them failing will result in a revert.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Comptroller.sol#L578

```solidity
        for (uint i = 0; i < assets.length; i++) {
            MToken asset = assets[i];

            // Read the balances and exchange rate from the mToken
            (oErr, vars.mTokenBalance, vars.borrowBalance, vars.exchangeRateMantissa) = asset.getAccountSnapshot(account);
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L170-L233

```solidity

            for (uint256 i = 0; i < cTokenConfigsLength; i++) {
                Configs.CTokenConfiguration memory config = cTokenConfigs[i];

                /// ----- Jump Rate IRM -------
                {
                    address irModel = address(
                        new JumpRateModel(
                            config.jrm.baseRatePerYear,
                            config.jrm.multiplierPerYear,
                            config.jrm.jumpMultiplierPerYear,
                            config.jrm.kink
                        )
                    );

                    addresses.addAddress( // @audit external call
                        string(
                            abi.encodePacked(
                                "JUMP_RATE_IRM_",
                                config.addressesString
                            )
                        ),
                        address(irModel)
                    );
                }

                /// stack isn't too deep
                CTokenAddresses memory addr = CTokenAddresses({
                    mTokenImpl: addresses.getAddress("MTOKEN_IMPLEMENTATION"),  // @audit external call
                    irModel: addresses.getAddress( // @audit external call
                        string(
                            abi.encodePacked(
                                "JUMP_RATE_IRM_",
                                config.addressesString
                            )
                        )
                    ),
                    temporalGov: addresses.getAddress("TEMPORAL_GOVERNOR"),  // @audit external call
                    unitroller: addresses.getAddress("UNITROLLER") // @audit external call
                });

                /// TODO calculate initial exchange rate
                /// BigNumber.from("10").pow(token.decimals + 8).mul("2");
                /// (10 ** (18 + 8)) * 2 // 18 decimals example
                ///    = 2e26
                /// (10 ** (6 + 8)) * 2 // 6 decimals example
                ///    = 2e14
                uint256 initialExchangeRate = (10 **
                    (ERC20(config.tokenAddress).decimals() + 8)) * 2;  // @audit external call

                MErc20Delegator mToken = new MErc20Delegator(
                    config.tokenAddress,
                    ComptrollerInterface(addr.unitroller),
                    InterestRateModel(addr.irModel),
                    initialExchangeRate,
                    config.name,
                    config.symbol,
                    mTokenDecimals,
                    payable(addr.temporalGov),
                    addr.mTokenImpl,
                    ""
                );

                addresses.addAddress(config.addressesString, address(mToken));  // @audit external call
            }
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L275-L300

```solidity

            for (uint256 i = 0; i < cTokenConfigs.length; i++) {
                Configs.CTokenConfiguration memory config = cTokenConfigs[i];
                supplyCaps[i] = config.supplyCap;
                borrowCaps[i] = config.borrowCap;

                oracle.setFeed( // @audit external call
                    ERC20(config.tokenAddress).symbol(),  // @audit external call
                    config.priceFeed
                );

                /// list both mUSDC and mETH in the comptroller
                Comptroller(address(unitroller))._supportMarket(    // @audit external call
                    MToken(addresses.getAddress(config.addressesString))    // @audit external call
                );

                /// set mint paused for all MTokens
                Comptroller(address(unitroller))._setMintPaused(    // @audit external call
                    MToken(addresses.getAddress(config.addressesString)),  // @audit external call
                    true
                );

                /// get the mToken
                mTokens[i] = MToken(
                    addresses.getAddress(config.addressesString)  // @audit external call
                );
            }
```

https://github.com/code-423n4/2023-07-moonwell/blob/main/test/proposals/mips/mip00.sol#L706-L785

```solidity
             for (uint256 i = 0; i < cTokenConfigs.length; i++) {
                    Configs.CTokenConfiguration memory config = cTokenConfigs[
                        i
                    ];

                    /// CToken Assertions
                    assertFalse(
                        comptroller.mintGuardianPaused(
                            addresses.getAddress(config.addressesString)
                        )
                    );
                    assertEq(
                        comptroller.borrowCaps(
                            addresses.getAddress(config.addressesString)
                        ),
                        config.borrowCap
                    );
                    assertEq(
                        comptroller.supplyCaps(
                            addresses.getAddress(config.addressesString)
                        ),
                        config.supplyCap
                    );
                    assertEq(
                        comptroller.supplyCaps(
                            addresses.getAddress(config.addressesString)
                        ),
                        config.supplyCap
                    );

                    /// assert cToken irModel is correct

                    assertEq(
                        address(
                            MToken(addresses.getAddress(config.addressesString))
                                .interestRateModel()
                        ),
                        addresses.getAddress(
                            string(
                                abi.encodePacked(
                                    "JUMP_RATE_IRM_",
                                    config.addressesString
                                )
                            )
                        )
                    );

                    /// Jump Rate Model Assertions
                    {
                        JumpRateModel jrm = JumpRateModel(
                            addresses.getAddress(
                                string(
                                    abi.encodePacked(
                                        "JUMP_RATE_IRM_",
                                        config.addressesString
                                    )
                                )
                            )
                        );

                        assertEq(
                            jrm.baseRatePerTimestamp(),
                            (config.jrm.baseRatePerYear * 1e18) /
                                jrm.timestampsPerYear() /
                                1e18
                        );
                        assertEq(
                            jrm.multiplierPerTimestamp(),
                            (config.jrm.multiplierPerYear * 1e18) /
                                jrm.timestampsPerYear() /
                                1e18
                        );
                        assertEq(
                            jrm.jumpMultiplierPerTimestamp(),
                            (config.jrm.jumpMultiplierPerYear * 1e18) /
                                jrm.timestampsPerYear() /
                                1e18
                        );
                        assertEq(jrm.kink(), config.jrm.kink);
                    }
                }
```

## [L-06] Use `safeApprove` instead of approve

The approve function in ERC20 tokens is not safe to use. It is possible for an attacker to front-run the transaction and steal the tokens. Use safeApprove instead.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L26

```solidity
        _weth.approve(address(_mToken), type(uint256).max);

```

## [L-07] Condition will not revert when `block.timestamp` is == to the compared variable

The condition does not revert when block.timestamp is equal to the compared > or < variable. For example, if there is a check for block.timestamp > identifier then there should be a check for cases where block.timestamp is == to identifier

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L409-L413

```solidity
  require(
            _endTime > block.timestamp + 1,
            "The _endTime parameter must be in the future!"
        );

```

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L793-L796

```solidity
  require(
            _newEndTime > block.timestamp,
            "_newEndTime MUST be > block.timestamp"
        );
```

## [L-08] Excess Ether Handling Issue in mint Function

The mint function in the WETHRouter contract is marked as payable, allowing it to receive Ether as part of the transaction. The function is responsible for minting `mToken` by converting the received Ether into the desired token using the Wrapped Ether (WETH) contract.

It does not handle any excess Ether sent by the caller. If the caller mistakenly sends more Ether than required for minting the mToken, the surplus Ether will be lost.

The issue can result in unintended loss of Ether for users who mistakenly send more Ether than required for minting the mToken. The contract does not refund the excess Ether back to the sender.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/router/WETHRouter.sol#L31-L40

```solidity
   function mint(address recipient) external payable {
        weth.deposit{value: msg.value}();

        require(mToken.mint(msg.value) == 0, "WETHRouter: mint failed");

        IERC20(address(mToken)).safeTransfer(
            recipient,
            mToken.balanceOf(address(this))
        );
    }
```

## [L-09] Risky `pendingAdmin` Check

The code checks if the caller is `pendingAdmin` and also ensures that `pendingAdmin` is not equal to address(0). However, there is a potential problem with this check. If `pendingAdmin` is a valid contract address but the contract is not yet deployed, it will return false for the equality check msg.sender == address(0). This means that the condition `msg.sender != pendingAdmin || msg.sender == address(0)` will evaluate to true, which could lead to unexpected behavior.

If the contract's `pendingAdmin` variable is set to a contract address that has not been deployed yet, an attacker could bypass the pendingAdmin check and take control of the admin role by calling `_acceptAdmin()` from an address other than pendingAdmin.

https://github.com/code-423n4/2023-07-moonwell/blob/main/src/core/Unitroller.sol#L109-L130

```solidity

   function _acceptAdmin() public returns (uint) {
        // Check caller is pendingAdmin and pendingAdmin ≠ address(0)
        if (msg.sender != pendingAdmin || msg.sender == address(0)) {
            return fail(Error.UNAUTHORIZED, FailureInfo.ACCEPT_ADMIN_PENDING_ADMIN_CHECK);
        }

        // Save current values for inclusion in log
        address oldAdmin = admin;
        address oldPendingAdmin = pendingAdmin;

        // Store admin with value pendingAdmin
        admin = pendingAdmin;

        // Clear the pending value
        pendingAdmin = address(0);

        emit NewAdmin(oldAdmin, admin);
        emit NewPendingAdmin(oldPendingAdmin, pendingAdmin);

        return uint(Error.NO_ERROR);
    }

```

### Recommendation

it is essential to add an additional check to verify if pendingAdmin is a valid contract address. One way to achieve this is by using the isContract function to check if pendingAdmin is a deployed contract address.
