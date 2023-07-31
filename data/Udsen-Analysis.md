
## Any comments for the judge to contextualize your findings

My findings are based on following areas:

How the `supplyCaps` can be used by malicious users to prevent borrower adding liquidity to thier existing position to avoid liquidations
How a malicioius borrower can avoid liquidations by using the `tokenRecieved` hook calls in the ERC777 tokens
How certain critical functinalities are missing in the `Moonwell.Comptroller.sol` compared to the `Compound.Comptroller.sol` contract which it is forked from.
How `block.timestamp` overflow can break critical functionality of the protocol.
How functions containing `payable address` do not have functionality to handle native `ETH` transfers.
How the `latestRoundData()` function of chainlink oracles can provide stale prices due to not using the `updatedAt` timestamp

## Approach taken in evaluating the codebase

I mainly focussed on the `MultiRewardDistributor`, `ChainlinkCompositeOracle` and `TemporalGovernor` contracts since the sponsor had mentioned they were the contracts with specific areas of concern.
Further I analyzed the `Comptroller.sol` to find any discrepencies between the `Compound.Comptroller.sol` that could lead to vulnerabilities.  
Further I audited the `MToken` contract and `MErc20` contracts since they had main functinality related to asset management and liquidation process.

## Architecture recommendations

The `MultiRewardDistributor` contract was implementing the novel idea of multiple emission tokens which behave as multiple reward tokens that should be provided to users of the protocol.
More attention should be given to how the rewards are distributed to users during critical functions such as `liquidations` and `Borrow Repayements`.
Since the rewards are distributed by iterating through each of the emission tokens for a particular market, a single revert in an iteration could completely revert the entire transaction.
Futher How passing the control to an external contract during reward distribution could impact the transaction execution should also considered.
For example if `ERC777` which is backward compatible with `ERC20` is used as `emissionToken` then a malicious user can use the `tokenRecieved` hook call of the ERC777 token to revert the entire liquidation transaction.
These vulnearbilities could be extremely dangerous for proper execution of the protocol.

Codebase quality analysis

Codebase was nicely coded and well structured. Most of the functionalities were imported from the `Compound` protocol. Few customization were added to core function like `Comptroller.mintAllowed` by introducing `supplyCap` mapping.
The `Comptroller`, `MToken` and `MErc20` showed close resemblance to core contracts of the `Compound` protocol. Few typos in the `Natspec` comments were observed and it is recommended to pay more attention on clarity of the `Natspec` comments.
For example some of the functions did not have the `Natspec` comments on the `return` value.
It will make it easier for the audtiors and developers to better understand the codebase.

Centralization risks

Centrelization risk was observed in the `MultiRewardDistributor` contract where the `admin` was give all the authority within the contract to control key functions such as `_addEmissionConfig` etc ...
Further admin acts like an owner of the config to set parameters, and act like the comptroller to kick the reward index updates, and act like a pause guardian to pause things.  
Hence in this contract admin plays few major roles.

## Systemic risks

More focus should be paid on how the multiple token reward distribution is executed. Since this could open more avenues for various vulnerabilities which are common among `ERC20` tokens.
These vulnerabilities occur since multiple ERC20 tokens can be configured as `emissionTokens`.

### Time spent:
15 hours