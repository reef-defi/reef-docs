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
Reefscan comes with a public GraphQL service, also known as "The Graph".

It is available for both mainnet and testnet, on `/api/v3`:
```
Mainnet: https://reefscan.com/api/v3
Testnet: https://testnet.reefscan.com/api/v3
```

![](/docs/developers/the_graph.png)

#### Examples

Here is an example query for getting chain stats:
```
query Query_root {
  total {
    count
    name
  }
}
```

We can subscribe to smart contract events, following a similar interface to `eth_subscribe('logs')`:
```
import { gql } from '@apollo/client';

export const CONTRACT_EVENTS_GQL = gql`
          subscription events(
            $blockNumber: bigint
            $perPage: Int!
            $offset: Int!
            $contractAddressFilter: String!
          ) {
            event(
              limit: $perPage
              offset: $offset
              where: {block_number: { _eq: $blockNumber }, section: { _eq: "evm" }, method: { _eq: "Log" }, data: {_like: $contractAddressFilter}}
              order_by: { block_number: desc, event_index: desc }
            ) {
              block_number
              event_index
              data
              method
              phase
              section
              timestamp
            }
          }
        `;
```

We can also find tokens and their balances for a specific account:
```
export const TOKEN_BALANCES_GQL = gql`
          subscription tokenHolders(
            $contractId: String!
            $accountAddress: String!
            $perPage: Int!
            $offset: Int!
          ) {
            token_holder(
              limit: $perPage
              offset: $offset
              where: {contract_id: { _eq: $contractId }, holder_evm_address: { _like: $accountAddress } }
              order_by: { block_height: desc }
            ) {
              contract_id
              holder_account_id
              holder_evm_address
              balance
              block_height
              timestamp
            }
          }
        `;
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
yarn workspace backend docker:mainnet
```
Check out the full [documentation](https://github.com/reef-defi/reef-explorer#readme) on setting up and configuring your own, production grade instance of Reefscan.

You may then connect to your local Postgres database and run queries against it.

## Smart contracts

### Querying API

### Verification API

