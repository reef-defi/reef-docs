---
title: "Reef chain v8 is live on mainnet"
description: "Reef v8 has been activated on mainnet on block 1408888"
lead: ""
date: 2021-11-01T00:00:00+02:00
lastmod: 2021-11-01T00:00:00+02:00
draft: false
weight: 50
images: []
contributors: ["Reef Developers"]
---

Reef v8 has been activated on mainnet on block [1408888](https://reefscan.com/block/?blockNumber=1408888).

**Reef v8 is the largest blockchain upgrade since the Reef mainnnet launch on May 28th 2021**:
 - Upgraded from Polkadot Substrate 3 to Substrate 4
 - Upgraded Ethereum virtual machine (EVM) to post London fork
 - Support for live chain forking in development mode
 - Support for blockchain database snapshots /w Postgres mirroring
 - Improved integration tests suite


![](postgres.png)

## Upgrade instructions
This release also requires node upgrade.
All users must upgrade the their Reef node to 4.0.0. Chain re-sync is not required.

```
git checkout v8
make release
```

You can read more about the release [here](https://github.com/reef-defi/reef-chain/releases/tag/v8).
Full node compilation instructions are available [here](/docs/developers/nodes/).

## Ecosystem improvements
Developers need to upgrade to the latest versions of our [JavaScript](/docs/developers/js_libraries/) and [Python](https://github.com/reef-defi/py-reef-interface#readme) libraries.

## Ledger Nano support
Reef chain firmware for Ledger hardware wallets is now available on [GitHub](https://github.com/reef-defi/ledger-reef). The firmware supports native Reef chain transactions related to REEF transfers, bonding and NPoS governance.

![](ledger.jpeg)

A user friendly UI with Ledger support is currently in development.

## Developer support
We are inviting developers to join us in [Reef's Discord server](https://discord.gg/invite/DHpr7sCeGa) with any questions related to Reef chain. Make sure to verify your account, then select the Builder role in `📋┊start-here`. You will get access to the Reef chain testnet faucet in `🚰┊faucet`.
