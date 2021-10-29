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

Here is an example query for getting chain stats:
![](/docs/developers/the_graph.png)

(todo js example)
```
query Query_root {
  total {
    count
    name
  }
}
  ```

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

