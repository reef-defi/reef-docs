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
{{< btn-copy text="git clone --recurse-submodules https://github.com/reef-defi/reef-chain" >}}
```bash
git clone https://github.com/reef-defi/reef-chain
```
{{< btn-copy text="cd reef-chain" >}}
```bash
cd reef-chain
```

### Install Rust
You can install the compiler and the toolchain with:
{{< btn-copy text="make init" >}}
```bash
make init
```

### Start a development node

The `make run` command will launch a temporary node and its state will be discarded after you terminate the process.

To run the temporary node with Ethereum compatibility enabled run:
{{< btn-copy text="make eth" >}}
```bash
make eth
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
We can use [NPM](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/) to install and run packages required for deploying and running Solidity smart contracts on Reef chain.


### Clone the examples repo
{{< btn-copy text="git clone https://github.com/reef-defi/reef-examples" >}}
```bash
git clone https://github.com/reef-defi/reef-examples
```

We are going to pick a simple Flipper contract for our demo, but feel free to
try out other examples as well.
{{< btn-copy text="cd reef-examples/flipper" >}}
```bash
cd reef-examples/flipper
```

### Install the dependencies
{{< btn-copy text="npm i" >}}
```bash
npm i
```

Alternatively you can also use `yarn`.

### Deploy the contract
{{< btn-copy text="npm run deploy" >}}
```bash
npm run deploy
```
Alternatively you can also use `yarn deploy`.

## Interact with a smart contract

There are multiple ways to interact with the contract.
On the lowest level, we can use a web3 client such as xxx (Python) and xxx (TypeScript).
On the high level we can use a UI, such as polkadot.js, a block explorer or a
custom UI for a specific contract.

Here is how we can call a function on our smart contract in TypeScript:

```typescript
// TODO
```

Now lets call the same function through the UI:

// TODO


## Next steps

Congratulations. You now have a running development node with the first smart contract. You and now are ready to start building your first DeFi app.
