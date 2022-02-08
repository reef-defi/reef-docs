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

It is available for both mainnet and testnet, on `/graphql`:
```
Mainnet: https://reefscan.com/graphql
Testnet: https://testnet.reefscan.com/graphql
```

![](/docs/developers/the_graph.png)

#### Examples

Here is an example query for getting chain stats:
```
query ChainInfo {
  chain_info {
    name
    count
  }
}
```

We can subscribe to smart contract events, following a similar interface to Ethereum's `eth_subscribe('logs')`:
```
import { gql } from '@apollo/client';

export const CONTRACT_EVENTS_GQL = gql`
    subscription evmEvent($address: String!, $perPage: Int!, $offset: Int!, $topic0: String){
        evm_event(
        limit: $perPage,
        offset: $offset
        order_by: {block_id: desc, extrinsic_index: desc, event_index: desc},
        where: {
            contract_address: {_eq: $address},
            topic_0: {_eq:$topic0},
            method: {_eq: "Log"}
            }
        ) {
        contract_address
        data_parsed
        data_raw
        topic_0
        topic_1
        topic_2
        topic_3
      }
    }
`;
```

We can also find tokens and their balances for a specific account:
```
subscription query ($accountId: String!) {
    token_holder(
      order_by: { balance: desc }
      where: { signer: { _eq: $accountId } }
    ) {
      token_address
      balance
    }
  }
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
