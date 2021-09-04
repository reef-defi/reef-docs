---
title: "Troubleshooting"
description: "Here are the common errors new developers encounter, and their solutions."
lead: "Here are the common errors new developers encounter, and their solutions."
date: 2020-11-12T15:22:20+01:00
lastmod: 2020-11-12T15:22:20+01:00
draft: false
images: []
menu:
  docs:
    parent: "help"
weight: 620
toc: true
---

### FATAL: Unable to initialize the API: createType(CurrencyId):: Cannot construct unknown type CurrencyId
You are missing the [types.json](https://github.com/reef-defi/reef-chain/blob/master/assets/types.json) file in the developer settings of the console.

[>> Docs](/docs/developers/resources/#developer-console)

### Bootnode with peer id ... is on a different chain (our genesis: 0x... theirs: 0x...)
If you compiled the `reef-node` binary older than v3.0.1 then you will have to initialize the chain with the chain
spec file, since the WASM builds are not deterministic. Just do `--chain chain_spec_mainnet_raw.json`.

The new Reef node has genesis bytecode embedded and no longer needs manual imports of the chain spec file.

[>> Chain spec download](https://github.com/reef-defi/reef-chain/tree/master/assets)

### Error: Service(Keystore(Io(Os { code: 13, kind: PermissionDenied, message: "Permission denied" })))
Make sure `--base-path` is set to a folder you have appropriate permissions for. Either update the
permissions of your account to access `/reef/validator` (documentation default) or change
`--base-path` to somewhere else (ie. your home folder).

### Weird database errors
Whenever you run into some sort of database error, `purge-cache` subcommand is your friend.
You can do a fresh sync after the purge, and things should just work.

[>> Docs](/docs/developers/resources/#reset-the-local-chain)


