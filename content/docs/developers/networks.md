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
**The Reef mainnet is launching in "canary" mode.** Reef chain is not just an Ethereum clone - is built from ground up with new blockchain
technology, and as such carries risk of potentially disruptive bugs.

While Reef chain is in "canary" mode, the chain might have to be reset in the event of catastrophic failure,
and only REEF account balances at the time of the failure would carry over to the new network.

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

