# **MyToken ERC20 Contract**

This project demonstrates a Solidity contract, `MyToken`, which implements a custom ERC20 token with essential functionalities: minting, burning, and transferring tokens. The contract is deployable using Remix and includes features for token management.

---

## **Description**

The project provides a custom implementation of an ERC20 token contract, focusing on simplicity and functionality. 

### **Features**

#### **Token Details**
- **Name:** PICHI
- **Symbol:** PCH
- **Decimals:** 18 (default for ERC20 tokens)

#### **Functionalities**
1. **Mint Tokens:** 
   - Allows the contract owner to mint new tokens to any specified address.
   - Increases the total supply upon minting.

2. **Transfer Tokens:** 
   - Allows any user to transfer tokens from their balance to another address.

3. **Burn Tokens:** 
   - Allows any user to burn tokens from their balance or another account (given sufficient balance).
   - Reduces the total supply upon burning.

4. **Event Logging:**
   - Emits `Transfer`, `Mint`, and `Burn` events for transaction transparency.

---

## **Code Overview**

#### **MyToken Contract**
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract MyToken {
    string public name = "PICHI";
    string public symbol = "PCH";
    uint8 public decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed to, uint256 value);
    event Burn(address indexed from, uint256 value);

    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    constructor() {
        owner = msg.sender;
    }

    function transfer(address to, uint256 amount) external returns (bool) {
        require(to != address(0), "Cannot transfer to the zero address");
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");

        balanceOf[msg.sender] -= amount;
        balanceOf[to] += amount;

        emit Transfer(msg.sender, to, amount);
        return true;
    }

    function mint(address to, uint256 amount) external onlyOwner returns (bool) {
        require(to != address(0), "Cannot mint to the zero address");

        totalSupply += amount;
        balanceOf[to] += amount;

        emit Mint(to, amount);
        return true;
    }

    function burn(address from, uint256 amount) external returns (bool) {
        require(from != address(0), "Cannot burn from the zero address");
        require(balanceOf[from] >= amount, "Insufficient balance to burn");

        balanceOf[from] -= amount;
        totalSupply -= amount;

        emit Burn(from, amount);
        return true;
    }
}
```

### Deployment Instructions
Deploy via Remix:
Open Remix IDE.
Create a new Solidity file (e.g., MyToken.sol) and paste the contract code.
Compile the contract using the Solidity compiler (^0.8.0).
Deploy the contract:
No parameters are required for deployment.
The deploying address will automatically become the contract owner.

### Interacting with the Contract
Functions:
Mint Tokens (mint):
Input: Recipient address and the amount to mint.
Restriction: Only callable by the contract owner.
Transfer Tokens (transfer):
Input: Recipient address and the amount to transfer.
Any user can transfer tokens if they have sufficient balance.
Burn Tokens (burn):
Input: Address to burn tokens from and the amount to burn.
Any user can burn tokens from their own or another account (with sufficient balance).
Example Usage
Mint Tokens:
Call mint as the contract owner to allocate tokens to specific addresses.

Transfer Tokens:
Use transfer to send tokens from your account to another address.

Burn Tokens:
Use burn to destroy tokens from your own or another address.

### Future Improvements
Add role-based access control for advanced minting and burning permissions.
Implement approval-based transfers (similar to standard ERC20 allowances).
Integrate a frontend using React.js for easier user interaction.

### Authors
Shawn Aaron Quirante
