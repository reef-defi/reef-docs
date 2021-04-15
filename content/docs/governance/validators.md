---
title: "Validators"
description: "Reef chain validators"
lead: "Become a Reef chain validator and help secure the future of Reef network."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "governance"
weight: 330
toc: true
---


## Who is this for?
Becoming a Validator is a serious responsibility that requires strong technical skills and quick problem solving ability as the baseline. Validators are elected by Nominators, so strong communication skills and ecosystem contributions are a good traits as well.

Validators's core responsibility is reliable block production. However in practice becoming a validator is a highly competitive game, and as a result Validators may also provide additional services to the community, such as:
 - host Reef chain boot nodes
 - host fast sync block export/import infrastructure
 - develop and maintain DevOps tooling
 - develop and maintain security tooling
 - develop and maintain block explorers, RPC nodes, SQL indexers and other
   public good infrastructure

## Become a validator
This guide only covers setting up the validator node, however there is a lot more to being a validator than running a node.

### Bonding
The first step is bonding (staking) Reef tokens. This step is the same as [bonding](/docs/governance/nominators/#bonding) for Nominators.

### Start the validator node
Here is the sample command for spinning up a `--validator` node on `testnet`.

```bash
./reef-node \
  --base-path /tmp/reefnode \
  --chain testnet \
  --validator \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --rpc-methods Unsafe \
  --no-telemetry \
  --no-mdns \
  --no-telemetry \
  --name MyValidatorNode
```

Note the `--rpc-methods Unsafe` flag. This flag is necessary to enable key management endpoints. Do not expose these endpoints to the public internet. You are solely responsible for adequate operational security.


### Setup the session keys
{{< alert icon="🔥" text="Make sure to only execute the `author_*` commands against a secure RPC node that you own. Failure to do so may result in loss of funds." >}}

There are 2 options for setting the session keys:

 1.) Generate them offline and submit via `author_setKeys`

 2.) Let the node generate the keys via `author_rotateKeys`

#### Generating the keys by hand
To generate the keys by hand we can use our [keypair](/docs/developers/accounts/#generate-a-keypair) tool. Note that the GRANDPA keys are ED25519, while the rest are SR25519.

We can then submit the seed+key derivation paths (suri) and the corresponding AccountId's for BABE and GRANDPA keys via RPC:
```
curl http://localhost:9934 -H "Content-Type:application/json;charset=utf-8" -d \
  '{ "jsonrpc":"2.0", "id":1, "method":"author_insertKey", "params": [ "babe", "seed//babe", "0x..." ] }'

curl http://localhost:9934 -H "Content-Type:application/json;charset=utf-8" -d \
  '{ "jsonrpc":"2.0", "id":1, "method":"author_insertKey", "params": [ "babe", "seed//grandpa", "0x..." ] }'
```

{{< alert icon="⚠️" text="The validator node will need to be restarted after setting or changing GRANDPA keys." >}}

#### Generating the keys within node
Generating the session keys (or rotating the keys) can be done by calling the `author_rotateKeys` RPC method.

The result will be hex encoded:
```
{'jsonrpc': '2.0',
 'result': '0x4e0f43d7196b342b86c634ce6b1797e49e4ba5f01763324f2c9960ea33899561de3a616370becc71cb01775dc938f69d17b1ee0a4fd1689ede79c107f24b224c',
 'id': 7}
```
The result needs to be submitted as `session.setKeys(keys, proof)` extrinsic call.

![example](https://i.imgur.com/LKR6q9w.png)


### Validating
The last step is to signal that we want to become a validator. This is done by calling `session.validate(ValidatorPreferences)`.

![example](https://i.imgur.com/77juTZY.png)

{{< alert icon="💡" text="To withdraw your validator commitment simply call `session.chill()`. It will take effect in the next era." >}}

Congratulations, you are now a candidate validator. To become elected as a Validator you will need to get Nominator's support and win the elections. Good luck!

