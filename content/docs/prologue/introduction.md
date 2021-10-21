---
title: "Introduction"
description: "Reef Chain is an EVM compatible chain for DeFi. You can use this guide to learn how to write and deploy Solidity smart contracts."
lead: "Reef Chain is an EVM compatible chain for DeFi with NPoS/PoC consensus."
date: 2020-10-06T08:48:57+00:00
lastmod: 2020-10-06T08:48:57+00:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 100
toc: true
---


## Get started

(TODO user guide intro)

### Build on Reef

Get started with Reef by installing the [Reef node](/docs/developers/nodes/), and deploying a simple DeFi app locally in [Quick Start]({{< ref "quick-start" >}}).

For in depth tutorials on the underlying aspects of the Reef chain check out the [Developer Guide]({{< ref "quick-start" >}}) and the [Developer Resources]({{< ref "resources" >}}).

### Governance

[Validators](/docs/governance/validators/) and [Nominators](/docs/governance/nominators/) are essential to the Reef ecosystem.

Learn how to [stake](/docs/governance/staking/) your Reef tokens and vote for validators. Highly technical users may also participate in validator elections to have a chance of becoming a paid block producer.


## Why Reef?
Reef is fast, scalable, has low transaction costs and does no wasteful mining.

Reef chain features next-gen blockchain technology, utilizing Nominated Proof of Stake, extensible EVM, on-chain upgradability, libp2p networking and state of the art cryptography.

#### Fees
Reef is efficient, and has a predictable algorithmic fee model. All fees from Reef chain usage are
automatically burned.
```
| Cost of:                    | Ethereum   | Reef       |
| --------------------------- | ---------- | ---------- |
| Making a payment            | $11.5      | $0.05      |
| Executing an Uniswap trade  | $47.6      | $0.12      |
| Deploying an ERC-20 Token   | $500+      | $1.95      |
| Minting a NFT               | ...        | ...        |
```
*\*Last updated on Oct 20th 2021*


#### Reef vs legacy chains
|                             | Bitcoin             | Ethereum            | Reef                           |
| --------------------------- | ------------------- | ------------------- | ------------------------------ |
| Consensus algorithm         | Proof of Waste      | Proof of Waste      | Nominated Proof of Stake       |
| Energy consumption          | More than [Poland](https://digiconomist.net/bitcoin-energy-consumption)    | More than [Austria](https://digiconomist.net/ethereum-energy-consumption)   | Less than a shopping mall |
| On-Chain upgradability      | N/A                 | N/A                 | Yes                            |
| Transaction finality        | 3600+ seconds       | 600+ seconds        | 40 seconds                     |
| Block time                  | ~600 seconds        | ~17 seconds         | 10 seconds                     |
| Max block size              | < 2MB               | N/A                 | 3.75MB                         |
| Throughput                  | 7 tps               | 14 tps              | TBD                            |
| Smart contracts             | No                  | Yes, EVM            | Yes, EVM                       |
