### Low risk issues
| |Issue|Instances|
|-|:-|:-:|
| [L&#x2011;01] | The reserveFactor and the seizeShare of all mTokens are not set in mip00  | 1 | 
| [L&#x2011;02] | The consistency level of a VAA is not checked in the TemporalGovernor  | 1 |  
| [L&#x2011;03] | ChainlinkOracle will return the wrong price for asset if underlying aggregator hits minAnswer | 1 | 
| [L&#x2011;04] | revokeGuardian() should transfer ownership to an emergency address in case wormhole starts sending malicious messages | 1 |
| [L&#x2011;05] | No zero address check can lead to loss of funds when redeeming in WETHRouter | 1 | 
| [L&#x2011;06] | Wrong operator is used when checking nextTotalSupplies/nextTotalBorrows | 2 | 
| [L&#x2011;07] | The intendedRecipient is not checked when fastTrackProposalExecution() is used  | 1 | 
| [L&#x2011;08] | Missing assertion of collateralFactor of all mTokens in mip00  | 1 |  

Total: 9 instances over 8 issues 

## [L&#x2011;01] The reserveFactor and the seizeShare of all mTokens are not set in mip00

The mip00 contract handles deployment and parameterization of the initial system. All important parameters for mTokens are set, however 2 things are missing: the reserveFactor and the seizeShare. 


In Config.sol all mTokens have the reserveFactor and the seizeShare defined, however they are not being set in mip00. If they are not set then the protocol will have 0 reserves


### Impact
The mip00 ensures that everything is ready for operations to begin and after it users can start using the protocol. Everything is supposed to be set so if the reserveFactor and the seizeShare are not set and the team doesnt notice then it is a problem. 

The reserveFactor and the seizeShare are both crucial for the protocol because without them Moonwell will fail to generate any reserves. 



### Code Snippet

As you can see in Configs.sol, all mTokens have a reserveFactor and a seizeShare

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/test/proposals/Configs.sol#L18-L19



However if you check all the loops where different mToken parameters are set, you can see that the reserveFactor and seizeShare are missing:



https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/test/proposals/mips/mip00.sol#L170-L233


https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/test/proposals/mips/mip00.sol#L275-L300


https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/test/proposals/mips/mip00.sol#L334-L379


### Recommended Mitigation Steps

Set the reserveFactor and the seizeShare in the build() function in the loop. And also validate it in validate()

```solidity
_pushCrossChainAction(
    cTokenAddress,
    abi.encodeWithSignature("_setReserveFactor(uint256)", config.reserveFactor),
    "Set RF on MToken"
);
_pushCrossChainAction(
    cTokenAddress,
    abi.encodeWithSignature("_setProtocolSeizeShare(uint256)", config.seizeShare),
    "Set seizeShare on MToken"
);

```



## [L&#x2011;02] The consistency level of a VAA is not checked in the TemporalGovernor

As stated in the wormhole [docs](https://docs.wormhole.com/wormhole/quick-start/cross-chain-dev/specialized-relayer#evm-1) it is recommended to check the consistency level when receiving a message. 

The [consistency level](https://docs.wormhole.com/wormhole/explore-wormhole/core-contracts#consistencylevel) is the finality required and it controls how many blocks since the transaction originally occurred the guardian network should wait before the message begins the validation process. This is a defense against reorgs and rollbacks since a transaction, once considered final, is guaranteed not to have the state changes it caused be rolled back. Re-orgs can happen on all EVM chains.


https://moonbeam.network/blog/connected-contracts-wormhole/

### Impact

The state changes that were caused can be rolled back. 


### Code Snippet

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L344

As you can see in _executeProposal() once the message is received, there is no validation of the consistency level


### Recommended Mitigation Steps

Check the consistency level using vm.consistencyLevel in a require statament.


## [L&#x2011;03] ChainlinkOracle will return the wrong price for asset if underlying aggregator hits minAnswer

Chainlink aggregators have a built-in circuit breaker to prevent the price of an asset from deviating outside a predefined price range. This circuit breaker may cause the oracle to persistently return the minPrice instead of the actual asset price in the event of a significant price drop, as witnessed during the LUNA crash.


latestRoundData pulls the associated aggregator and requests round data from it. ChainlinkAggregators have minPrice and maxPrice circuit breakers built into them. This means that if the price of the asset drops below the minPrice, the protocol will continue to value the token at minPrice instead of it's actual value. This will allow users to take out huge amounts of bad debt and bankrupt the protocol.

Example:
TokenA has a minPrice of $1. The price of TokenA drops to $0.10. The aggregator still returns $1 allowing the user to borrow against TokenA as if it is $1 which is 10x it's actual value.


### Impact

In the event that an asset crashes (i.e. LUNA) the protocol can be manipulated to give out loans at an inflated price

### Code Snippet

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Oracles/ChainlinkOracle.sol#L101

```solidity
File: src/core/Oracles/ChainlinkOracle.sol

97: function getChainlinkPrice(
98:     AggregatorV3Interface feed
99: ) internal view returns (uint256) {
100:    (, int256 answer, , uint256 updatedAt, ) = AggregatorV3Interface(feed)
101:        .latestRoundData();
102:    require(answer > 0, "Chainlink price cannot be lower than 0");
103:    require(updatedAt != 0, "Round is in incompleted state");
104:
105:    // Chainlink USD-denominated feeds store answers at 8 decimals
106:    uint256 decimalDelta = uint256(18).sub(feed.decimals());
107:    // Ensure that we don't multiply the result by 0
108:    if (decimalDelta > 0) {
109:        return uint256(answer).mul(10 ** decimalDelta);
110:    } else {
111:        return uint256(answer);
112:    }
113: }
```

As you can see above, there is no check if the price is within an acceptable range


### Recommended Mitigation Steps

Implement the proper check for each asset. It must revert in the case of bad price.

```solidity
) = AggregatorV3Interface(oracleAddress).latestRoundData();
require(price >= minPrice[asset] && price <= maxPrice[asset], "invalid price")

```

## [L&#x2011;04] revokeGuardian() should transfer ownership to an emergency address in case wormhole starts sending malicious messages

There are several assumptions upon which the TemporalGovernor rests, two of which are:

1. Wormhole contract works as it should and no malicious messages are sent to our own contract.
2. If assumption 1 fails, then this contract will be paused, and the guardian will have to fast track a proposal to hand ownership to a new governor

Of course, the guardian role was and can be set. In contrast, if a call is made by TemporalGovernor revoking the guardian role or the old guardian relinquishes the role, the guardian role cannot be set again.

### Impact
If the guardian role is revoked and assumption 1 fails, there will be no guardian to pause the protocol and create a fastTrackProposalExecution which is dependent on the guardian role that has been revoked.

### Code Snippet

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L205-L221

```solidity
File: src/core/Governance/TemporalGovernor.sol


205: function revokeGuardian() external {
206:     address oldGuardian = owner();
207:     require(
208:         msg.sender == oldGuardian || msg.sender == address(this),
209:         "TemporalGovernor: cannot revoke guardian"
210:     );
211:
212:     _transferOwnership(address(0));
213:     guardianPauseAllowed = false;
214:     lastPauseTime = 0;
215:
216:     if (paused()) {
217:         _unpause();
218:     }
219:
220:     emit GuardianRevoked(oldGuardian);
221: }


```

As you can see here, the ownership is transferred to a zero address meaning the guardian wont be able to pause the protocol in an emergency situation


### Recommended Mitigation Steps

Create a third address - emergencyGuardian and transfer ownership to it. This address can only pause and call fastTrackProposalExecution() in emergency situations.




## [L&#x2011;05] No zero address check can lead to loss of funds when redeeming in WETHRouter

Although majority of the zero address check vulnerability have been caught and reported in the known issues report, the check wasnt done in WETHRouter. The absence of this check in WETHRouter::redeem() can lead to loss of funds when the recipient is a zero address.

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/router/WETHRouter.sol#L45

### Impact

Ether is transferred to a zero address and users funds are lost

### Recommended Mitigation Steps

add a zero address check:
```solidity

require(recipient != address(0), "ERR: ZERO_ADDRESS");

```

## [L&#x2011;06] Wrong operator is used when checking nextTotalSupplies/nextTotalBorrows

In Comptroller.sol, there is an issue with the operator used to check whether the nextTotalSupplies/nextTotalBorrows is smaller than or equal to the supplyCap/borrowCap. 
The correct operator to use in this case should be <= and not <.

### Impact

Because the nextTotalSupplies/nextTotalBorrows must be -1 smaller than supplyCap/borrowCap, the nextTotalSupplies/nextTotalBorrows will have to be 1 token less than intended.

### Code Snippet

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L236
https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L341


```solidity
File: src/core/Comptroller.sol

236: require(nextTotalSupplies < supplyCap, "market supply cap reached");

341: require(nextTotalBorrows < borrowCap, "market borrow cap reached");


```

<= Must be used instead of < 


### Recommended Mitigation Steps

Use <= instead of <


## [L&#x2011;07] The intendedRecipient is not checked when fastTrackProposalExecution() is used

If fastTrackProposalExecution() is not used then a proposal must be first queued and only after some period of time in can be executed. Because the proposal is first queued, then checking the intendedRecipient in _queueProposal() is enough however if fastTrackProposalExecution() is used then the intendedRecipient is not checked.


### Impact

If the admin makes an error and the intendedRecipient is not checked then we could be processing intentional malicious information sent by wormhole. 


### Code Snippet

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Governance/TemporalGovernor.sol#L344-L409

### Recommended Mitigation Steps

Check the intendedRecipient even in the _executeProposal() function in case an admin makes a mistake.



## [L&#x2011;08] Missing assertion of collateralFactor of all mTokens in mip00

In mip00, the validate() function is used to check if everything was set correctly. It checks if each config of a mToken has everything set however it doesnt check the collaterFactor

### Impact

If the collateralFactor was wrongly set then all the tests will pass and the team may not notice this so the collateralFactors will be different from what its supposed to be

### Code Snippet

https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/test/proposals/mips/mip00.sol#L712-L734

```solidity
File: test/proposals/mips/mip00.sol

712: assertFalse(
713:     comptroller.mintGuardianPaused(
714:         addresses.getAddress(config.addressesString)
715:     )
716: );
717: assertEq(
718:     comptroller.borrowCaps(
719:         addresses.getAddress(config.addressesString)
720:     ),
721:     config.borrowCap
722: );
723: assertEq(
724:     comptroller.supplyCaps(
725:         addresses.getAddress(config.addressesString)
726:     ),
727:     config.supplyCap
728: );
729: assertEq(
730:     comptroller.supplyCaps(
731:         addresses.getAddress(config.addressesString)
732:     ),
733:     config.supplyCap
734: );


```

As you can see the supplyCap is checked twice but instead of that it must check the collateralFactor


### Recommended Mitigation Steps

Check the collateral factor




