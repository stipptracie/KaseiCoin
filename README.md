# KaseiCoin - Martian Token Crowdsale

In this project, I created a fungible token that is ERC-20 compliant and that is minted by using a `Crowdsale` contract from the OpenZeppelin Solidity library.

The crowdsale contract manages the entire crowdsale process, allowing users to send ether to the contract and in return receive KSC, or KaseiCoin tokens. The contract will mint the tokens automatically and distribute them to buyers in one transaction.

Remix was used to write, compile, and deploy this test project. (https://remix-project.org/)

---

## Code

### KaseiCoin.sol
```
pragma solidity ^0.5.5;

//  Import the following contracts from the OpenZeppelin library:
//    * `ERC20`
//    * `ERC20Detailed`
//    * `ERC20Mintable`
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20Detailed.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/token/ERC20/ERC20Mintable.sol";

// Create a constructor for the KaseiCoin contract and have the contract inherit the libraries that you imported from OpenZeppelin.
contract KaseiCoin is ERC20, ERC20Detailed, ERC20Mintable {
    constructor(
        string memory name,
        string memory symbol,
        uint initial_supply
    )
        ERC20Detailed(name, symbol, 18)
        public
    {
        // mint(msg.sender, initial_supply);
    }
}
```

### KaseiCoinCrowdsale.sol
```
pragma solidity ^0.5.5;

import "./KaseiCoin.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/Crowdsale.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v2.5.0/contracts/crowdsale/emission/MintedCrowdsale.sol";


// Have the KaseiCoinCrowdsale contract inherit the following OpenZeppelin:
// * Crowdsale
// * MintedCrowdsale
contract KaseiCoinCrowdsale is Crowdsale, MintedCrowdsale { // UPDATE THE CONTRACT SIGNATURE TO ADD INHERITANCE

    // Provide parameters for all of the features of your crowdsale, such as the `rate`, `wallet` for fundraising, and `token`.
    constructor(uint rate, address payable wallet, KaseiCoin token) Crowdsale(rate, wallet, token) public {
        // constructor can stay empty
    }
}

contract KaseiCoinCrowdsaleDeployer {
    // Create an `address public` variable called `kasei_token_address`.
    address public kasei_token_address;
    
    // Create an `address public` variable called `kasei_crowdsale_address`.
    address public kasei_crowdsale_address;

    // Add the constructor.
    constructor(string memory name, string memory symbol, address payable wallet) public {
        // Create a new instance of the KaseiCoin contract.
        KaseiCoin token = new KaseiCoin(name, symbol, 0);
        
        // Assign the token contract’s address to the `kasei_token_address` variable.
        kasei_token_address = address(token);

        // Create a new instance of the `KaseiCoinCrowdsale` contract
        KaseiCoinCrowdsale kasei_crowdsale = new KaseiCoinCrowdsale(1, wallet, token);
            
        // Aassign the `KaseiCoinCrowdsale` contract’s address to the `kasei_crowdsale_address` variable.
        kasei_crowdsale_address = address(kasei_crowdsale);

        // Set the `KaseiCoinCrowdsale` contract as a minter
        token.addMinter(kasei_crowdsale_address);
        
        // Have the `KaseiCoinCrowdsaleDeployer` renounce its minter role.
        token.renounceMinter();
    }
}
```

---

## Evaluation Evidence

### 1. KaseiCoin Token Contract
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/Compiled_file.png" width="500" />

---

### 2. KaseiCoin Crowdsale Contract
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/compiled_crowdsale.png" width="500" />

---

### 3. Deploy the KaseiCoin Deployer and KaseiCoin Crowdsale contract
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/CrowdsaleDeployer.png" width="500" />
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/CrowdsaleFileDeployed.png" width="500" />

---

### 4. Buy Tokens
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/BuyTokens.png" width="500" />
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/KaseiCoinBalance.png" width="500" />
<img src="https://github.com/stipptracie/KaseiCoin/blob/main/ExecutionResults/walletsAfter.png" width="500" />

---

## Project by:
> email: stipptracie@gmail.com |
> [GitHub](https://github.com/stipptracie) |
> [LinkedIn](https://www.linkedin.com/in/tracie-stipp-0719691b/)

---
