# DegenToken - ERC20 Token for Degen Gaming

## Description

DegenToken is an ERC20 token designed for the Degen Gaming platform on the Avalanche network. The token allows players to earn rewards, transfer tokens to others, and redeem tokens for in-game items available in the platform's store. This smart contract is based on the OpenZeppelin library and includes features like token minting (for the platform owner), transferring tokens, and burning tokens. Additionally, players can redeem their tokens for in-game items from the store.

## Getting Started

### Installing

To use the DegenToken contract, you can clone this GitHub repository to your local machine.

### Executing program

To interact with the DegenToken contract, follow these steps:

1. Deploy the contract on the Avalanche network.
2. Mint new tokens and distribute them to players as rewards.
3. Players can transfer their tokens to others using the `transferTokens` function.
4. Players can redeem their tokens for in-game items by calling the `redeem` function.
5. Players can burn their tokens using the `burn` function when they are no longer needed.

## Video walkthrough: 
https://www.loom.com/share/d645e4d88faf481ca2db5e4ac958a286?sid=c67f07d8-d783-45a1-ab63-b975ce75271a

## Code: 
```
/*Your task is to create a ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract should have the following functionality:

Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
Transferring tokens: Players should be able to transfer their tokens to others.
Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
Checking token balance: Players should be able to check their token balance at any time.
Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.*/


// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {
    constructor() ERC20("Degen", "DGN") {}

    // Store item structure
    struct StoreItem {
        string itemName;
        uint256 price;
    }

    // Store items list
    StoreItem[] public storeItems;

    // Add store items (onlyOwner)
    function addStoreItem(string memory itemName, uint256 price) external onlyOwner {
        storeItems.push(StoreItem(itemName, price));
    }

    // Mint new tokens and distribute them to players as rewards. Only the owner can do this.
    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
    }

    // Transfer tokens from the sender's account to another account.
    function transferTokens(address to, uint256 amount) external {
        require(balanceOf(msg.sender) >= amount, "Don't have enough Degen Tokens");
        _transfer(msg.sender, to, amount);
    }

    // Redeem tokens for items in the in-game store based on the store item index.
    event RedeemItem(address indexed player, string itemName, uint256 price);

    function redeem(uint256 itemIndex) public virtual {
        require(itemIndex < storeItems.length, "Invalid store item index");
        uint256 price = storeItems[itemIndex].price;
        require(balanceOf(msg.sender) >= price, "Not enough Degen Tokens to redeem this item");

        _burn(msg.sender, price);
        emit RedeemItem(msg.sender, storeItems[itemIndex].itemName, price);
    }

    // Anyone can burn their own tokens that are no longer needed.
    function burn(uint256 amount) public virtual {
        _burn(msg.sender, amount);
    }
}

contract StoreInitializer {
    // Deploy the DegenToken contract and add examples to storeItems
    constructor() {
        DegenToken token = new DegenToken();

        // Add examples to storeItems
        token.addStoreItem("Epic Sword of Valor", 100);
        token.addStoreItem("Mystic Potion of Power", 50);
        token.addStoreItem("Legendary Shield of Fortitude", 200);
    }
}

```

## Help

For any questions or issues related to this project, please feel free to open an issue in this GitHub repository.

## Author

Anurag Mishra  
Contact: [Email](mailto:anurag7706@gmail.com)

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
