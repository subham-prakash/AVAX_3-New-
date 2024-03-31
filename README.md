# ETH_AVAX-MOD3

## Functionality

This Solidity program is a simple  program that works as creating a token and applying following functions Functionality:
1. A new token is created on the local HardHat network.
2. Only contract owner should be able to mint tokens.
3. Any user can transfer tokens.
4. Any user can burn tokens.
## Description

This program is a simple contract written in Solidity, a programming language used for developing smart contracts on the Ethereum blockchain. The contract has functions that allows owner to mint,transfer or burn tokens. This program serves as a simple and straightforward introduction to Solidity programming, and can be used as a stepping stone for more complex projects in the future.

## Getting Started

## Prerequisites

To deploy and interact with this contract, you will need the following:

1. Solidity compiler version 0.8.0 or higher.
2. An Ethereum development environment (e.g., Remix, Truffle, or Hardhat) or an Ethereum client library (e.g., web3.js) to deploy and interact with the contract.

### Executing program
Deploy on local HardHat Neywork
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (e.g., Token.sol). Copy and paste the following code into the file:

```javascript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";

contract Shunham is IERC20{
    string private _name;
    string private _symbol;
    address private _owner;
    mapping(address => uint256) private _balances;
    uint256 private _totalSupply = 0;

    mapping(address => mapping(address => uint256)) private _allowances;

    event Burn(address from, uint256 value);
    event Mint(address to, uint256 value);

    constructor(string memory name, string memory symbol) {
        _name = name;
        _symbol = symbol;
        _owner = msg.sender;
    }

    modifier onlyOwner {
        require(msg.sender == _owner, "Only owner is allowed to perform this operation");
        _;
    }

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 value) public returns (bool) {
        
        if (to == address(0)) {
            revert InvalidReceiver(address(0));
        }
        address sender = msg.sender;
        uint256 senderBalance = _balances[sender]; 
        if (_balances[sender] < value) {
            revert InsufficientBalance(sender, senderBalance, value);
        } 
        _balances[sender] -= value;
        _balances[to] += value;
        emit Transfer(sender, to, value);
        return true;
    }


    function burn(uint256 value) public returns(bool) {
        address sender = msg.sender;
        uint256 senderBalance = _balances[sender];
        if(_balances[sender] < value) {
            revert InsufficientBalance(sender, senderBalance, value);
        }
        _balances[sender] -= value;
        _totalSupply -= value;
        emit Burn(sender, value);
        return true;
    }

    function mint(address to, uint256 value) public onlyOwner returns(bool) {
        _balances[to] += value;
        _totalSupply += value;
        emit Mint(to, value);
        return true;
    }


    function allowance(address owner, address spender) external view returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 value) external returns (bool) {
        address owner = msg.sender;
        uint256 ownerBalance = _balances[owner];
        if (spender == address(0)) {
            revert InvalidReceiver(spender);
        }
        if (ownerBalance < value) {
            revert InsufficientBalance(owner, ownerBalance, value);
        }
        _balances[owner] -= value;
        _allowances[owner][spender] += value;
        emit Approval(owner, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        address spender = msg.sender;
        uint256 allowanceBalance =  _allowances[from][spender];

        if (to == address(0)) {
            revert InvalidReceiver(to);
        }
        if (allowanceBalance < value) {
            revert InsufficientAllowance(spender, from, allowanceBalance, value);
        }
        _allowances[from][spender] -= value;
        _balances[to] += value;
        emit Transfer(from, to, value);
        return true;
    }

    error InvalidReceiver(address _to);

    error InsufficientBalance(address from,uint256 fromBalance,uint256 value);

    error InsufficientAllowance(address spender, address from, uint256 currentAllowance, uint256 value);

}
Deploy this on Harhat

To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.0" (or another compatible version), and then click on the "Compile" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. then click on the "Deploy" button.

Once the contract is deployed, you can interact with it by calling the function.
## Help

Node.js Must be installed.After that run this command

```
npx hardhat node
```
## Disclaimer
This contract is provided as-is without any warranty. Use it at your own risk. The contract author and contributors are not responsible for any loss or damage caused by using this contract.

## Authors

Subham Prakash

## License

This project is licensed under the MIT License.
