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

### Start the validator node
Here is the sample command for spinning up a `--validator` node on Reef mainnet.

```bash
./reef-node \
  --validator \
  --chain mainnet \
  --base-path /reef/validator \
  --execution=wasm \
  --port 30333 \
  --ws-port 9944 \
  --rpc-port 9933 \
  --rpc-methods Unsafe \
  --no-mdns \
  --no-private-ipv4 \
  --no-prometheus \
  --no-telemetry \
  --name MyValidatorNode
```

Note the `--rpc-methods Unsafe` flag. This flag is necessary to enable key management endpoints for validator setup, however it should be omitted once the validator is configured (see systemd snippet below). Do not expose wallet rpc endpoints to the public internet. You are solely responsible for adequate operational security.

### Setup the session keys
{{< alert icon="🔥" text="Make sure to only execute the `author_*` commands against a secure RPC node that you own. Failure to do so may result in loss of funds." >}}

There are 2 options for setting the session keys:

 1.) Let the node generate the keys via `author_rotateKeys` (recommended)

 2.) Generate them offline and submit via `author_setKeys`


#### Option 1: Generating the keys within node
Generating the session keys (or rotating the keys) can be done by calling the `author_rotateKeys` RPC method.

```
curl http://localhost:9933 -H "Content-Type: application/json" -d \
  '{"id":1, "jsonrpc":"2.0", "method": "author_rotateKeys", "params":[]}'
```

The result will be hex encoded:
```
{'jsonrpc': '2.0',
 'result': '0x4e0f43d7196b342b86c634ce6b1797e49e4ba5f01763324f2c9960ea33899561de3a616370becc71cb01775dc938f69d17b1ee0a4fd1689ede79c107f24b224c',
 'id': 7}
```
The result needs to be submitted as `session.setKeys(keys, proof)` extrinsic call from the
**controller** account. See bonding section below.

![example](https://i.imgur.com/LKR6q9w.png)

{{< alert icon="⚠️" text="The validator node will need to be restarted after setting or changing GRANDPA keys." >}}


#### Option 2: Inserting existing authoring keys
This is useful if our authoring keys have been generated via [keypair](/docs/developers/accounts/#generate-a-keypair) tool.
Note that the GRANDPA keys are ED25519, while the rest are SR25519.

We can then submit the seed+key derivation paths (suri) and the corresponding AccountId's for all session keys via RPC:
```
curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d \
  '{ "jsonrpc":"2.0", "id":1, "method":"author_insertKey", "params": [ "babe", "seed//babe", "0x..." ] }'

curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d \
  '{ "jsonrpc":"2.0", "id":1, "method":"author_insertKey", "params": [ "babe", "seed//grandpa", "0x..." ] }'

curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d \
  '{ "jsonrpc":"2.0", "id":1, "method":"author_insertKey", "params": [ "imon", "seed//im-online", "0x..." ] }'

curl http://localhost:9933 -H "Content-Type:application/json;charset=utf-8" -d \
  '{ "jsonrpc":"2.0", "id":1, "method":"author_insertKey", "params": [ "audi", "seed//authority-discovery", "0x..." ] }'
```

{{< alert icon="⚠️" text="The validator node will need to be restarted after setting or changing GRANDPA keys." >}}

### Optional: Systemd service
To automatically start Reef node on boot, you can create a systemd service from the following template:
```
[Unit]
Description=Reef Validator

[Service]
ExecStart=/bin/reef-node --base-path /reef/validator --validator --chain mainnet --execution=wasm --port 30333 --no-private-ipv4 --no-mdns --no-prometheus --no-telemetry --name MyValidatorNode
Restart=always
RestartSec=120

[Install]
WantedBy=multi-user.target
```

Make sure to configure the `--base-path` and the sentry nodes if you have them (recommended).

### Bonding
This step is the same as when [bonding](/docs/governance/nominators/#bonding) as Nominators.

Note: The controller account set via `staking.bond` should be used to make the `session.setKeys` and `session.validate` calls as well.

### Validating
The last step is to signal that we want to become a validator. This is done by calling `session.validate(ValidatorPreferences)`.

![example](https://i.imgur.com/77juTZY.png)

{{< alert icon="💡" text="To withdraw your validator commitment simply call `session.chill()`. It will take effect in the next era." >}}

Congratulations, you are now a candidate validator. To become elected as a Validator you will need to get Nominator's support and win the elections. Good luck!

### Identity
To make yourself identifiable to the Reef community you can set on-chain identity for your address. The identity consists of display name, as well as socials (website, twitter, etc).

The easiest way to set the idenitity is via the [console](https://console.reefscan.com/#/accounts).

After you've set the on-chain idenity, call `identity.requestJudgement()` with the fee of 100 REEF.
The identities are verified by the "[REEF ECOSYSTEM](https://reefscan.com/account/5CfqnipK6v9zz3zGC8joQE1hSuxczLaFcNtBhDcKWVw2W1a8)" registrar.
To complete the "KYC" and get your identitiy approved by the registrar, please write into the `#reef:matrix.org` [group](https://app.element.io/#/room/#reef:matrix.org).

*Note: "REEF ECOSYSTEM" registrar is only available for current and prospecting validators, as well
as established projects deploying on Reef chain. The registrar is not available for verifying
regular accounts for "KYC" purposes.*

