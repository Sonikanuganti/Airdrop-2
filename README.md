# Airdrop-2
// SPDX-License-Identifier: MIT

 pragma solidity ^0.8.8;


 contract Dropdistributor {
 /**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
//interface IERC20 {
    /**
     * @dev Returns the number of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
}


    IERC20 public token;

    uint256 public dropAmount;
    uint256 setAmount;
    // uint256 public balance = IERC20(token).balaceOf(this);

    mapping(address => uint ) public addressesClaimed; 

    // This is a packed array of booleans.
    mapping(uint256 => uint256) private claimedBitMap;

    event Claimed(address indexed _from, uint _dropAmount);

    constructor(IERC20 token_) {
        token = token_;
        // dropAmount = dropAmount_;
    }


    modifier onlyOwner() {
        require(
            strategicAllianceTeamAddress == msg.sender,
            "ONLY_STRATEGIC_ALLIANCE_TEAM_CAN_EXECUTE_THIS_FUNCTION"
        );
        _;
    }

    function setDropAmount(uint256 _dropAmount) public onlyOwner{
        setAmount = dropAmount;
    }

    function claim(/*address NewUser*/)
        external 
    {
        if(addressesClaimed[msg.sender] == 1){
            revert("Tokens Already Claimed");
        }
        // require(addressesClaimed[msg.sender] ==0,'Drop Tokens Already Claimed');

        if(addressesClaimed[msg.sender] == 0){
        require(IERC20(token).transfer(msg.sender,setAmount),'Transfer Failed');
        addressesClaimed[msg.sender] = 1;
        }
        emit Claimed(msg.sender,dropAmount);
    }

    function balanceof() public view returns(uint256){
        return IERC20(token).balanceOf(address(this));
    }
}
