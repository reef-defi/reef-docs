---
title: "Networks"
description: "Reef chain networks"
lead: "Reef chain's networks."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "developers"
weight: 230
toc: true
---


## Reef mainnet
**The mainnet is not live yet.**

### Mainnet info sheet

```
| Key                 | Value                               |
| ------------------- | ----------------------------------- |
| Currency, Precision | REEF, 18                            |
| EVM chain id        | 13939                               |
| SS58 Format         | 42                                  |
| Block authoring     | BABE                                |
| Finality            | GRANDPA                             |
| Block Time          | 10s                                 |
| Block Size          | 1.25MB to 3.75MB                    |
| HTTP RPC            | https://rpc.reefscan.com            |
| Websocket           | wss://rpc.reefscan.com/ws           |
```

## Reef testnet
Reef testnet is a common playground for deploying and testing DeFi apps. It is
recommended to first deploy apps on the testnet before launching on mainnet.

{{< alert icon="⚠️" text="Reef testnet tokens have no value and may be destroyed on testnet upgrades." >}}

### Testnet info sheet

```
| Key                 | Value                               |
| ------------------- | ----------------------------------- |
| Currency, Precision | REEF, 18                            |
| EVM chain id        | 13939                               |
| SS58 Format         | 42                                  |
| Block authoring     | BABE                                |
| Finality            | GRANDPA                             |
| Block Time          | 10s                                 |
| Block Size          | 1.25MB to 3.75MB                    |
| HTTP RPC            | https://rpc-testnet.reefscan.com    |
| Websocket           | wss://rpc-testnet.reefscan.com/ws   |
```

