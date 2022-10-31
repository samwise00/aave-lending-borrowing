# Notes

## Overview

Project for learning purposes. Will attempt to document development process to be used as reference in the future.

Raffle/Lottery system with verifiable randomness using chainlink oracle, as well as chainlink keepers for interacting with the contract according to a time interval. Focus is on smart contracts with minimum frontend UI. User can enter the lottery, and the contract will pick a random winner based on those who have entered after x amount of time has passed.

## Setup

1. Install Dependencies
   Note: hardhat toolbox packages within it @nomiclabs/hardhat-ethers, @nomiclabs, hardhat-etherscan, hardhat-gas-reporter, solidity-coverage, @typechain/hardhat
   `npm install --save-dev @nomicfoundation/hardhat-toolbox @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers @nomiclabs/hardhat-waffle chai ethereum-waffle hardhat hardhat-contract-sizer hardhat-deploy prettier prettier-plugin-solidity solhint dotenv`

2. Add configuration files
   `.env` containing private keys, rpc urls
   `.prettierrc` prettier config
   `hardhat.config.js`
   `.gitignore`
   `.solhint.json` solidity linter

## Chainlink VRF

Using `requestRandomWinner` to get randomly generated number
Reference: https://docs.chain.link/docs/intermediates-tutorial/

`npm install --save-dev @chainlink/contracts`

Import Interface and VRF contracts
`import '@chainlink/contracts/src/v0.8/interfaces/VRFCoordinatorV2Interface.sol';`
`import '@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol';`

Inherit VRFConsumerBaseV2
`contract Raffle is VRFConsumerBaseV2 { ...`

Initialize
`constructor(address vrfCoordinatorV2, uint256 entranceFee) VRFConsumerBaseV2(vrfCoordinatorV2) {...`

Implement request winner logic in `pickRandomWinner()`

Implement fulfill logic in `fulfillRandomWords()`

Create subscription for Chainlink VRF at https://vrf.chain.link/
Create one on testnet for use in staging tests

## Chainlink Automation (Keepers)

import `import "@chainlink/contracts/src/v0.8/AutomationCompatible.sol";`
Reference: https://docs.chain.link/docs/chainlink-automation/compatible-contracts/

`import "@chainlink/contracts/src/v0.8/AutomationCompatible.sol";`

Chainlink Automation requres two functions to exist in the contract in order to perform the upkeep.

1. `checkUpkeep()` to determine if the criteria has been met to perform the upkeep
2. `performUpkeep()` to perform the action

## Mocking Contracts for Local Deploys

When we are on a local network we will not have access to the live chainlink contracts on a test network like Goerli. We will need to reference the code for the required contracts (VRFCoordinator and Chainlink Automation in our case) and deploy the contracts manually for our Raffle contract to reference.

This can be done by simply importing the mock contract from chainlink repo:
`import "@chainlink/contracts/src/v0.8/mocks/VRFCoordinatorV2Mock.sol";`
Importing like this is the same as copy pasting their contract code.

To deploy programatically:

1. Have the deploy script check if that target environment is a dev environment
2. If it's a dev environment, deploy mock contract before deploying Raffle.sol

\*\* Be sure to check what constructor arguments the mocked contracts take

## Commands Cheat Sheet

Initialize Hardhat Project
`npm hardhat`

Compile solidity contracts in contracts directory
`npx hardhat compile`

## Misc

Solidity Style Guide
https://docs.soliditylang.org/en/v0.8.16/style-guide.html
