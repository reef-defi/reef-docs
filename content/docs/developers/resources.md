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

## Reef Chain

### Developer Console
You can connect to the developer UI for different networks:

[Mainnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc.reefscan.com%2Fws#/explorer) | [Testnet](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-testnet.reefscan.com%2Fws#/explorer) | [Local Node](https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944#/explorer)


{{< alert icon="💡" text="If you are opening the developer console for the first time you will need to setup Types and Metadata." >}}


The `types.json` file for the block explorer UI can be found [here](https://github.com/reef-defi/reef-chain/blob/master/assets/types.json).

To set the types.json go to Developer > Settings. [example](https://i.imgur.com/ShfG9v7.png)

The metadata syncing with the [Polkadot browser extension](https://polkadot.js.org/extension/) will be offered to you automatically. Just click accept.

### Block explorer
Reefscan is the main block explorer for Reef chain.

[Mainnet](https://reefscan.com) | [Testnet](https://testnet.reefscan.com)

### Remix IDE
Developers can use Remix IDE for Reef chain to quickly deploy and test their smart contracts.
[https://remix.reefscan.com](https://remix.reefscan.com/)

### EVM Playground
You can deploy and interact with smart contracts via EVM Playground UI:
[https://evm.reefscan.com](https://evm.reefscan.com)


## DeFi

### Learn Solidity
Solidity is a programming language for writing DeFi applications. The Solidity programs are compiled
and uploaded to Reef chain, where they run in a completely decentralized fashion.

Here are some great resources for learning Solidity:
 - The [official Solidity docs](https://docs.soliditylang.org)
 - Solidity [tutorial](https://www.tutorialspoint.com/solidity/index.htm) for programmers


Compiling, deploying and managing Solidity smart contracts by hand can be a chore. For this reason
we have developer frameworks for Python and JS/TypeScript.

### Reef for JS/TypeScript devs
Javascript developers can use our [HardHat integration](https://github.com/reef-defi/hardhat-reef) to develop and deploy smart contracts on Reef chain.

### Reef for Python devs
Reef ecosystem for Python is coming soon.


## Help
You can ask technical questions in our matrix.org developer [chat](https://app.element.io/#/room/#reef:matrix.org) room `#reef:matrix.org`.
