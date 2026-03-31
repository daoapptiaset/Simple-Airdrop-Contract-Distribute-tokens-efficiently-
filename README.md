# Simple-Airdrop-Contract-Distribute-tokens-efficiently-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

interface IERC20 {
    function transfer(address to, uint256 amount) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
}

contract SimpleAirdrop {
    address public owner;
    IERC20 public token;

    constructor(address _token) {
        owner = msg.sender;
        token = IERC20(_token);
    }

    function airdrop(address[] calldata recipients, uint256[] calldata amounts) public {
        require(msg.sender == owner, "Only owner");
        require(recipients.length == amounts.length, "Length mismatch");

        for (uint256 i = 0; i < recipients.length; i++) {
            token.transfer(recipients[i], amounts[i]);
        }
    }

    function withdrawTokens() public {
        require(msg.sender == owner, "Only owner");
        uint256 balance = token.balanceOf(address(this));
        token.transfer(owner, balance);
    }
}
