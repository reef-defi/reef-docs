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

## Reef Apps
Reef comes with multiple open-source apps that are ready to use:
 - [Reefswap](https://reefswap.com) is a Uniswap V2 based DEX for Reef ([source](https://github.com/reef-defi/reefswap-ui))
 - [Reefscan](https://reefscan.com) is a block explorer for Reef ([source](https://github.com/reef-defi/reef-explorer))

## Reef Developer Ecosystem

### Developer Console
You can connect to the developer UI for different networks:

[Mainnet](https://console.reefscan.com/?rpc=wss%3A%2F%2Frpc.reefscan.com%2Fws#/explorer) | [Testnet](https://console.reefscan.com/?rpc=wss%3A%2F%2Frpc-testnet.reefscan.com%2Fws#/explorer)

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


## Developer SDKs

### Reef for JS/TypeScript devs
We provide multiple libraries to interact with the Reef chain. [reef.js](https://github.com/reef-defi/reef.js) can be used for both Substrate as well as EVM module interaction. An [evm-provider.js](https://github.com/reef-defi/evm-provider.js/commits/master) wrapper around the reef.js strives to make the EVM module interaction easier - it is compatible with the ethers.js library. The [hardhat-reef](https://github.com/reef-defi/hardhat-reef) plugin goes a step further and allows to be used in the Hardhat framework - you can easily compile/deploy/interact with the contracts in a single project.


See [the corresponding page](/docs/developers/js_libraries) describing JS libraries in detail.



### Reef for Python devs
Developers can use our [Python library](https://pypi.org/project/reef-interface/) to interact with Reef chain.

[Getting Started](https://github.com/reef-defi/py-reef-interface#readme) | [Documentation](https://reef-defi.github.io/py-reef-interface/reefinterface/index.html)


## Help
You can ask technical questions in our matrix.org developer [chat](https://app.element.io/#/room/#reef:matrix.org) room `#reef:matrix.org`.
