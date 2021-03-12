---
title: "Quick Start"
description: "Get quickly up and running with Reef chain"
lead: "Get quickly up and running with Reef chain"
date: 2020-11-16T13:59:39+01:00
lastmod: 2020-11-16T13:59:39+01:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 110
toc: true
---

In this guide we will deploy a simple Flipper smart contract onto Reef chain.

The guide is broken into 3 parts:
 - setting up the Reef development node
 - setting up the smart contract development environment
 - deploying and running your first smart contract


# Setting up a development node

## Requirements

Reef chain is written in [Rust →](https://www.rust-lang.org/). A basic familiarity with Rust tooling is required.


### Clone the repo
```bash
git clone https://github.com/reef-defi/reef-chain

cd reef-chain
```

### Install Rust
You can install the compiler and the toolchain with:
```bash
make init
```

### Start a development node

The `make run` command will launch a temporary node and its state will be discarded after you terminate the process.
```bash
make run
```
To run the temporary node with Ethereum compatibility enabled run:
```bash
make eth
```

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

Start the development chain with detailed logging:

```bash
RUST_LOG=debug RUST_BACKTRACE=1 ./target/release/reef-node -lruntime=debug --dev
```


# Deploy a smart contract

## Requirements

The Reef chain smart contracts are written in [Solidity →](https://docs.soliditylang.org/en/v0.8.2/).


### Clone the examples repo
```bash
git clone https://github.com/reef-defi/reef-examples

cd reef-examples
```

# Interact with a smart contract

(polkadot.js ui todo)

### Next steps

Congratulations. You now have a running development node with the first smart contract. You and now are ready to start building your first DeFi app.
