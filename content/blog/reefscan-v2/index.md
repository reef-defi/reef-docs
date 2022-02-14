---
title: "Reefscan v2 is live"
description: "Reefscan v2 is live. Reefscan is the main Reef chain block explorer."
lead: ""
date: 2022-02-13T07:45:48+02:00
lastmod: 2022-02-13T07:45:48+02:00
draft: true
weight: 50
images: []
contributors: ["Reef Developers"]
---

[Reefscan](https://reefscan.com) is the most advanced, fully open-source block explorer. It was built from
the ground up for Reef chain.

## Reefscan UI Redesign
Reefscan has a new look. It is the first Reef project utilizing components from the upcoming [Reef UI kit](/docs/developers/ui_kit/).

![](redesign.png)

## ERC-20 Token Tracker
Reefscan will now automatically label and track smart contracts that satisfy ERC-20 contract interface.
This gives users the full transparency into token holdings and token movements on chain.

## NFT support
Reefscan now supports NFTs as well. This makes it easy for developers to integrate NFTs into their
apps and wallets.

## Improved smart contract support
Smart contracts now have a full source viewer and improved verification API. The verification APIs
are also integrated into [Reef Hardhat](https://github.com/reef-defi/hardhat-reef) and [Remix](https://remix.reefscan.com).

## The Graph
Reefscan's choice of database backend - Postgres - will certainly delight many software engineers.
However we have received a lot of requests for "The graph". Fortunately, Reefscan also ships with
Hasura GraphQL server which makes it easy for web developers to connect with Reef chain
data warehouse for faster and more powerful frontend applications.

Check out the [Reefscan documentation](/docs/developers/reefscan/) to learn how to access the graph
and other features.

## Reefscan APIs
Reefscan also ships with multiple API endpoints that are useful for quickly implementing wallet
enhancements and other features:
 - An API for listing user's tokens and balances
 - An API for historic trading data on Reefswap DEX
 - An API for programmatically verifying smart contracts source code
 - An API for retreiving verified smart contract's source and ABI
 - and [more](/docs/developers/reefscan/#smart-contracts)

## Full Docker support
Reefscan runs in Docker. Setting up the full Reefscan stack (Postgres, GraphQL, Backend & Frontend) is as easy as:
```
yarn
make net=mainnet env=prod up
```
Check out the full [documentation](https://github.com/reef-defi/reef-explorer#readme) on setting up and configuring your own, production grade instance of Reefscan.
