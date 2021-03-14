---
title: "Networks"
description: "Reef chain networks"
lead: "Reef chain networks"
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "developers"
weight: 220
toc: true
---

## Reef Testnet
{{< alert icon="⚠️" text="Reef testnet tokens have no value and may be destroyed on testnet upgrades." >}}

Reef testnet is a common playground for deploying and testing DeFi apps. It is
recommended to first deploy apps on the testnet before launching on mainnet.

### Testnet Info Sheet

```
| Key                 | Value                               |
| ------------------- | ----------------------------------- |
| SS58 Format         | 42                                  |
| Currency, Precision | REEF, 10                            |
| Block authoring     | BABE                                |
| Finality            | GRANDPA                             |
| Block Time          | 10s                                 |
| HTTP RPC            | https://rpc-testnet.reef.finance    |
| Websocket           | wss://rpc-testnet.reef.finance/ws   |
```

The `types.json` file for the block explorer UI can be found [here](https://github.com/reef-defi/reef-chain/blob/master/types.json).


## Reef Mainnet
The mainnet is not live yet.

## Block explorer
The block explorer for both networks is available at:

[https://explorer.reef.finance](https://explorer.reef.finance)
