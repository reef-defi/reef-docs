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


## Deploy a smart contract

### Requirements

The Reef chain smart contracts are written in [Solidity](https://docs.soliditylang.org/en/v0.8.2/).
We can use [yarn](https://yarnpkg.com/) to install and run packages required for deploying and running Solidity smart contracts on Reef chain. In the following example we use our plugin for a [Hardhat development environment](https://hardhat.org/) to deploy and interact with the contract.


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

### Deploy the contract
We are going to pick a simple Flipper contract for our demo, but feel free to
try out other examples as well.

{{< btn-copy text="npx hardhat run scripts/flipper/deploy.js" >}}
```bash
npx hardhat run scripts/flipper/deploy.js
```

## Interact with a smart contract

After the contract is deployed you can interact with the contract. In our Flipper example the value in the Flipper contract can be flipped by calling:

{{< btn-copy text="npx hardhat run scripts/flipper/flip.js" >}}
```bash
npx hardhat run scripts/flipper/flip.js
```

There are multiple ways to interact with the contract.
On the lowest level, we can use a web3 client such as [bodhi.js](https://github.com/AcalaNetwork/bodhi.js) (TypeScript). We recommend using our [hardhat-reef plugin](https://www.npmjs.com/package/@reef-defi/hardhat-reef), which abstracts many of the intricacies.

Here is how we can call a function on our smart contract in TypeScript:

```typescript
async function main() {
  const flipperAddress = "0x0230135fDeD668a3F7894966b14F42E65Da322e4";

  await hre.reef.getContractFactory("Flipper");

  const artifact = await hre.artifacts.readArtifact("Flipper");
  const flipper = new ethers.Contract(
    flipperAddress,
    artifact.abi,
    await hre.reef.getSigner()
  );

  // Call flip()
  console.log("Current value:", await flipper.get());
  await flipper.flip();
  console.log("New value after flip():", await flipper.get());
}
```

On the high level we can use a UI, such as polkadot.js, a block explorer or a
custom UI for a specific contract.

## Next steps

Congratulations. You now have a running development node with the first smart contract. You and now are ready to start building your first DeFi app.
