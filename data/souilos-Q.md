# VULN 1 

## [LOW] Empty receive()/payable fallback() function does not authenticate requests
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 65 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

    receive() external payable {}

------------------------------------------------------------------------ 

### Mitigation 

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))).Empty receive()/fallback() payable functions that are not used, can be removed to save deployment gas. Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. If the concern is having to spend a small amount of gas to check the sender against an immutable address, the code should at least have a function to rescue unused Ether.










# VULN 2 

## [LOW] Use .call instead of .transfer to send ether
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 965 at 2023-07-moonwell/src/core/Comptroller.sol:

            token.transfer(admin, token.balanceOf(address(this)));


Found in line 967 at 2023-07-moonwell/src/core/Comptroller.sol:

            token.transfer(admin, _amount);


Found in line 152 at 2023-07-moonwell/src/core/MErc20.sol:

    	token.transfer(admin, balance);


Found in line 225 at 2023-07-moonwell/src/core/MErc20.sol:

        token.transfer(to, amount);

------------------------------------------------------------------------ 

### Mitigation 

.transfer will relay 2300 gas and .call will relay all the gas. If the receive/fallback function from the recipient proxy contract has complex logic, using .transfer will fail, causing integration issues.Replace .transfer with .call. Note that the result of .call need to be checked.










# VULN 3 

## [LOW] Open TODOs
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 144 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                addresses.getAddress("GUARDIAN") /// TODO figure out what the pause guardian is on Base, then replace it


Found in line 210 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

                /// TODO calculate initial exchange rate

------------------------------------------------------------------------ 

### Mitigation 

Code architecture, incentives, and error handling/reporting questions/issues should be resolved before deployment.










# VULN 4 

## [LOW] Use safeTransferOwnership instead of transferOwnership function
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 260 at 2023-07-moonwell/test/proposals/mips/mip00.sol:

        proxyAdmin.transferOwnership(governor);

------------------------------------------------------------------------ 

### Mitigation 

Use a 2 structure transferOwnership which is safer. safeTransferOwnership, use it is more secure due to 2-stage ownership transfer. https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable2Step.sol










# VULN 5 

## [LOW] Immutables should be in uppercase
------------------------------------------------------------------------ 

### Proof of concept 

Found in line 15 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

    WETH9 public immutable weth;


Found in line 18 at 2023-07-moonwell/src/core/router/WETHRouter.sol:

    MErc20 public immutable mToken;


Found in line 34 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    IWormhole public immutable wormholeBridge;


Found in line 37 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    uint256 public immutable proposalDelay;


Found in line 40 at 2023-07-moonwell/src/core/Governance/TemporalGovernor.sol:

    uint256 public immutable permissionlessUnpauseTime;


Found in line 15 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

    address public immutable base;


Found in line 19 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

    address public immutable multiplier;


Found in line 23 at 2023-07-moonwell/src/core/Oracles/ChainlinkCompositeOracle.sol:

    address public immutable secondMultiplier;


Found in line 57 at 2023-07-moonwell/src/core/MultiRewardDistributor/MultiRewardDistributor.sol:

    Comptroller public comptroller; /// we can't make this immutable because we are using proxies

------------------------------------------------------------------------ 

### Mitigation 

Immutables should be in uppercase, it helps to distinguish immutables from other types of variables and provides better code readability.
