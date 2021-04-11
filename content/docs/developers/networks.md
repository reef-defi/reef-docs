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
weight: 220
toc: true
---


## Reef mainnet
The mainnet is not live yet.

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

## Start a RPC node
If you have downloaded the official binary for Ubuntu 20.04 from [GitHub Releases](https://github.com/reef-defi/reef-chain/releases), you can run it like so:
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

**If the binary is not available for your platform, you can compile it yourself:**

Use `git tag` to list all available release builds:
```bash
git tag
```

To create a corresponding production build, first checkout the tag:
```bash
git checkout testnet-2.1
```

Then run this command to install appropriate compiler version and produce a binary:
```bash
make release
```

The last thing we need is the `chain_spec.json` file. Compiling WASM is not deterministic, yet
we want to have matching genesis. The `chain_spec.json` file can be downloaded from [GitHub Releases](https://github.com/reef-defi/reef-chain/releases) as well.

Now we can start our node like so:
```
./reef-node \
  --base-path /tmp/reefnode \
  --chain chain_spec.json \
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

