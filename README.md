# CrossLink SDK README

The CrossLink SDK is a powerful tool for developers to implement multichain transactions with multihop features supported by Chainlink CCIP. It is also compatible with Viem Wallet.

## Table of Contents

- [Installation](#installation)
- [Getting Started](#getting-started)
- [SDK Functions](#sdk-functions)
  - [1. `hopThenExecute`](#1-hopthenexecute)
  - [2. `fetchBestRoutes`](#2-fetchBestRoutes)
- [Example](#example)
- [Contributing](#contributing)

---

## Installation

To get started with the CrossLink SDK, you can install it via npm:

`
npm install crosslink-sdk
`
or
`yarn add crosslink-sdk
`

### Functions
1. hopThenExecute(source, destination, sourceDetails)
This function allows you to perform a multichain transaction with multihop features. It returns a transaction hash upon success.
  - `source`: The source chain identifier. It can be one of the following: 'op-testnet', 'polygon-testnet', 'base-testnet', or 'bsc-testnet'.
  - `destination`: The destination chain identifier. It can be one of the following: 'op-testnet', 'polygon-testnet', 'base-testnet', or 'bsc-testnet'.
  - `sourceDetails`: An object containing details about the transaction on the source chain. It should have the following structure:
```
const sourceDetails = {
  contractAddr: contractAddr,
  contractABI: contractABI,
  functionName: functionName,
  args: [] // arguments/parameters will pass to the destination contract
};
```



2. fetchBestRoutes(FROM, TO)
This function retrieves the best possible routes for a transaction.
```
const bestRoutes = await crosslink.fetchBestRoutes(FROM, TO);
```

#### Source and Destination Chain Identifiers
The source and destination variables are used to specify the source and destination chain identifiers for multichain transactions. These identifiers determine the blockchain networks involved in the transaction. Here are the valid options for these variables:

- `op-testnet`: This represents the Opera Testnet blockchain network.
- `fuji-testnet`: This represents the Polygon Testnet blockchain network.
- `polygon-testnet`: This represents the Polygon Testnet blockchain network.
- `base-testnet`: This represents Base testnet blockchain network 
- `bsc-testnet`: This represents the Binance Smart Chain Testnet blockchain network



### Example
Follow this example to use the SDK

```
const CrossLink = require('crosslink-sdk');
const { createWalletClient, http } = require('viem');
const { privateKeyToAccount } = require('viem/accounts');
const { polygonMumbai } = require('viem/chains');

const account = privateKeyToAccount(process.env.PK); // Replace with your private key
const walletAccount = createWalletClient({
  chain: polygonMumbai, // Replace with your desired chain configuration
  account,
  transport: http(),
});
const crosslink = new CrossLink(walletAccount);
const sourceDetails = {
  contractAddr: "0xF52907fC5b46788CEb25C9b3051e818a88a6Dce4",
  contractABI: MarketplaceMockABI,
  functionName: "buy",
  args: [
    "0xeD7B73A82dB4D2406c0a25c55122fc317f2e6Afd", // tokenAddr
    "1" // tokenId
  ]
};
const FROM = 'polygon-testnet';
const TO = 'bsc-testnet';
const hash = await crosslink.hopThenExecute(FROM, TO, sourceDetails);
```

## Contributing
Contributions to the CrossLink SDK are welcome. If you find any issues or have suggestions for improvements, please open an issue or create a pull request on the GitHub repository.
