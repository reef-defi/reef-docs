---
title: "Reefscan"
description: "Reefscan is more than just a block explorer. It contains useful resources that developers can leverage for their dapps, such as the PostgreSQL database layer, GraphQL server and smart contract APIs."
lead: "Reefscan is more than just a block explorer. It contains useful resources that developers can leverage for their dapps, such as the PostgreSQL database layer, GraphQL server and smart contract APIs."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "developers"
weight: 270
toc: true
---

## Reef chain data
Most apps require a fast and efficient data source for things that are not easily available over raw
blockchain RPC, for example:
 - balances of all tokens that a user has
 - list of token holders for a particular coin
 - history of token trades on a DEX
 - history of user's interaction with a smart contract


Reefscan offers 3 solutions for various use cases:
 - GraphQL server for web apps
 - PostgreSQL database for advanced apps and data analysis
 - HTTP API for developers

### The Graph
Reef uses a public GraphQL service from Subsquid that indexes all onchain events at blazing speeds.

It is available for both mainnet and testnet, under following URIs:
```
Mainnet: https://squid.subsquid.io/reef-explorer/graphql
Testnet: https://squid.subsquid.io/reef-explorer-testnet/graphql
```
The URIs provides a GraphQL Playground interface for testing queries aswell.

![](/docs/developers/the_graph.png)

#### Examples
The graphql endpoints support 3 types of operations: `Query` , `Subscription` and `Mutation`. Here we will discuss examples of each of these. You can see the schema for each of these by going into corresponding docs on the top right side of Graphql playground.

##### Query
A Query is a specific request allowing user to specify structure & shape of the response they want to receive.
here is a query for getting chain stats:
```
query ChainInfo {
  chainInfos(limit: 10) {
    count
    id
  }
}
```
In this example we are querying the chain info , the parameters which we are requesting are `count` and `id`, the `limit` clause is used to limit the response array size. 

![](/docs/developers/response_chainInfos.png)


In the next example we will see how to fetch transfers count using xConnections in Graphql

```
query Last30Days {
  transfersConnection(orderBy: timestamp_DESC, where: {timestamp_gte: "2023-08-14T04:23:50.000000Z"}) {
    totalCount
  }
}
```
You can replace the `2023-08-14` with the date you want, here the format is `YYYY-MM-DD` . This query is fetching the transfers count which have timestamp greater than `2023-08-14T04:23:50.000000Z`

##### Subscription

A `Subscription` is like a live stream that keeps sending you updates whenever something interesting happens, so you always have the latest information.

We can subscribe to the evmEvents like this

```
import { gql } from '@apollo/client';

export const EVM_EVENTS_GQL = gql`
 subscription MySubscription {
  evmEvents(limit: 10) {
    block {
      height
    }
    contractAddress
    id
    type
  }
}`;
```
Here we are requesting the server to send us data whenever any event occurs, we are getting a stream of data. In this example we are just fetching block `height`, contract `address`, `id` of event and the `type` of event. We can modify the structure and shape as we want.Just like this :

```
const EVM_EVENT_QUERY = gql`
    query evmEvent(
      $address: String_comparison_exp!
      $blockId: bigint_comparison_exp!
      $topic0: String_comparison_exp
    ) {
      evm_event(
        order_by: [
          { block_id: desc }
          { extrinsic_index: desc }
          { event_index: desc }
        ]
        where: {
          _and: [
            { contract_address: $address }
            { topic_0: $topic0 }
            { method: { _eq: "Log" } }
            { block_id: $blockId }
          ]
        }
      ) {
        contract_address
        data_parsed
        data_raw
        topic_0
        topic_1
        topic_2
        topic_3
        block_id
        extrinsic_index
        event_index
      }
    }
  `;
```

To make it more intuitive you can always check the response of any query here in the graphql playground.

![](/docs/developers/response_evmEvents.png)

We can also find tokens and their balances for a specific account:
```
subscription query ($accountId: String!) {
  tokenHolders(orderBy: balance_DESC, limit: 10,where: {id_eq: $accountId}) {
    balance
    id
  }
  }
```

##### Use this data in your apps

You can use this data inside your websites/apps as well. Here is a basic example for it. In this example we are using axios to make requests you can use apollo as well.

Suppose you want to create an app which displays NFTs owned by the user, then here is a basic example for that.

You need to write a query to fetch the NFTs, you can do it like this:
```
const SIGNER_NFTS_QUERY = `
  query signer_nfts($accountId: String) {
    tokenHolders(
      orderBy: balance_DESC
      limit: 199
      where: {
        AND: {
          nftId_isNull: false
          token: { id_isNull: false }
          signer: { id_eq: $accountId }
          balance_gt: "0"
        }
        type_eq: Account
      }
    ) {
      token {
        id
        type
      }
      balance
      nftId
    }
  }
