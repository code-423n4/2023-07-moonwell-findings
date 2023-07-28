# L-001 Should check available amount before transfer funds



## Instance 1

[MultiRewardDistributor.sol#L471-L487](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/MultiRewardDistributor/MultiRewardDistributor.sol#L471-L487)

    function _rescueFunds(
    address _tokenAddress,
    uint256 _amount
    ) external onlyComptrollersAdmin {
        IERC20 token = IERC20(_tokenAddress);
        // Similar to mTokens, if this is uint256.max that means "transfer everything"
        if (_amount == type(uint256).max) {
            token.safeTransfer(
                comptroller.admin(),
                token.balanceOf(address(this))
            );
        } else {
            @audit Should check available balance
            @audit require( _amount <= token.balanceOf(address(this), "Not enough token balance");
            
            token.safeTransfer(comptroller.admin(), _amount);
        }

        emit FundsRescued(_tokenAddress, _amount);
    }

## Instance 2

[Comptroller.sol#L959-L969](https://github.com/code-423n4/2023-07-moonwell/blob/fced18035107a345c31c9a9497d0da09105df4df/src/core/Comptroller.sol#L959-L969)

    function _rescueFunds(address _tokenAddress, uint _amount) external {
        require(msg.sender == admin, "Unauthorized");

        IERC20 token = IERC20(_tokenAddress);
        // Similar to mTokens, if this is uint.max that means "transfer everything"
        if (_amount == type(uint).max) {
            token.transfer(admin, token.balanceOf(address(this)));
        } else {
            @audit Should check available balance before transfer
            @audit require( _amount <= token.balanceOf(address(this), "Not enough token balance");
            
            token.transfer(admin, _amount);
        }
    }


