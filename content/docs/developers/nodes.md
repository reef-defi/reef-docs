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


## Start a RPC node
If you have downloaded the official binary for Ubuntu 20.04 from [GitHub Releases](https://github.com/reef-defi/reef-chain/releases), you can run it like so:
```
./reef-node \
  --chain mainnet \
  --base-path /tmp/reefnode \
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

**If the binary is not available for your platform, you can compile it yourself:**

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

The last thing we need is the `chain_spec.json` file. Compiling WASM is not deterministic, yet
we want to have matching genesis. The `chain_spec.json` file can be downloaded from [GitHub Releases](https://github.com/reef-defi/reef-chain/releases) as well.

Now we can start our node like so:
```
./reef-node \
  --chain chain_spec_mainnet.json \
  --base-path /tmp/reefnode \
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
  --base-path /tmp/reefnode \
  --port 30333 \
  --node-key 1ba5794ee476523629e84661ee0d28707bf43a486bb204633b789ffc226fe559 \
  --name MyBootNode
```

This node can now be directly connected to via from another node via `--bootnodes` flag:

```bash
./reef-node \
  --chain mainnet \
  --base-path /tmp/reefnode \
  --port 30333 \
  --bootnodes /ip4/<MyBootNode-IP-ADDRESS>/tcp/30333/p2p/12D3KooWL4scSWDRaTNja1KH4Rnv7JxVKGkrSvr9XH8Li8yL5mCA
  --name MyOtherNode
```

{{< alert icon="💡" text="You can also point a domain name to your node's ip and then reference it as /dns/bootnode.mydomain.com/tcp/30333/..." >}}

