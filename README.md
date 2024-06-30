# DegenToken - ERC20 Token

DegenToken is an ERC20 token created using Solidity and deployed using the Remix IDE. This token allows the contract owner to mint tokens, and any user to burn and transfer tokens. Additionally, users can redeem different types of cards using the tokens they possess.

## Contract Details

### Contract Name: DegenToken

### Token Details
- **Name**: DegenGaming
- **Symbol**: DGN

### Features
1. **Minting**: The contract owner can mint tokens for players.
2. **Burning**: Any user can burn their tokens.
3. **Transferring**: Users can transfer tokens to others.
4. **Redeem Cards**: Users can redeem different types of cards by burning tokens.

## Installation

1. Clone the repository or create a new file in Remix IDE.
2. Copy and paste the contract code into your new file.

## Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract DegenToken is ERC20, Ownable, ERC20Burnable {

    constructor() ERC20("DegenGaming", "DGN") Ownable(msg.sender) {}

    // Redeemable items
    enum Cards { premium, superpremium, legend, pro }

    struct Player {
        address toAddress;
        uint amount;
    }

    // Players who can buy DegenGaming
    Player[] public players;

    struct PlayerCards {
        uint premium;
        uint superpremium;
        uint legend;
        uint pro;
    }

    // Storing of redeemable cards
    mapping(address => PlayerCards) public playerCards;

    function buyDegen(address _toAddress, uint _amount) public {
        players.push(Player({toAddress: _toAddress, amount: _amount}));
    }

    // Mint tokens for the players
    function mintToken() public onlyOwner {
        while (players.length != 0) {
            uint i = players.length - 1;
            if (players[i].toAddress != address(0)) {
                _mint(players[i].toAddress, players[i].amount);
                players.pop();
            }
        }
    }

    // Transfer tokens to others
    function transferDegen(address _to, uint _amount) public {
        require(_amount <= balanceOf(msg.sender), "low degen");
        _transfer(msg.sender, _to, _amount);
    }

    // Redeem different cards
    function redeemCards(Cards _card) public {
        if (_card == Cards.premium) {
            require(balanceOf(msg.sender) >= 10, "Low degen");
            playerCards[msg.sender].premium += 1;
            burn(10);
        } else if (_card == Cards.superpremium) {
            require(balanceOf(msg.sender) >= 20, "Low degen");
            playerCards[msg.sender].superpremium += 1;
            burn(20);
        } else if (_card == Cards.legend) {
            require(balanceOf(msg.sender) >= 30, "Low degen");
            playerCards[msg.sender].legend += 1;
            burn(30);
        } else if (_card == Cards.pro) {
            require(balanceOf(msg.sender) >= 40, "Low degen");
            playerCards[msg.sender].pro += 1;
            burn(40);
        } else {
            revert("invalid card selected");
        }
    }

    // Burn tokens
    function burnDegengaming(address _of, uint amount) public {
        _burn(_of, amount);
    }

    // Check token balance
    function checkBalance() public view returns (uint) {
        return balanceOf(msg.sender);
    }
}
```

## Usage
Deploy the contract: Deploy the DegenToken contract using Remix IDE.

Mint Tokens: Only the owner can call mintToken to mint tokens for players.

Transfer Tokens: Use the transferDegen function to transfer tokens to other addresses.

Redeem Cards: Call redeemCards with the appropriate card type to redeem cards by burning tokens.

Check Balance: Use checkBalance to view the balance of tokens.

## Enum - Cards
premium

superpremium

legend

pro

## Public Functions
buyDegen(address _toAddress, uint _amount)

mintToken()

transferDegen(address _to, uint _amount)

redeemCards(Cards _card)

burnDegengaming(address _of, uint amount)

checkBalance()

## License
This project is licensed under the MIT License - see the LICENSE file for details.
