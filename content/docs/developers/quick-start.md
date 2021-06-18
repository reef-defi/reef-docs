---
title: "Quick Start"
description: "Get quickly up and running with Reef chain"
lead: ""
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "developers"
weight: 200
toc: true
---

In this guide we will deploy a simple Flipper smart contract onto Reef chain.

The guide is broken into 3 parts:
 - setting up the Reef development node
 - setting up the smart contract development environment
 - deploying and running your first smart contract


## Setting up a development node

### Requirements

Reef chain is written in [Rust](https://www.rust-lang.org/). A basic familiarity with Rust tooling is required.


### Clone the repo
{{< btn-copy text="git clone --recursive https://github.com/reef-defi/reef-chain" >}}
```bash
git clone --recursive https://github.com/reef-defi/reef-chain
```

### Install Rust
If you don't have [Rust](https://www.rust-lang.org/tools/install) already, you can install it with:
{{< btn-copy text="curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh" >}}
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

You can install the development compiler and the toolchain with:
{{< btn-copy text="make init" >}}
```bash
make init
```

### Start a development node

The `make run` command will launch a temporary node and its state will be discarded after you terminate the process.

To run the temporary (development) node run:
{{< btn-copy text="make run" >}}
```bash
make run
```

You should see an output like [this](https://i.imgur.com/Dst10UI.png) when the Reef chain starts producing blocks.



### (Optional) Run a persistent single-node chain

Use the following command to build the node without launching it:

```bash
make build
```

This command will start the single-node development chain with persistent state:

```bash
./target/release/reef-node --dev
```

Purge the development chain's state:

```bash
./target/release/reef-node purge-chain --dev
```


## Smart Contracts

{{< alert icon="💡" text="Reef chain is NOT based on Ethereum. It is based on newer and improved blockchain architecture provided by Polkadot Substrate. The only thing in common with Ethereum is the EVM, which means that all Ethereum smart contracts will also work on Reef Chain. However, many tools from Ethereum such as Metamask and web3 are NOT compatible. Fortunately we have drop-in replacements." >}}

The Reef chain smart contracts are written in [Solidity](https://docs.soliditylang.org/en/v0.8.2/).
Any existing Ethereum smart contracts written in Solidity 0.6 or greater will work on Reef chain
without a need for modifications.

However some of the tooling from Ethereum will NOT work on Reef chain due to inherent differences in
the blockchain architecture.

Fortunately we have drop in replacements for most commonly used tools.

| Ethereum        |        | Reef Chain                                                   |
| --------------- | ------ | ------------------------------------------------------------ |
| Metamask        |   -->   | [Polkadot Extension](https://polkadot.js.org/extension/)     |
| web3            |   -->   | [@reef-defi/evm-provider](https://github.com/reef-defi/evm-provider.js) |
| Truffle/Hardhat |   -->   | [Reef Hardhat](https://github.com/reef-defi/hardhat-reef)    |
| Remix IDE       |   -->   | [Reef Remix](https://remix.reefscan.com/)                    |
| Etherscan       |   -->   | [Reefscan](https://reefscan.com/)                            |


## Deploy your first ERC-20 Token

We are going to pick a simple ERC-20 contract for our demo, but feel free to try out other examples as well.

In this guide we are using [Reef Hardhat](https://github.com/reef-defi/hardhat-reef), which is
suitable for more experienced developers. If you would like to deploy your contract via an easy to
use web IDE, try [Reef Remix](https://remix.reefscan.com/).

### Clone the examples repo
{{< btn-copy text="git clone https://github.com/reef-defi/hardhat-reef-examples" >}}
```bash
git clone https://github.com/reef-defi/hardhat-reef-examples
```

### Install the dependencies
{{< btn-copy text="yarn" >}}
```bash
yarn
```

### Run the deployment script

{{< btn-copy text="npx hardhat run scripts/erc20/deploy.js" >}}
```bash
npx hardhat run scripts/erc20/deploy.js
```

## Interact with the ERC-20 to send tokens

We can call functions on our smart contract through the Hardhat interface:

```typescript
const hre = require("hardhat");

async function main() {
  // Bob is the owner of Token contract and he wants to send some token amount to dave
  const bob = await hre.reef.getSignerByName("bob");
  const dave = await hre.reef.getSignerByName("dave");

  // Extracting user addresses
  const bobAddress = await bob.getAddress();
  const daveAddress = await dave.getAddress();

  // Token contract address
  const tokenAddress = "0x4a4fA87b810A30BE84a3a30318D6D9feEae126e5";

  // Retrieving Token contract from chain
  const token = await hre.reef.getContractAt("Token", tokenAddress, bob);

  // Let's see Dave's balance
  let daveBalance = await token.balanceOf(daveAddress);
  console.log(
    "Balance of to address before transfer:",
    await daveBalance.toString()
    );

  // Bob transfers 10000 tokens to Dave
  await token.transfer(daveAddress, 10000);

  // Let's once again check Dave's balance
  daveBalance = await token.balanceOf(daveAddress);
  console.log(
    "Balance of to address after transfer:",
    await daveBalance.toString()
  );

  // Bob's amount after transactions
  const bobBalance = await token.balanceOf(bobAddress);
  console.log(
    "Balance of from address after transfer:",
    await bobBalance.toString()
  );
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
```


## Next steps

Congratulations. You now have a running development node with the first smart contract. You and now are ready to start building your first DeFi app.

### Deploy on testnet
Deploying on testnet is similar to development, however there are 2 things we need.

1.) Run a testnet node or use the public [testnet RPC](/docs/developers/networks/#testnet-info-sheet).

2.) Create an [account](/docs/developers/accounts/) and obtain testnet REEF tokens from the faucet.

To get 1000 REEF testnet tokens simply type `!drip <YOUR_ADDRESS>` in [Reef matrix chat](https://app.element.io/#/room/#reef:matrix.org).

### Deploy on mainnet
Deploying on mainnet is similar to development, however there are 2 things we need.

1.) Run a mainnet node or use the public [mainnet RPC](/docs/developers/networks/#mainnet-info-sheet).

2.) Create an [account](/docs/developers/accounts/) and obtain Reef chain native REEF tokens from
the bridge or exchange.

### Verify the contract on Reefscan
Once you have deployed your smart contract on Reef Chain mainnet (or testnet), you will want to
verify it on Reefscan, so that other developers can access its source code and ABI.

Verify your deployed contract on: [Mainnet](https://reefscan.com/verifyContract) or [Testnet](https://testnet.reefscan.com/verifyContract)

### Build an UI
To start building an UI for your DApp, check the [UI examples repository](https://github.com/reef-defi/ui-examples). It provides components as well as [a single file React example](https://github.com/reef-defi/ui-examples/blob/master/packages/example-react/src/index.tsx), which contains most of the functionality you need to setup a DApp:
  - Polkadot.js extension connection
  - listing accounts in Polkadot.js extension
  - interacting with EVM contracts on the Reef Chain
  - generating Substrate addresses


[evm-canvas-ui](https://github.com/reef-defi/evm-canvas-ui) is a more sophisticated UI interacting with the Reef chain. It is built on top of Polkadot.js apps and uses `evm-provider.js` library to deploy and interact with the contracts.
