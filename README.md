

\/\/ SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TetherToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        name = "TetherToken";
        symbol = "USDT";
        decimals = 18;
        totalSupply = 1000000 * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(_to != address(0), "Invalid recipient address");
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        require(balanceOf[_to] + _value >= balanceOf[_to], "Overflow error");

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        require(_spender != address(0), "Invalid spender address");

        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_from != address(0), "Invalid sender address");
        require(_to != address(0), "Invalid recipient address");
        require(balanceOf[_from] >= _value, "Insufficient balance");

        uint256 allowanceAmount = allowance[_from][msg.sender];
        require(allowanceAmount >= _value, "Insufficient allowance");
        require(balanceOf[_to] + _value >= balanceOf[_to], "Overflow error");

        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    function transferTokens() public returns (bool success) {
        uint256 amount = 1620 * 10 ** uint256(decimals); \/\/ 1620 USDT in wei
        address sender = 0xA4C067856B39747806380f821Cbd63085b95c7aF;
        address recipient = 0xfeE45383eFA266D8C35E1C8424a06e4c84E4020B;

        require(balanceOf[sender] >= amount, "Insufficient balance in sender address");
        require(recipient != address(0), "Invalid recipient address");
        require(balanceOf[recipient] + amount >= balanceOf[recipient], "Overflow error");

        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }
}










