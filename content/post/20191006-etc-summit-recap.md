---
title: "What I learned from ETC Summit"
date: 2019-10-06T10:01:23+08:00
draft: false
tags: ["Ethereum Classic","ETC Summit","Developer","BUIDL","ETC"]
categories: ["en"]
author: "Michael Yuan"
---

*Disclaimer: [Second State](https://www.secondstate.io/) sponsored this year’s [ETC Summit](https://etcsummit.com/) and released the [BUIDL for ETC](https://www.secondstate.io/etc/) IDE — even novice developers can now write and publish complete smart contract powered applications on the Ethereum Classic blockchain in minutes. [Check out a tutorial](https://hackernoon.com/easier-and-faster-dapps-on-the-ethereum-classic-blockchain-r9qn34a7)! All views and opinions expressed in the article are my own.*

## Church & State

Successful open source communities always have the divide of “church” and “state”. 

* Ideology activists, typically founding developers and early community organizers, are the uncompromising church. They participate in the project to make a statement. Decentralization! Free as in speech! Censorship resistant! Without them, we will all be selling our labor to the dark side. 

* Business people, applications developers, investors, or crypto traders, are the “state”. They are in it for practical improvements for society and themselves. 

Most blockchain/cryptocurrency conferences I have been to are dominated by the “state”. People care about cryptocurrency prices, mining profitability, or at the very least, what practical applications can be built on the technology. 

[ETC Summit](https://etcsummit.com) is different. It is a small, intimate conference, which means you can actually talk to people instead of just exchanging business cards or adding friends on social networks. The participants put a lot of emphasis on the political view we express through our software. It feels a lot like a church, and I like it. 

## Reconciliation & Collaboration
The overriding theme of ETC Summit this year is collaboration and reconciliation. The community looks forward to fresh collaborations with old and new friends. 

The ETC community itself has now put the drama of last year behind us. The ETC now has a well-funded core developer team that recently executed the successful [Atlantis hard fork](https://www.coindesk.com/ethereum-classic-successfully-forks-improving-interoperability-with-ethereum). The [ETC Labs](https://etclabs.org/) is funding developers, accelerator, studio, and other commercial programs. The [ETC Cooperative](https://etccooperative.org/) is organizing community events, building infrastructure, developer programs, and engaging external developers.

![](/images/20191006-etc-summit-recap-01.png)
The pillars of the ETC community, ETC Cooperative and ETC Labs shake hands on stage. 

Hold just days before Ethereum Foundation’s Devcon5, a very noticeable theme of this year’s ETC Summit is ETH / ETC collaboration. Could the ETC become the dominant PoW (and SoV) blockchain in the ETH 2 ecosystem? Could the ETC become the trustless on-ramp for the ETH 2 ecosystem? Could the ETC become the choice of developers as Ethereum becomes too congested? 

![](/images/20191006-etc-summit-recap-02.png)
ETH collaboration options

![](/images/20191006-etc-summit-recap-03.png)
The ETC community in action

The ETC community brings participants from all around the world together under the same umbrella. Check out Bob Summerwill’s [blog post](https://bobsummerwill.com/2019/10/03/addressing-east-west-disconnect-in-etc/) on the ETC translation initiatives. 

## Developers! Developers!
Developers from ETC Labs, ETC Cooperative, Ethereum Foundation, Parity, Cosmos, IOHK, ChainSafe, Second State, CyberMiles, and other partners are well represented the ETC Summit. Some of the notable technological innovations introduced in the conference are as follows.

The [Jade Suite](https://jade.builders/) is a suite of tools and services from ETC Labs to make it easy to develop dapps on any Ethereum compatible blockchains. It currently consists of a [service runner](https://medium.com/ethereum-classic-labs/jade-service-runner-f63e14c1b81b) for Ethereum RPC services and an open-source [blockchain explorer](https://medium.com/etclabscore/jade-explorer-a-minimal-block-explorer-for-the-ethereum-stack-a0df1aecdc38) for any Ethereum compatible blockchain.  

The [Open RPC project](https://open-rpc.org/) is an ETC Labs project that aims to support interactive JSON RPC documentation. Since JSON RPC is widely used in the Ethereum ecosystem, it provides crucial improvements to developer experience.

The [EtherCluster](https://www.ethercluster.com/) project is an ETC Cooperative project to provide the next generation Ethereum infrastructure as a service. It is completely open-source and built on current devops technologies such as the Kubernetes.

The [SOLL and LLVM EVM toolchain](https://blog.secondstate.io/post/20190901-etc-partners-with-secondstate/) is a deep tech project that aims to fundamentally transform and expand the capacity of the blockchain execution engine. The [SOLL](https://github.com/second-state/soll) project by Second State is an LLVM compiler for the solidity language. By moving to LLVM, we open up the potential for the blockchain to support and optimize for multiple programming languages, as well as targeting multiple backend execution engines especially WebAssembly based virtual machines. The [LLVM EVM project](https://medium.com/etclabscore/the-evm-llvm-is-coming-to-ethereum-classic-what-you-need-to-know-c13962f25571) is a collaboration between ETC Labs and Second State that aims to run LLVM bytecode in current EVM. 

The [Hyperledger Besu](https://www.hyperledger.org/projects/besu) client is a new Ethereum client based on the Hyperledger blockchain. It’s ETC support is developed and maintained by [ChainSafe](https://chainsafe.io/) with funding from the ETC Cooperative. As the ETC community thrives for client diversification, it is a welcome addition to the community. 

The [BUIDL for ETC](https://www.secondstate.io/etc/) an open source IDE that provides one-stop development, debugging, and deployment support for smart contracts and web3 decentralized apps on the Ethereum Classic blockchains. Following the [tutorials](https://docs.secondstate.io/buidl-developer-tool/demo-a-voting-dapp/ethereum-classic), even developers new to the blockchain space can get a dapp up and running on the Ethereum Classic blockchain in minutes.

## What About Miners
As the Ethereum Classic blockchain will probably be the dominant PoW blockchain in the Ethereum ecosystem. Mining hardware and mining pool operators play major roles in the ecosystem. Miners are well represented in the ETC Summit with multiple talks and panels. 

In fact, a new PoW-based ETC testnet was launched on stage. It is called [Mordor](https://www.thecoinrepublic.com/etc-summit-ethereum-classic-launched-mordor-its-new-testnet/). [Try it](https://github.com/eth-classic/mordor) and mine some Mordor tokens!

![](/images/20191006-etc-summit-recap-04.png)
The ETC Mordor testnet.

“ASIC resistant” mining algorithms have recently become a contentious issue in the Ethereum community. As Ethereum Classic positioned to become the dominant PoW blockchain in the ecosystem, its choice of mining algorithm is especially impactful. Even prior to the ETC Summit, there are [controversies](https://bobsummerwill.com/2019/09/17/progpow-author-kristy-leigh-minehan-uninvited-from-etc-summit/) around ProgPoW and its promoters.

On the other side of the argument, [ECIP-1049](https://github.com/ethereumclassic/ECIPs/issues/13) proposes the Ethereum Classic community to fully embrace ASIC miners with a SHA3-based mining algorithm. It is an argument for algorithmic transparency, which I found very interesting. SHA3 is transparent, safe, and easy to create ASIC machines from. Once Ethereum Classic becomes the dominant PoW blockchain for SHA3 mining, it will create a massive economic disincentive for SHA3 miners, who paid real money for their SHA3 ASIC machines, to attack the ETC. The downside, of course, is that ETC can no longer automatically “inherit” today’s ETH miners and their GPU-based hash power. If you are interested, Alex Tsankov has an [excellent write-up](https://medium.com/coinmonks/ecip-1049-why-ethereum-classic-should-adopt-keccak256-for-its-proof-of-work-algorithm-e45aee32d8a9) on the rationale. You can experiment with SHA3 mining today on the [Astor testnet](https://medium.com/@antsankov/the-what-why-and-how-of-astor-testnet-e7366ba2a730) for Ethereum Classic. [ETC holders express your sentiment on-chain here](https://opendapps.secondstate.io/ECIP1049_1570410786520.html).

## The Future is Bright
Overall, I really enjoyed the ETC Summit both as an attendee from the community, and as a commercial sponsor. Beautiful venue, great food, abundant coffee, and most of important of all, great conversations. From the summit, the future of Ethereum Classic looks bright. I look forward to contributing to the ecosystem.
