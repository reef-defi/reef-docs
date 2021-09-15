---
title: "Nodes"
description: "Running a Reef chain node"
lead: "Learn how to setup your Reef chain RPC node."
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

## Reef node
Here is a quick cheat-sheet with some of the most commonly used commands on the Reef node.

{{< alert icon="💡" text="To see all commands and their documentation run <b>./reef-node help</b>" >}}

### Compile the node
If you haven't yet, compile the Reef node locally:

Use `git tag` to list all available release builds:
```bash
git tag
```

To create a corresponding production build, first checkout the tag:
```bash
git checkout mainnet-1
```

Then run this command to install appropriate compiler version and produce a binary:
```bash
make release
```

## Start a development node
The development node will create a temporary, local chain.
```
./reef-node \
  --chain dev \
  --base-path /tmp/reefnode \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --rpc-methods Auto \
  --rpc-cors all \
  --rpc-external \
  --ws-external \
  --name MyDevNode
```


## Start a RPC node
The Reef chain mainnet or testnet RPC node can be started like so:
```
./reef-node \
  --chain mainnet \
  --base-path /reef/fullnode \
  --pruning=archive \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --rpc-methods Auto \
  --rpc-cors all \
  --rpc-external \
  --ws-external \
  --name MyRPCNode
```

To make the node archival (for use with indexers or block explorers) we use the `--pruning=archive` flag. Omit this flag to run the _light_ RPC node.

Additionally, you may want to configure nginx frontend with TLS. See an [example](https://github.com/reef-defi/reef-chain/blob/master/rpc/nginx.conf).


## Start a bootnode

Bootnodes are important for a healthy p2p network.

First we need to generate our persistent node key:
```bash
./reef-node key generate-node-key
```

This will output the public key and the private node key:
```
12D3KooWL4scSWDRaTNja1KH4Rnv7JxVKGkrSvr9XH8Li8yL5mCA
1ba5794ee476523629e84661ee0d28707bf43a486bb204633b789ffc226fe559
```

Now we can start our boot node with the generated private key by using the `--node-key` flag:

```
./reef-node \
  --chain mainnet \
  --base-path /reef/bootnode \
  --port 30333 \
  --node-key 1ba5794ee476523629e84661ee0d28707bf43a486bb204633b789ffc226fe559 \
  --name MyBootNode
```

This node can now be directly connected to via from another node via `--bootnodes` flag:

```bash
./reef-node \
  --chain mainnet \
  --base-path /reef/bootnode \
  --port 30333 \
  --bootnodes /ip4/<MyBootNode-IP-ADDRESS>/tcp/30333/p2p/12D3KooWL4scSWDRaTNja1KH4Rnv7JxVKGkrSvr9XH8Li8yL5mCA
  --name MyOtherNode
```

{{< alert icon="💡" text="You can also point a domain name to your node's ip and then reference it as /dns/bootnode.mydomain.com/tcp/30333/..." >}}


## Reset the chain state
To prune (reset) the node run:
```
./reef-node purge-chain --chain <dev/testnet/mainnet> --base-path /path/to/state
```
