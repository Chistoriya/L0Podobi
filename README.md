// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MoulenToken {
    string public name = "Moulen";
    string public symbol = "MLN";
    uint256 public maxSupply = 1000000;
    uint256 public totalSupply;

    mapping(address => uint256) private balances;

    constructor() {
        totalSupply = 0;
    }

    function donate() public payable {
        require(msg.value > 0, "Donation amount must be greater than zero");
        require(totalSupply + msg.value <= maxSupply, "Max supply exceeded");

        balances[msg.sender] += msg.value;
        totalSupply += msg.value;
    }

    function moneyBack(uint256 amount) public {
        require(amount > 0, "Amount must be greater than zero");
        require(amount <= balances[msg.sender], "Insufficient balance");

        balances[msg.sender] -= amount;
        totalSupply -= amount;

        (bool success, ) = msg.sender.call{value: amount}("");
        require(success, "Transfer failed");
    }

    function balanceOf(address account) public view returns (uint256) {
        return balances[account];
    }
}
# L0Podobi
L0 code for ERC 20 token
