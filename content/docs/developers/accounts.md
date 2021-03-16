---
title: "Accounts"
description: "Reef chain accounts"
lead: "Create your Reef chain account and link it with your Ethereum address."
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

## Generate a keypair
You can create a new random mnemonic and corresponding SR25519 keypair with `reef-node key generate`:

```
./reef-node key generate --words 12
Secret phrase `any member stadium combine company grass jar wood brown second blame rocket` is account:
  Secret seed:      0xf087a7ebb71070624fede0758ed46facf24eddef78e2c4a96e3d0e6ee934472a
  Public key (hex): 0xf47813a285f917a3c2a3eae00d94f1158761ad5105261e98bceb050ad8871638
  Account ID:       0xf47813a285f917a3c2a3eae00d94f1158761ad5105261e98bceb050ad8871638
  SS58 Address:     5HbFDCvjGmG2VRae2KN6kkYbYaspfdGb5n7LtZf1FfqvNrDt
```

If you already have a mnemonic (seed) you can use it to generate obtain corresponding SR25519, ED25519 or ECDSA keypairs:
```
./reef-node key inspect-key --scheme Ed25519 \
  "any member stadium combine company grass jar wood brown second blame rocket"
Secret phrase `any member stadium combine company grass jar wood brown second blame rocket` is account:
  Secret seed:      0xf087a7ebb71070624fede0758ed46facf24eddef78e2c4a96e3d0e6ee934472a
  Public key (hex): 0x4457741669d11953becab1e7ec348eef0885db824dfc92cd3bc06b3efb167071
  Account ID:       0x4457741669d11953becab1e7ec348eef0885db824dfc92cd3bc06b3efb167071
  SS58 Address:     5DcK6KBAdwXeGuz5zKRPfkz1gT5bK4LRCWtFxhWqNwNVgKvL
```

{{< alert icon="💡" text="The SR25519 keypairs can also be generated with Polkadot.js extension." >}}

## EVM Account

Now that we have a Reef Chain account, we need to link an EVM account to it. This can be done by either generating an EVM address or importing an existing Ethereum address.

{{< alert icon="⚠️" text="Each Reef account can be linked to exactly one EVM account." >}}

Reef chain accounts are based on SR25519 and their public keys (addresses) are encoded with [SS58](https://github.com/paritytech/substrate/wiki/External-Address-Format-(SS58)). Example Reef chain address:
```
5H1tqyqYgBhYky3rMwjwmR9uNrBMrm45EaF8c9UQoxuVAmRA
```

Ethereum accounts are based on ECDSA and their public keys (addresses) are hex encoded. Example Ethereum or Reef chain EVM address:
```
0xebdcfce3377bd7593f14a4c70ed2974d55a1ab96
```

### Generating an EVM address

Generating an EVM address is easy. We just need to call the `evmAccounts.claimDefaultAccount()` function. This will create an EVM address for our Reef chain account.

To see our EVM address we can call `evmAccounts.evmAddresses(AccountId)`.

Check out [this guide](https://imgur.com/a/PcQ300l) on how to generate and check an EVM address within Polkadot.js UI.

### Linking an existing Ethereum address

If you would like to import your existing Ethereum address into Reef chain, and assign it to your Reef Account you can call the `evmAccounts.claimAccount(eth_address, eth_signature)` function. A valid signature is needed to verify the ownership of the address.


{{< alert icon="💡" text="The balances in Reef Account and its linked EVM address are invariant." >}}
