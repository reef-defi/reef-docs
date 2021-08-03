---
title: "Upgradability"
description: "Reef chain upgradability and governance primer."
lead: "Reef chain upgradability and governance primer."
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "governance"
weight: 300
toc: true
---

## History
Legacy chains such as Bitcoin and Ethereum have no formal governance system. Those chains are operated by PoW miners, who's incentive is to extract the most value through rewards and fees while successfuly navigating the highly competitive mining landscape. This has historically made PoW chains stagnate (no new features), have resistance to scalability solutions (which would reduce miner's fee revenue) and generally suffer from the anti-scaling property of PoW (exponential energy tx costs).

## Reef Governance
Reef chain is being built with upgradability and long-term sustainable governance in mind. The governance responsibility is split into two groups:
 - **Technical Council** is responsible for reviewing and approving blockchain updates. Elected via *Proof of Commitment*.
 - **Validators** are responsible for reliable block production. Elected via *Nominated Proof of Stake*.

### Fees
The fees for deploying smart contracts as well as the transaction fees are **burned**. This is not only an important tokenomics feature, but had the fees been rewarded to the block producers, they would have a perverse incentive to undermine scalability efforts in order to capture the rewards from fee bidding wars.

## Blockchain Upgrades
The **Technical Council** is elected via Proof of Commitment. PoC is inspired by DPoS, however the main feature of PoC is the extremely long un-bonding period (1 year minimum). This ensures that the PoC nominators have skin in the game and are incentivized towards electing competent technical council members with long term sustainability and growth in mind.

### Parameter updates
Technical Council is responsible for regulating key blockchain parameters, such as those pertaining to block space and throughput (block time, feePerByte, max smart contract storage, etc).

### On-Chain upgrades
Reef chain is fully utilizing Substrate's on-chain upgrade capability. Since the runtime is compiled for both native and wasm targets,
the Reef blockchain can be upgraded on-chain trough a `system.setCode()` call.
The on-chain upgrade can only be scheduled upon majority approval of the Techinical Council.

## Block Production
**Validators** are elected by **Nominators** via NPoS. A the end of every block production *era*, [Phragmén](https://wiki.polkadot.network/docs/en/learn-phragmen#weighted-phragm%C3%A9n) elections are held.

Validators and Nominators are paid out from the yearly inflation (current mean target rate is 6.25%). Nominators receive part of revenue share of the elected Validators they voted for, however both Validators and Nominators are slashed (penalized) if Validators misbehave (are offline or equivocating).

Learn more about about how to stake your Reef and participate as Nominator or Validator next.

