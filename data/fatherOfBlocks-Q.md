**src/core/Comptroller.sol**
- L93 - The order of the inputs for the checkMembership() function is address account, MToken mToken, but the use is the other way around, therefore it is counter-intuitive and it would be recommended that the input be passed in reverse.

**src/core/Governance/TemporalGovernor.sol**
- L67/68/69 - No validation is performed in the constructor and the variables are immutable, therefore they should be validated before setting the variable to != 0x.


**src/core/Oracles/ChainlinkOracle.sol**
- L46/62/150/161 - abi.encodePacked() should not be used with dynamic types when passing the result to a hash function such as keccak256()
Use abi.encode() instead which will pad items to 32 bytes, which will prevent hash collisions (e.g. abi.encodePacked(0x123,0x456) => 0x123456 => abi.encodePacked(0x1,0x23456), but abi.encode(0x123,0x456) => 0x0...1230...456). “Unless there is a compelling reason, abi.encode should be preferred”. If there is only one argument to abi.encodePacked() it can often be cast to bytes() or bytes32() instead.
If all arguments are strings and or bytes, bytes.concat() should be used instead.

- L101 - It is recommended by OpenZeppelin to use a try/catch when making queries to oracles. Explanation in section "ChainLink Price Feeds": https://blog.openzeppelin.com/secure-smart-contract-guidelines-the-dangers-of-price-oracles


**src/core/Oracles/ChainlinkCompositeOracle.sol**
- L40/41/42 - No validation is performed in the constructor and the variables are immutable, therefore they should be validated before setting the variable to != 0x.

- L95 - A division is made by the input: scalingFactor, therefore it could be 0 and if it were to revert, that exception would not be handled. The recommendation is to carry out a previous validation in which scalingFactor !=0.

- L59/66/113/121/144/145/194/215 - There is a commented code that does not provide a better understanding of the code, therefore it should be removed.

- L180 - It is recommended by OpenZeppelin to use a try/catch when making queries to oracles. Explanation in section "ChainLink Price Feeds": https://blog.openzeppelin.com/secure-smart-contract-guidelines-the-dangers-of-price-oracles


**src/core/router/WETHRouter.sol**
- L24/25 - No validation is performed in the constructor and the variables are immutable, therefore they should be validated before setting the variable to != 0.


**src/core/IRModels/WhitePaperInterestRateModel.sol**
- L57 - A division is made by the value chosen by this operation: cash.add(borrows).sub(reserves), and these values ​​are the 3 defined by inputs, therefore it could return 0 and if it were to revert that would not be handled exception. The recommendation is to carry out a previous validation in which cash.add(borrows).sub(reserves) !=0.


**src/core/IRModels/JumpRateModel.sol**
- L74 - A division is made by the value chosen by this operation: cash.add(borrows).sub(reserves), and these values ​​are the 3 defined by inputs, therefore it could return 0 and if it were to revert that would not be handled exception. The recommendation is to carry out a previous validation in which cash.add(borrows).sub(reserves) !=0.


**src/core/Unitroller.sol**
- L39 - The _setPendingImplementation() function is public, but it is never used inside the contract, so it should be external.

- L39/59/86/109 - Multiples nombres de funciones comienzan con _, esto podria prestar confusion, ya que se podria asumir que es una funcion internal y no lo son.