`;
```

We are creating a function which returns an object consisting of `query` and `variables`:

```
const getSignerNftsQuery = (accountId: string) => {
  return {
    query: SIGNER_NFTS_QUERY,
    variables: { accountId },
  };
};
```

Specifying the Graphql Endpoint :
```
const gqlEndpoint = "https://squid.subsquid.io/reef-explorer/graphql";
```

Creating a request object for `axios` :

```
const requestObject = {
      method: "post",
      url: gqlEndpoint,
      headers: {
        "Content-Type": "application/json",
      },
      data: getSignerNftsQuery(accountId),
    };
```

Logging the response in console, you can use this to display in the frontend
```
const resp = await axios(gqlReq);
console.log(resp.data.data.tokenHolders);
```


A few more examples can also be found in:
 - Reefscan block explorer is [open-source](https://github.com/reef-defi/reef-explorer), and all UI components use GraphQL as the backend.
 - [UI-examples](https://github.com/reef-defi/ui-examples) has a few GraphQL [queries](https://github.com/reef-defi/ui-examples/blob/master/packages/example-react/src/gql.ts) as well
 - [UI Kit](/docs/developers/ui_kit/) has pre-made components for common tasks


### Postgres database
Power users who wish to have Reef chain data available in Postgres can run their own instance of
Reefscan.

Reefscan runs in Docker. Setting up the full Reefscan stack (PostgreSQL, GraphQL, APIs) is as easy as:
```
yarn
make net=mainnet env=prod up
```
Check out the full [documentation](https://github.com/reef-defi/reef-explorer#readme) on setting up and configuring your own, production grade instance of Reefscan.

You may then connect to your local Postgres database and run queries against it.

## Smart contracts

### Querying API
The querying API allows one to obtain the contract sources and ABI (if verified).
```
curl -s 'https://testnet.reefscan.com/api/contract/0xc12532e256D63F9A2C3b7Cc750ed5C136035AEe9' | jq .
```

Response:
```
{
  "address": "0xc12532e256d63f9a2c3b7cc750ed5c136035aee9",
  "bytecode": "0x608060405234801561001057600080fd5b5061012f806100206000396000f3fe6080604052348015600f57600080fd5b506004361060325760003560e01c80632e64cec11460375780636057361d146051575b600080fd5b603d6069565b6040516048919060c2565b60405180910390f35b6067600480360381019060639190608f565b6072565b005b60008054905090565b8060008190555050565b60008135905060898160e5565b92915050565b60006020828403121560a057600080fd5b600060ac84828501607c565b91505092915050565b60bc8160db565b82525050565b600060208201905060d5600083018460b5565b92915050565b6000819050919050565b60ec8160db565b811460f657600080fd5b5056fea26469706673582212206967f80107a3b8ab68d5b6075e84b822cbd472d325fe716af1eaa2a4fa2548f364736f6c63430008040033"
}
```

### Verification API
The verification API allows developers to automatically upload and verify the source code of their smart contracts.
```
curl 'https://testnet.reefscan.com/api/verificator/submit-verification' \
  --data-raw $'{"address":"0xc12532e256D63F9A2C3b7Cc750ed5C136035AEe9","arguments":"[]","name":"Storage","filename":"contracts/1_Storage.sol","target":"london","source":"{\\"contracts/1_Storage.sol\\":\\"// SPDX-License-Identifier: GPL-3.0\\\\n\\\\npragma solidity >=0.7.0 <0.9.0;\\\\n\\\\n/**\\\\n * @title Storage\\\\n * @dev Store & retrieve value in a variable\\\\n */\\\\ncontract Storage {\\\\n\\\\n    uint256 number;\\\\n\\\\n    /**\\\\n     * @dev Store value in variable\\\\n     * @param num value to store\\\\n     */\\\\n    function store(uint256 num) public {\\\\n        number = num;\\\\n    }\\\\n\\\\n    /**\\\\n     * @dev Return value\\\\n     * @return value of \'number\'\\\\n     */\\\\n    function retrieve() public view returns (uint256){\\\\n        return number;\\\\n    }\\\\n}\\"}","optimization":"false","compilerVersion":"v0.8.4+commit.c7e474f2","license":"GPL-3.0","runs":200}' \
  --compressed
```

[Remix](https://remix.reefscan.com) and [HardHat](https://github.com/reef-defi/hardhat-reef) already support the verification API, and should be the default methods for
deploying smart contracts onto Reef chain.
