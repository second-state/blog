---
title: "What I learned from Ethereum Devcon5"
date: 2019-10-24T01:01:23+08:00
draft: false
tags: ["Ethereum", "EWASM", "eth2","SOLL","LLVM","Devcon5,"Plasma"]
categories: ["en"]
author: "Michael Yuan"
---

The Ethereum Foundation Devcon is the most important annual gathering of developers, designers, and researchers in the Ethereum ecosystem. This year’s Devcon5 in Osaka, Japan is the 5th Devcon, and as in past years, it is a bellwether of what to come in Ethereum.

*Note: My company, [Second State](https://www.secondstate.io/), was invited to [give a talk at Devcon5](https://blog.secondstate.io/post/20191014-secondstate-at-devcon5/), where we showed off [the world's first toolchain](https://blog.secondstate.io/post/20191022-soll-compiler-project/) to compile and deploy Solidity smart contracts on the official Ewasm testnet. [Learn more](https://www.secondstate.io/devcon5/) about Second State’s smart contract innovation and developer tools @Devcon5.*

## No news is big news

In Devcon4 in Prague last year, there was great optimism about ETH 2 (Serenity), as well as Ethereum scaling solutions generally known as Plasma. The overall sentiment in Prague was that we have finally figured out how to build scalable decentralized smart contract platforms. However, fast forward to Osaka in 2019, much of optimism is gone. ETH 2 is still under intensive research. Many experiments in the past year had not produced the results we hoped for.

In Osaka, most people seem to agree that the entire ETH 2 effort requires a reboot. There was very little information about the launch of ETH 2. The lack of concrete ETH 2 news and concrete plans is perhaps the most significant news from Devcon5. CoinDesk published a [controversial article](https://www.coindesk.com/scam-or-iteration-at-devcon-ethereum-diehards-still-believe-in-2-0) on this subject on the very first day of the conference. 

## ETH 1.x is baaaack

With ETH 2 still perhaps years away, there is now renewed focus on making the Ethereum blockchain we have today more usable for application developers. For example, it will attempt to reduce the bloat of state data on nodes by making changes to gas fees, instituting a “state rent”, and optimizing node client storage.

There are also discussions about launching the next generation WebAssembly-based Ethereum Virtual Machine (known as Ethereum flavored WebAssembly, or Ewasm) on the ETH 1.x blockchain. That could significantly improve application developer experience on Ethereum. However, the Ewasm toolchain and execution engine are still work-in-progress. 

![](/images/20191024-devcon5-racap-01.png)

One of the contributions my team at Second State made was to create the [world’s first toolchain](https://blog.secondstate.io/post/20191022-soll-compiler-project/) to compile and deploy Solidity smart contracts on the official Ewasm testnet. Our toolchain is based on the LLVM infrastructure, making it future proof. You can [read more](https://www.secondstate.io/devcon5/) about our effort to improve the EVM experience.

## Reconciliation with ETC

The Ethereum Classic (ETC) blockchain is the original ETH 1.x blockchain. It remains committed to PoW consensus while trying to maintain compatibility with ETH 2 at the application layer. 

With ETH 2’s move to PoS and multi-chain architecture (sharding), it is possible that ETC will inherit the ETH mining community and become a PoW store-of-value token in the ETH 2 ecosystem.

![](/images/20191024-devcon5-racap-02.png)

At Devcon5, ETCLabs got on the main stage to discuss collaboration and governance. The message of reconciliation echoes the sentiment at [ETC Summit](https://blog.secondstate.io/post/20191006-etc-summit-recap/) in Vancouver a few days prior to Devcon5. It is great to see the ETH and ETC communities making amends. We look forward to seeing more innovations and contributions from ETC!

## Layer 2 scaling
With the successful launch of the Bitcoin lightning network, developers were very optimistic about layer 2 scaling solutions for ETH. However, in the past year, Plasma, ETH’s leading candidate for Layer 2 solutions had encountered many problems and setbacks. At Devcon5, we still do not have a working Plasma solution.

However, a new approach emerged. The Uniswap team demonstrated a layer 2 solution that is specifically designed and optimized for the Uniswap Exchange application. It is called the [Unipig Exchange](https://unipig.exchange/). You can go try it out. It is very fast with instant trade confirmations. On Ethereum testnet, it aims to reach 2000 TPS. Would this “application specific layer 2” approach work for most popular applications on Ethereum? We will probably find out in Devcon6 next year.

## The composability problem

As the Ethereum evolves to become the public blockchain for decentralized finance (DeFi) applications, a serious concern about the ETH 2 technical architecture was raised. Many DeFi applications rely on the interactions between smart contracts to function. For example, a loan contract might need to interact with token contracts to manage the tokens it lends. Would ETH 2 sharding, with different smart contracts potentially living on different blockchains, continue to guarantee deterministic interactions between smart contracts? Can we still build applications that compose multiple smart contracts? This is known as the composability for ETH 2, and it received a lot of discussion at Devcon5. 

During the conference, Vitalik Buterin [published an article](https://ethresear.ch/t/cross-shard-defi-composability/6268) addressing this issue. His answer? By and large, application developers can still compose smart contracts except for certain conditions. However, some developers remain unconvinced, as those conditions might just be some DeFi apps require. Interested readers should checkout Vitalik’s article and its comments thread.

## Continued focus on developer experience

The diverse and large developer community is always key to the success of Ethereum. It is also why Devcon is called Devcon to begin with.

In Devcon5, there is a continued focus on developer communities. A lot of sessions and speaking time were allocated for developer related content. Joseph Rubin of Consensys went on the main stage and raised the goal of one million Ethereum developers.

![](/images/20191024-devcon5-racap-03.png)

At Second State, we provide [tools for even novice developers](https://docs.secondstate.io/buidl-developer-tool/why-buidl) to quickly get started with Ethereum application development. [Checkout the BUIDL IDE](https://docs.secondstate.io/buidl-developer-tool/getting-started) and you could publish your first web-based dapp on a public blockchain in minutes!

Libra, the elephant in the room
One of the big news in 2019 was Facebook’s entrance into cryptocurrency space via the Libra stablecoin project. The members from the Cosmos Foundation and Ethereum Foundation banded together to announce the OpenLibra project, which aims to create a Libra-like stablecoin, but with open source software and open governance. Many in the Ethereum community, however, do not like this idea.

![](/images/20191024-devcon5-racap-04.png)

While Facebook Libra has recently suffered push backs from regulators and partners alike, I believe Libra as a cryptocurrency experiment and software innovation will remain relevant in the years to come. Interested readers could check out [our articles](https://medium.com/hackernoon/libra-first-impressions-ed6b5f15ae63) on Libra’s MOVE language and VM. 

## The road ahead
Overall, I enjoyed my experience at Devcon5. Osaka is a great place to hold such a conference — very safe and clean, great food, easy to get around. While ETH 2 and Plasma development suffered setbacks, the community continues to engage developers and find ways to move forward.

With increased funding for Ethereum 1.x (both from Ethereum Foundation and ETC Labs), I believe that we will continue to see innovations and improvements on the Ethereum application layer, including the new Ewasm virtual machine, tooling, and application-specific layer 2 solutions. We remain optimistic about the future of Ethereum. 

