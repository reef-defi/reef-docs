---
title: "Resources"
description: "Reef chain tools and resources"
lead: "Reef chain tools and resources"
date: 2020-10-13T15:21:01+02:00
lastmod: 2020-10-13T15:21:01+02:00
draft: false
images: []
menu:
  docs:
    parent: "prologue"
weight: 130
toc: true
---

{{< alert icon="💡" text="You can change the commands in the scripts section of `./package.json`." >}}

## Running a node

Start local development server:

{{< btn-copy text="npm run start" >}}

```bash
npm run start
```

## Developing your first DeFi app

Check scripts, styles, and markdown for errors:

{{< btn-copy text="npm run lint" >}}

```bash
npm run lint
```

## Deploying on testnet

Check scripts for errors:

{{< btn-copy text="npm run lint:scripts" >}}

```bash
npm run lint:scripts [-- --fix]
```

## Deploying on mainnet

Check styles for errors:

{{< btn-copy text="npm run lint:styles" >}}

```bash
npm run lint:styles [-- --fix]
```

