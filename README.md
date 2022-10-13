# Flash Loan Arbitrage Using AAVE
### Based on the Goerli Network  

This project demonstrates how a flash loan can be used in Arbitrary trades on decentralised exchanges like Uniswap, SushiSwap etc

What you will need:

* Visual Studio Code
* Metamask
* Remix
* Infura Account

## Getting Started

Clone the repository and open the folder within Visual Studio Code

Create a .env file in the root folder with the following two lines of code:

```
PRIVATE_KEY=
INFURA_GOERLI_ENDPOINT=
```

Head over to Infura (register for an account if you have not done so already) https://infura.io/dashboard - Once you can see the dashboard. click CREATE NEW KEY located on the top right of your screen. Once you have a new project you will be presented with network endpoints, be sure to select Gorli under the Ethereum section. Copy the URL and past this within the .env file created earlier.

Open metamask and find the private key to your wallet, enter your private key into the .env file you created earlier.

## Getting Test Tokens

In order to use this Flash Loan Arbitrage project you will need some test ether and also Test USDC & Test DAI, let's get the ether first: 

Requires Twitter Login:
https://faucets.chain.link/

Now we need to get some USDC & DAI

https://staging.aave.com/faucet/?marketName=proto_goerli 
Ensure you have selected VERSION 3 on the Gorli network (this is located on the drop arrow of the page heading)

Click Faucet next to both USDC & DAI, then add them to your Metamask wallet.

## Publish contracts to the blockchain

We are going to publish the DEX contract first as we will need to have the contract address for the FlashLoan to communicate with.
Using hardhat enter the following command within the terminal of VS Code

```
npx hardhat run --network goerli scripts/deployDex.js
```
Once the contract has been deployed you will be provided with a unique contract address, copy this to your clipboard so we can use it in the next step. 

Open FlashLoanArbitrage.sol nlocated within the contracts folder, on line 30 please remove the current address and PASTE the contract address you copied in the previous step. Hit CTRL+S to save the file, this sets the contract address to your DEX contract. 

Next we want to publish FlashLoanArbitrage.sol, using hardhat hat within the terminal enter the following similar to above;
```
npx hardhat run --network goerli scripts/deployFlashLoanArbitrage.js
```

## Interacting with your contracts

Open https://remix.ethereum.org and create TWO .sol files, one called Dex.sol and the other called FlashLoanArbitrage.sol copy the code from VS Code and paste it into the matching file names. 

Compile the scripts and then head over to the deploy tab. We are going to interact with Dex.sol first. 
Under the environment dropdown menu select INJECTED PROVIDER, connect your metamask wallet and ensure the ACCOUNT matched the one you have been using. 

In the textbox 'At Address' PASTE the contract address for your Dex.sol contract (this can be found within your visual studio code terminal) then click the blue 'At Address' button. 

Then select the FlashLoanArbitrage.sol, find the deployed contract address for the FlashLoanArbitrage.sol contract you deployed in Visual Studio Code, paste this the same as above. 

You should see both contracts under the Deployed Contracts tab with some buttons allowing you to interact. 

## Adding funds to your smart contracts

Copy the DEX deployed contract address from remix and open your metamask. 

Click send and send this contract TWO transactions of test tokens:
1500 DAI 
1500 USDC 

Once these are confirmed you can check the contract to ensure correct balances using the following addresses in the getBalance function of the DEX contract. 
DAI = 0xDF1742fE5b0bFc12331D8EAec6b478DfDbD31464
USDC = 0xA2025B15a1757311bfD68cb14eaeFCc237AF5b43

Now head over to FlashLoanArbitrage within the deployed contracts tab

We need to approve a limit for USDC and DAI, as USDC and DAI operate different decimal places the following entries must be used:

USDC = 1000000000
DAI = 1200000000000000000000 

click the orange button on each to approve this. 

## Let's Run The Arb Trade With A Flash Loan! 

Within the requestFlashLoan function of the FlashLoanArbitrage contract copy and paste the following: 

0xA2025B15a1757311bfD68cb14eaeFCc237AF5b43,1000000000 

Metamask will open, check the gas and confirm when happy

This will borrow 1000USDC and follow the logic for an arb trade, securing you profit. 

Check Etherscan to see the inner workings of what just happened: https://goerli.etherscan.io/

I hope you enjoyed this, any questions please feel free to reach out to me. 
