---
title: "Polkadot is getting WebAssembly-based Ethereum 2.0 virtual machine from Second State"
date: 2020-03-02T20:01:23+08:00
draft: false
tags: ["ewasm","Solidity","polkadot","Web3","Ethereum","WebAssembly"]
categories: ["en"]
---

[Polkadot](https://polkadot.network/)’s founder, Dr. Gavin Wood, is one of the original architects of the Ethereum Virtual Machine (EVM). Dr. Wood, the co-author of “[Mastering Ethereum](https://www.oreilly.com/library/view/mastering-ethereum/9781491971932/)”, is also the author of [the Ethereum yellow paper](https://ethereum.github.io/yellowpaper/paper.pdf); a formal definition of the Ethereum protocol (in mathematical terms), which also provides an in-depth explanation of how the EVM actually works.

![web3 foundation grant](/images/20200228-polkadot-01.png)

Some may be aware that the Polkadot ecosystem did not initially come with a Turing-complete virtual machine like the EVM. In fact, the Substrate framework, which underlies blockchains in the Polkadot ecosystem, is focused on Runtime Modules that build application functionalities and logic directly into the blockchain itself. This makes sense in a cross-chain ecosystem, as each application can be served by its own blockchain instead of a smart contract. These are called application-specific blockchains, and they can exchange results and assets via the Polkadot protocol.

Those who deploy their own application-specific blockchain, are essentially required to manage a complex infrastructure of nodes, networks, operation systems and so forth. This approach seemingly goes against the larger trend in IT today. As a general rule, infrastructure complexities for developers are constantly being reduced. This is evidenced by the continual rise of products and services in the genres of serverless computing architectures and microservices, as well as low-cost dynamically-managed remote-machine offerings.

> In a serverless world, developers just need to upload their code to the cloud, and users can access it and pay for the usage.

The serverless vision is, in fact, very close to the smart contract model on Ethereum compatible blockchains.

To support publicly submitted, unsecured application (smart contract) code, we need a Turing-complete, secure, and high-performance virtual machine on blockchain nodes.

In late 2019, a large developer, the Aragon Project, decided to [leave the Polkadot ecosystem](https://blog.aragon.one/aragon-chain/) due to the lack of a viable blockchain virtual machine at the time. After that, the Polkadot team [added EVM support](https://www.parity.io/substrate-evm/) on the Substrate framework which in turn enabled EVM blockchains in the Polkadot ecosystem.

Then in Feb 2020, the Web3 Foundation, [announced that it was funding Second State to bring the next generation Ethereum virtual machine](https://medium.com/second-state/second-state-awarded-a-grant-to-bring-next-gen-ethereum-infrastructure-to-polkadot-ecosystem-6545fb2267fc), known as the Ethereum flavored WebAssembly (Ewasm), into the Polkadot ecosystem; building on top of the very popular [WebAssembly](https://webassembly.org/) technology.

Ewasm is a high performance and flexible virtual machine for the future of the Ethereum ecosystem, and hence will be used by the majority of blockchain application developers. This project will bring Ethereum developers and applications into the Polkadot ecosystem; expanding support for the Ethereum protocol inside the Polkadot platform.

[Second State](https://www.secondstate.io/) is a leading developer of WebAssembly-based virtual machines for server-side applications. Ewasm is an extension of its generic [Second State VM (SSVM)](https://github.com/second-state/SSVM). The goal of the SSVM is to enable high performance, highly manageable, and secure microservice applications. It works with existing blockchain and web service frameworks including Substrate, GETH, Tendermint etc. (on the blockchain side), as well as NodeJS, Python Django, RoR, PHP, Java etc. (on the web service side). To learn more about how WebAssembly can help cloud and web services developers, [check out this](https://docs.secondstate.io/beginners-guide-to-webassembly/why-webassembly).

As software eats the world, infrastructure must be adapted to meet developer needs. Developers clearly want “serverless” and less infrastructure. WebAssembly and SSVM-based open source solutions are here to help!





