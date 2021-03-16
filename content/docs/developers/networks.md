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

## Reef testnet
Reef testnet is a common playground for deploying and testing DeFi apps. It is
recommended to first deploy apps on the testnet before launching on mainnet.

{{< alert icon="⚠️" text="Reef testnet tokens have no value and may be destroyed on testnet upgrades." >}}

### Testnet info sheet

```
| Key                 | Value                               |
| ------------------- | ----------------------------------- |
| SS58 Format         | 42                                  |
| Currency, Precision | REEF, 10                            |
| Block authoring     | BABE                                |
| Finality            | GRANDPA                             |
| Block Time          | 10s                                 |
| HTTP RPC            | https://rpc-testnet.reefscan.com    |
| Websocket           | wss://rpc-testnet.reefscan.com/ws   |
```

### Polkadot.js UI
You can connect to the testnet UI [here](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc-testnet.reefscan.com%2Fws#/explorer).

The `types.json` file for the block explorer UI can be found [here](https://github.com/reef-defi/reef-chain/blob/master/types.json).

*To set the types.json go to Developer > Settings. [example](https://i.imgur.com/ShfG9v7.png)*

### Start a testnet RPC node
```
./reef-node \
  --base-path /tmp/reefnode \
  --chain testnet \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --rpc-methods Auto \
  --rpc-cors all \
  --rpc-external \
  --ws-external \
  --name RPCNode
```

To make the node archival (for use with indexers or block explorers) add the `--pruning=archive` flag.

To make the node expose wallet related methods add `--rpc-methods Unsafe` flag.

## Reef mainnet
The mainnet is not live yet.

## Block explorer
The block explorer for both networks is available at:

[https://reefscan.com](https://reefscan.com)
