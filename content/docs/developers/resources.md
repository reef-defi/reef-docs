---
title: "Resources"
description: "Reef chain developer tools and resources."
lead: "Reef chain developer tools and resources."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "developers"
weight: 210
toc: true
---


## Learn Solidity
Solidity is a programming language for writing DeFi applications. The Solidity programs are compiled
and uploaded to Reef chain, where they run in a completely decentralized fashion.

Here are some great resources for learning Solidity:
 - The [official Solidity docs](https://docs.soliditylang.org)
 - Solidity [tutorial](https://www.tutorialspoint.com/solidity/index.htm) for programmers


Compiling, deploying and managing Solidity smart contracts by hand can be a chore. For this reason
we have developer frameworks for Python and JS/TypeScript.


## Reef for Python devs
Reef ecosystem for Python is coming soon.

## Reef for JS/TypeScript devs
Javascript developers can use our [HardHat integration](https://github.com/reef-defi/hardhat-reef-chain) to develop and deploy smart contracts on Reef chain.

## Reef node commands
Here is a quick cheat-sheet with some of the most commonly used commands on the Reef node.

{{< alert icon="💡" text="To see all commands and their documentation run <b>./reef-node help</b>" >}}


### Compile the node
If you haven't yet, compile the Reef node locally:
```
make build && cd target/release
```

### Start the local development node
```
./reef-node \
  --base-path /tmp/reefnode \
  --chain dev \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --rpc-methods Auto \
  --rpc-cors all \
  --rpc-external \
  --ws-external \
  --name MyNode
```

To prune (reset) the node run:
```
./target/release/reef-node purge-chain --chain dev
```

### Generate a keypair
You can follow the [accounts](/docs/developers/accounts/#generate-a-keypair) guide on how to use the CLI to make new keypairs.


## Reef UI
TODO

## Block explorer
The block explorer for Reef all chain networks is available at:

[https://reefscan.com](https://reefscan.com)

## Help
You can ask technical questions in our matrix.org developer [chat](https://app.element.io/#/room/#reef:matrix.org) room `#reef:matrix.org`.
