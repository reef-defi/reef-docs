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

{{< alert icon="💡" text="To see all commands and their documentation run <b>./reef-node help</b>" >}}

### Requirements

Reef chain is written in [Rust](https://www.rust-lang.org/). A basic familiarity with Rust tooling is required.


### Clone the repo
{{< btn-copy text="git clone --recursive https://github.com/reef-defi/reef-chain" >}}
```bash
git clone --recursive https://github.com/reef-defi/reef-chain
```

### Install Rust
If you don't have [Rust](https://www.rust-lang.org/tools/install) already, you can install it with:
{{< btn-copy text="curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh" >}}
```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

You can install the development compiler and the toolchain with:
{{< btn-copy text="make init" >}}
```bash
make init
```

### Start a local development node

{{< alert icon="💡" text="If you are only interested in using Reef chain (ie. to deploy your dapps), consider running the RPC node instead." >}}

The `make run` command will launch a temporary node and its state will be discarded after you terminate the process.

To run the temporary (development) node run:
{{< btn-copy text="make run" >}}
```bash
make run
```

You should see an output like [this](https://i.imgur.com/Dst10UI.png) when the Reef chain starts producing blocks.


Use the following command to build the node without launching it:

```bash
make build
```

This command will start the single-node development chain:

```bash
cd target/release
./reef-node --dev
```

Purge the development chain's state:

```bash
./reef-node purge-chain --dev
```


### Configure your local development node
Here are some of the common arguments you can use for a local node:
```bash
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

## Start a validator node
Please check our [validator](/docs/governance/validators/) guide on how to setup and configure a
validator node.

## Reset the chain state
To prune (reset) the node run:
```
./reef-node purge-chain --chain <dev/testnet/mainnet> --base-path /path/to/state
```
