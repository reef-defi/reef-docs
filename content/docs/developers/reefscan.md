---
title: "Reefscan"
description: "Reefscan is more than just a block explorer. It contains useful resources that developers can leverage for their dapps, such as the GraphQL server and smart contract APIs."
lead: "Reefscan is more than just a block explorer. It contains useful resources that developers can leverage for their dapps, such as the GraphQL server and smart contract APIs."
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


Reefscan offers the following solutions for various use cases:
 - GraphQL server for web apps
 - Smart contract source verification
 - HTTP API for developers
 - easy custom indexing and access of chain data with subsquid.io

### GraphQL
Reef uses a public GraphQL service from Subsquid that indexes all onchain events at blazing speeds.

It is available for both mainnet and testnet, under following URIs:
```
Mainnet: https://squid.subsquid.io/reef-explorer/graphql
Testnet: https://squid.subsquid.io/reef-explorer-testnet/graphql
```
The URIs provides a GraphQL Playground interface for testing queries aswell.

![](/docs/developers/subsquid_playground.png)

#### Examples
The GraphQL endpoints support `Query` operations. For custom deployment `Subscription` and `Mutation` can be used. Here we will discuss examples of each of these. You can see the schema for each of these by going into corresponding docs on the top right side of Graphql playground.

<img src="/docs/developers/subsquid_gql_schema.png" alt="Image Alt Text" width="250"/>


##### Query
A Query is a specific request allowing user to specify structure & shape of the response they want to receive.
here is a query for getting chain stats:
```
query {
  blocks(limit: 10, orderBy: height_DESC) {
    id
    hash
    timestamp
  }
}
```
In this example we are querying last 10 blocks, the `limit` clause is used to limit the response array size. 

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

A `Subscription` is possible on custom deployment and is like a live stream that keeps sending you updates whenever change in query happens, so you always have the latest information.

We provide util-lib library to easily subscribe to evm events. As an example you can also subscribe to GraphQL evmEvents in web app like this:

```
const EVM_EVENT_QUERY = gql`
    subscription evmEvent(
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

Our util-lib offers observables to easily stream current account token balances.
For custom GraphQL case we can use Axios or Apollo GraphQL lib as well.

To get NFTs owned by the account, you can do it like this:
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


### Custom indexer instance
Power users who need their own custom indexer or data source can clone and customize https://github.com/reef-chain/subsquid-evm-event-processor repository.
You can then spin up multiple versions or instances of indexer and GraphQL gateway servers.

## Smart contracts

### Querying API
The querying API allows one to obtain the contract sources and ABI (if verified).
```
curl -s 'https://api-testnet.reefscan.info/contract/0x0000000000000000000000000000000001000000' | jq .
```

Response:
```
{"address":"0x0000000000000000000000000000000001000000",
  "bytecode":"0x608060405234801561001057600080fd5...}
```

### Verification API
The verification API allows developers to automatically upload and verify the source code of their smart contracts.
```
curl 'https://api-testnet.reefscan.com/verificator/submit-verification' \
  --data-raw $'{"address":"0xc12532e256D63F9A2C3b7Cc750ed5C136035AEe9","arguments":"[]","name":"Storage","filename":"contracts/1_Storage.sol","target":"london","source":"{\\"contracts/1_Storage.sol\\":\\"// SPDX-License-Identifier: GPL-3.0\\\\n\\\\npragma solidity >=0.7.0 <0.9.0;\\\\n\\\\n/**\\\\n * @title Storage\\\\n * @dev Store & retrieve value in a variable\\\\n */\\\\ncontract Storage {\\\\n\\\\n    uint256 number;\\\\n\\\\n    /**\\\\n     * @dev Store value in variable\\\\n     * @param num value to store\\\\n     */\\\\n    function store(uint256 num) public {\\\\n        number = num;\\\\n    }\\\\n\\\\n    /**\\\\n     * @dev Return value\\\\n     * @return value of \'number\'\\\\n     */\\\\n    function retrieve() public view returns (uint256){\\\\n        return number;\\\\n    }\\\\n}\\"}","optimization":"false","compilerVersion":"v0.8.4+commit.c7e474f2","license":"GPL-3.0","runs":200}' \
  --compressed
```

[Remix](https://remix.reefscan.com) and [HardHat](https://github.com/reef-defi/hardhat-reef) already support the verification API, and should be the default methods for
deploying smart contracts onto Reef chain.
