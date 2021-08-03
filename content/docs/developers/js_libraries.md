---
title: "JS libraries"
description: "Reef chain JS libraries."
lead: "We provide multiple libraries to interact with the Reef chain. reef.js can be used for both Substrate as well as EVM module interaction. An evm-provider.js wrapper around the reef.js strives to make the EVM module interaction easier - it is compatible with the ethers.js library. The hardhat-reef plugin goes a step further and allows to be used in the Hardhat framework - you can easily compile/deploy/interact with the contracts in a single project."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "developers"
weight: 250
toc: true
---

## reef.js

[reef.js](https://github.com/reef-defi/reef.js) is the low-level wrapper around all the methods exposed by a Reef Chain client and defines all the types exposed by a node.

The API is split up into a number of internal packages:
  - [@reef-defi/api](https://github.com/reef-defi/reef.js/tree/master/packages/api): The API library, providing both Promise and RxJS Observable-based interfaces. This is the main user-facing entry point.
  - [@reef-defi/api-derive](https://github.com/reef-defi/reef.js/tree/master/packages/api-derive): Derived results that are injected into the API, allowing for combinations of various query results (only used internally and exposed on the API instances via `api.derive.*`)
  - [@reef-defi/types](https://github.com/reef-defi/reef.js/tree/master/packages/types), [@reef-defi/type-definitions](https://github.com/reef-defi/reef.js/tree/master/packages/type-definitions): Codecs/type definitions for all Substrate primitives.


reef.js exposes most of the methods that can be found in [Polkadot.js documentation](https://polkadot.js.org/docs/api), e.g. to return a data about an account:

```javascript
import { ApiPromise } from '@polkadot/api';
import { WsProvider } from '@polkadot/rpc-provider';
import { options } from '@reef-defi/api';

async function main() {
    const provider = new WsProvider('wss://rpc-testnet.reefscan.com/ws');
    const api = new ApiPromise(options({ provider }));
    await api.isReady;

    // use api
    const data = await api.query.system.account('5F98oWfz2r5rcRVnP9VCndg33DAAsky3iuoBSpaPUbgN9AJn');
    console.log(data.toHuman())
}

main()
```

### Transactions
Transaction endpoints are exposed, as determined by the metadata, on the `api.tx` endpoint. These allow you to submit transactions for inclusion in blocks, be it transfers, setting information or anything else your chain supports.

A simple transaction to send a transfer from Alice to Bob would look like:
```javascript
// Sign and send a transfer from Alice to Bob
const txHash = await api.tx.balances
  .transfer(BOB, 12345)
  .signAndSend(alice);

// Show the hash
console.log(`Submitted with hash ${txHash}`);
```

We have already become familiar with the `Promise` syntax that is used throughout the API, in this case it is no different. We construct a transaction by calling balances.`transfer(<accountId>, <value>)` with the required params and then as a next step we submit it to the node.

As with all other API operations, the to params just needs to be "account-like" and the value params needs to be "number-like", the API will take care of encoding and conversion into the correct format.

The result for this call (we will deal with subscriptions in a short while), is the transaction hash. This is a hash of the data and receiving this does not mean that transaction has been included, but rather only that it has been accepted for propagation by the node. (It can still fail on execution.)


#### Under the hood

Despite the single-line format of `signAndSend`, there is a lot happening under the hood (and all of this can be manually provided)
- Based on the sender, the API will query `system.account` to determine the next nonce to use
- The API will retrieve the current block hash and use it to create a mortal transaction, i.e. the transaction will only be valid for a limited number of blocks
- It will construct a payload and sign this, this includes the `genesisHash`, the `blockHash` for the start of the mortal era as well as the current chain `specVersion`
- The transaction is submitted to the node

As suggested, you can override all of this, i.e. by retrieving the nonce yourself and passing that as an option, i.e. `signAndSend(alice, { nonce: aliceNonce })`, this could be useful when manually tracking and submitting transactions in bulk.

The variable `alice` seems to have appeared from thin air. To understand how transactions are signed, we will take a brief diversion into the keyring.

### Keyring

The `@polkadot/keyring` keyring is included directly with the API as a dependency, so it is directly importable alongside the API.

Once installed, you can create an instance by just creating an instance of the `Keyring` class.

```javascript
// Import the keyring as required
import { Keyring } from '@polkadot/api';

// Initialize the API as we would normally do
...

// Create a keyring instance
const keyring = new Keyring({ type: 'sr25519' });
```

#### Adding accounts

The recommended catch-all approach to adding accounts is via ``.addFromUri(<suri>, [meta], [type])`` function, where only the `suri` param is required. For instance to add an account via mnemonic, you would do the following:

```javascript
// Some mnemonic phrase
const PHRASE = 'entire material egg meadow latin bargain dutch coral blood melt acoustic thought';

// Add an account, straight mnemonic
const newPair = keyring.addFromUri(PHRASE);

// (Advanced) add an account with a derivation path (hard & soft)
const newDeri = keyring.addFromUri(`${PHRASE}//hard-derived/soft-derived`);

// (Advanced, development-only) add with an implied dev seed and hard derivation
const alice = keyring.addFromUri('//Alice', { name: 'Alice default' });
```


#### Working with pairs
In the previous examples we added a pair to the keyring (and we actually immediately got access to the pair). From this pair there is some information we can retrieve:
```javascript
// Add our Alice dev account
const alice = keyring.addFromUri('//Alice', { name: 'Alice default' });

// Log some info
console.log(`${alice.meta.name}: has address ${alice.address} with publicKey [${alice.publicKey}]`);
```

Additionally you can sign and verify using the pairs. This is the same internally to the API when constructing transactions:

```javascript
// Some helper functions used here
import { stringToU8a, u8aToHex } from '@polkadot/util';

...

// Convert message, sign and then verify
const message = stringToU8a('this is our message');
const signature = alice.sign(message);
const isValid = alice.verify(message, signature);

// Log info
console.log(`The signature ${u8aToHex(signature)}, is ${isValid ? '' : 'in'}valid`);
```

For more options and methods using the `reef.js`, please refer to the [Polkadot.js documentation](https://polkadot.js.org/docs/api). Now we will take a look at the `evm-provider.js` wrapper, which simplifies a lot of things and allows to interact with the underlying EVM engine through `ethers.js` API.


## evm-provider.js

[evm-provider.js](https://github.com/reef-defi/evm-provider.js) is a wrapper around the `reef.js` library described above, primarily used to interact with the EVM module deployed on the Reef chain.

### Instantiation

The instantiation is similar to reef.js:

```javascript
import { options } from "@reef-defi/api";
import { Provider } from "@reef-defi/evm-provider";
import { WsProvider } from "@polkadot/api";

const provider = new Provider(
  options({
    provider: new WsProvider("ws://localhost:9944")
  })
);
```

`Provider` object can now be used for both Substrate as well as EVM module interaction. For Substrate interaction use `provider.*` methods such as `provider.api.*`, `provider.rpc.*` - `reef.js` methods are exposed through this object.

For the EVM interaction the `evm-provider.js` provides multiple objects that simplify contract interaction on the Reef chain. A full instantiation example would look like:
```javascript

import {
  TestAccountSigningKey,
  Provider,
  Signer,
} from "@reef-defi/evm-provider";
import { WsProvider, Keyring } from "@polkadot/api";
import { createTestPairs } from "@polkadot/keyring/testingPairs";
import { KeyringPair } from "@polkadot/keyring/types";

const WS_URL = process.env.WS_URL || "ws://127.0.0.1:9944";
const seed = process.env.SEED;

const setup = async () => {
  const provider = new Provider({
    provider: new WsProvider(WS_URL),
  });

  await provider.api.isReady;

  let pair: KeyringPair;
  if (seed) {
    const keyring = new Keyring({ type: "sr25519" });
    pair = keyring.addFromUri(seed);
  } else {
    const testPairs = createTestPairs();
    pair = testPairs.alice;
  }

  const signingKey = new TestAccountSigningKey(provider.api.registry);
  signingKey.addKeyringPair(pair);

  const wallet = new Signer(provider, pair.address, signingKey);

  // Claim default account
  if (!(await wallet.isClaimed())) {
    console.log(
      "No claimed EVM account found -> claimed default EVM account: ",
      await wallet.getAddress()
    );
    await wallet.claimDefaultAccount();
  }

  return {
    wallet,
    provider,
  };
};

export default setup;
```

This is taken from the [reefswap repo](https://github.com/reef-defi/reefswap/blob/master/src/setup.ts). We initialize the `Provider` object first, create a keyring pair using the `Keyring` object from Polkadot and wrap it around `TestAccountSigningKey` object used by `evm-provider`. Note that this object can be either a test account (such as `alice`) or an arbitrary account specified by the `seed` variable (mnemonic).


### Signer (EVM wallet) object

In the example above the pair is wrapped into the `Signer` object, which is compatible with the `ethers.js` [Signer object](https://docs.ethers.io/v5/api/signer/). A `Signer` in `ethers` is an abstraction of an Ethereum Account, which can be used to sign messages and transactions and send signed transactions to execute state changing operations. Most of the `evm-provider.js` API is compatible with `ethers.js`. If you are not familiar with `ethers.js`, you can start by looking at its [documentation](https://docs.ethers.io/v5/single-page/).

The wallet (`Signer` object) is then checked whether the EVM address was already claimed and if it was not, it claims the default account calculated from the Substrate address (EVM address binding). This has to be performed only once since the Substrate address does not have the EVM address assigned to it by default.

### Deploy and interact with the contract

With the `wallet` and `provider` objects we can now interact with the chain using `ethers.js` syntax. If we take a look at the [deploy script](https://github.com/reef-defi/reefswap/blob/master/src/deploy.ts) for the Reefswap:

```javascript
import { Contract, ContractFactory, BigNumber } from "ethers";
...
import Token from "../artifacts/contracts/Token.sol/Token.json";

// Setup script from above
import setup from "./setup";

// A big number
const dollar = BigNumber.from("10000000000000");

const main = async () => {
  // The instantiation
  const { wallet, provider } = await setup();
  const deployerAddress = await wallet.getAddress();

  // Using ethers ContractFactory and evm-provider.js wallet
  const tokenReef = await ContractFactory.fromSolidity(Token)
    .connect(wallet)
    .deploy(dollar.mul(1000));

  ...
  // Calling `approve` function on the ERC contract
  await tokenReef.approve(router.address, dollar.mul(100));

  ...
  // Disconnect
  provider.api.disconnect();
}
```

To instantiate a contract object we use a [`ContractFactory`](https://docs.ethers.io/v5/api/contract/contract-factory/#ContractFactory) object. It requires a contract ABI `Token.json`  and is the output of a contract compilation. You can create it by any means of Solidity compilation (e.g. directly through `solc`, through the Remix website IDE...)

{{< alert icon="💡" text="Use the Hardhat framework below to abstract and simplify many of the intricacies involving using the evm-provider directly. The compilation for example will be done automatically by the Hardhat framework." >}}

We connect `wallet` from the `setup` step above and supply the constructor arguments in the `.deploy(<arguments>)` method. The result of the call is a contract object on which we can call all the methods defined in the ABI. For example, we called the `approve(address,uint256)` method on the newly deployed `tokenReef` ERC-20 contract.

### Calling existing contract

The above example considered a newly deployed contract. If we want to interact with the existing contract on the chain then we would use `Contract` object:

```javascript
import Token from "../artifacts/contracts/Token.sol/Token.json";
import setup from "./setup";

const main = async () => {
  const { wallet, provider } = await setup();
  const tokenReef = new Contract("0x0000000000000000000000000000000001000000", Token.abi, wallet);

  await tokenReef.approve(router.address, dollar.mul(100));
}
```

The first argument is the contract's address, the second the contract's ABI and finally the `evm-provider.js Signer` object. From there on you can use the same way of calling contract's methods as in the above (deploy) case.

### Pre-deployed contracts addresses

Some contracts are already pre-deployed on the chain, most notably:
- REEF token: `0x0000000000000000000000000000000001000000`
- RUSD token: `0x0000000000000000000000000000000001000001`

REEF token is the native currency of the Reef chain, meaning a `.transfer()` on the EVM contract will transfer funds the same way the Substrate `.transfer()` call would.

If you want to simplify the setup, you may opt for the Hardhat Reef plugin, which is explained in the next section.

## Hardhat

Javascript developers can use [Reef Hardhat plugin](https://github.com/reef-defi/hardhat-reef) to develop, deploy and test smart contracts on the Reef chain. A few working examples can be found in [hardhat-reef-examples repo](https://github.com/reef-defi/hardhat-reef-examples). A hardhat reef template can be found [here](https://github.com/reef-defi/hardhat-reef-template).

## Examples
Here are a few example applications that can be used as a Reef chain integration reference:
- [Reefswap](https://github.com/reef-defi/reefswap) for integration with Reef Hardhat (`scripts` directory) and integration with the `evm-provider.js` (`src` directory).
- [Hardhat Reef Examples](https://github.com/reef-defi/hardhat-reef-examples) for examples of integration with the Reef Hardhat plugin.
- [UI examples](https://github.com/reef-defi/ui-examples) for integration with Polkadot.js browser extension and EVM contract interaction.
- [EVM Playground](https://github.com/reef-defi/evm-canvas-ui) a more sophisticated example of integration with Polkadot.js browser extension and EVM contract interaction.
- [Reefscan](https://github.com/reef-defi/reef-explorer) and [Remix Reef Plugin](https://github.com/reef-defi/reef-remix-plugin) for Reef chain integration.
