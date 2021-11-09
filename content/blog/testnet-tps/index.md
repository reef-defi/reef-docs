---
title: "Reef testnet stress test"
description: "Reef chain stress test yields ~ 550 TPS (transactions per second)."
lead: ""
date: 2021-11-09T00:00:00+02:00
lastmod: 2021-11-09T00:00:00+02:00
draft: false
weight: 50
images: []
contributors: ["Reef Developers"]
---

We have ran multiple stress tests on Reef testnet to benchmark Reef chain's performance.

### Methodology
Reef testnet is currently running the same runtime and configuration as Reef mainnet, therefore
these results are also indicative of mainnet performance. The transactions were broadcasted through
a regular Reef node, without any optimizations.

We have prepared and broadcasted two types of transactions to test for both on-chain and EVM throughput.

For the EVM throughput, ERC-20 token transfers were chosen, as they are the most common transaction
type on Ethereum.

The source code to replicate the test is available on [Github](https://github.com/reef-defi/reef-chain-test).

### Results
The **EVM token transfer test yielded 550 TPS** (transactions per second), while a slighly more complex
on-chain transfers yielded 465 TPS.

The current throughput seems to be around 20x higher than Ethereum, thanks to the more efficient
NPoS consensus and modern blockchain architecture.

### Future improvements
Reef chain can scale its throughput further by increasing block weight limit as well as by increasing
the compute time allowance. Currently, the allowance is 2s out of 10s block time to ensure everyone
can run a full node from anywhere in the world, using only modest hardware ($20/mo VPS should be
sufficient).

