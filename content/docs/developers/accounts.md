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
weight: 240
toc: true
---

## New accounts

{{< alert icon="⚠️" text="To create a Reef chain account, you need to deposit at least 1 REEF into it. This is also known as existential deposit." >}}

Each account has a unique private key. You should never share your private key or seed with anyone.

You can create a new random mnemonic and corresponding SR25519 keypair with `reef-node key generate`:

```
./reef-node key generate --words 12
Secret phrase `any member stadium combine company grass jar wood brown second blame rocket` is account:
  Secret seed:      0xf087a7ebb71070624fede0758ed46facf24eddef78e2c4a96e3d0e6ee934472a
  Public key (hex): 0xf47813a285f917a3c2a3eae00d94f1158761ad5105261e98bceb050ad8871638
  Account ID:       0xf47813a285f917a3c2a3eae00d94f1158761ad5105261e98bceb050ad8871638
  SS58 Address:     5HbFDCvjGmG2VRae2KN6kkYbYaspfdGb5n7LtZf1FfqvNrDt
```

The `SS58 Address` is your wallet address. You can use it to receive payments and check your balances/activity on the block explorer.

## Inspect an existing account

If you already have a mnemonic or random seed you can use it to obtain corresponding SR25519, ED25519 or ECDSA keypairs:
```
./reef-node key inspect --scheme sr25519 \
  "any member stadium combine company grass jar wood brown second blame rocket"
Secret phrase `any member stadium combine company grass jar wood brown second blame rocket` is account:
  Secret seed:      0xf087a7ebb71070624fede0758ed46facf24eddef78e2c4a96e3d0e6ee934472a
  Public key (hex): 0xf47813a285f917a3c2a3eae00d94f1158761ad5105261e98bceb050ad8871638
  Account ID:       0xf47813a285f917a3c2a3eae00d94f1158761ad5105261e98bceb050ad8871638
  SS58 Address:     5HbFDCvjGmG2VRae2KN6kkYbYaspfdGb5n7LtZf1FfqvNrDt
```


## EVM Account

Now that we have a Reef Chain account, we need to link an EVM account to it. This can be done by either generating a default EVM address or importing an existing Ethereum address.

{{< alert icon="⚠️" text="Each Reef account can be linked to exactly one EVM account." >}}

Reef chain accounts are based on SR25519 and their public keys (addresses) are encoded with [SS58](https://github.com/paritytech/substrate/wiki/External-Address-Format-(SS58)). Example Reef chain address:
```
5H1tqyqYgBhYky3rMwjwmR9uNrBMrm45EaF8c9UQoxuVAmRA
```

Ethereum accounts are based on ECDSA and their public keys (addresses) are hex encoded. Example Ethereum or Reef chain EVM address:
```
0xebdcfce3377bd7593f14a4c70ed2974d55a1ab96
```

### Generating a default EVM address

Generating an EVM address is easy. All we have to do is use EVM, and an address will be auto-generated and assigned to our account on the first EVM call.

Alternatively, we can generate the default address manually by calling the `evmAccounts.claimDefaultAccount()` function. This will create an EVM address for our Reef chain account.

To see our EVM address we can call `evmAccounts.evmAddresses(AccountId)`.

Check out [this guide](https://imgur.com/a/PcQ300l) on how to generate and check an EVM address within EVM playground UI.

### Linking an existing Ethereum address

If you would like to import your existing Ethereum address into Reef chain, and assign it to your Reef Account you can call the `evmAccounts.claimAccount(eth_address, eth_signature)` function. A valid signature is needed to verify the ownership of the address.


{{< alert icon="💡" text="The balances in Reef Account and its linked EVM address are invariant." >}}

## Developer accounts
For developers working on
Developers working on Reef chain (starting the node with `--dev` flag) also have access to standard
dummy accounts (alice, bob, charlie...) that are pre-funded with REEF.

The mnemonic for those accounts is:
`bottom drive obey lake curtain smoke basket hold race lonely fit walk`

Accounts can be accessed through subkey scheme like so:
```
bottom drive obey lake curtain smoke basket hold race lonely fit walk//Alice
bottom drive obey lake curtain smoke basket hold race lonely fit walk//Bob
...
```

{{< alert icon="💡" text="Note the // notation. This is used to generate hierarchical deterministic (HD) wallets." >}}

## Testnet accounts
To create a testnet account simply generate a key with the extension or Reef node cli.


Then request testnet REEF from the chatbot.  To get 1000 REEF testnet tokens simply type `!drip YOUR_SS58_ADDRESS` in the [Reef matrix chat](https://app.element.io/#/room/#reef:matrix.org).
