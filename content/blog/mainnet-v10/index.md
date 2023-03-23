---
title: "Reef v10 is live on mainnet"
description: "Reef v10 has been activated on mainnet on block 3780541"
lead: ""
date: 2022-08-04T00:00:00+02:00
lastmod: 2022-08-04T00:00:00+02:00
draft: false
weight: 50
images: []
contributors: ["Reef Developers"]
---

Reef v10 features and improvements:
 - on chain multisig accounts
 - transaction batching support (via Utilities pallet)
 - consistent storage costs across all modules
 - removed storage deposit refund when destructing live contracts
 - enhanced EVM events with additional metadata (caller/maintainer and gas usage)
 - ability to create forks in development mode


## Upgrade instructions
The node operators do not need to do anything, because this release will be rolled out as an on-chain runtime upgrade.

**All apps must upgrade the their [evm-provider](https://github.com/reef-defi/evm-provider.js) to the latest version on NPM.
All apps interacting with EVM must upgrade their event processing logic according with the v10 [changes](https://github.com/reef-defi/reef-chain/pull/63/files#diff-db3ad6e2ca083dd40dacda29e393a5d47e24baae25ab8efcab355ed09c5db7efL307-R344).**

## Live rollout
- Testnet upgraded to v10 on block [3,679,422](https://testnet.reefscan.com/block?blockNumber=3679422).
- Mainnet upgraded to v10 on block [3,780,541](https://reefscan.com/block/?blockNumber=3780541).

## Developer support
We are inviting developers to join us in [Reef's Discord server](https://discord.gg/invite/DHpr7sCeGa) with any questions related to Reef chain. Make sure to verify your account, then select the Builder role in `📋┊start-here`. You will get access to the Reef chain testnet faucet in `🚰┊faucet`.
