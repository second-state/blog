---
title: "Ethereum Classic partners with Second State to build the next generation Ethereum infrastructure"
date: 2019-09-01T01:01:23+08:00
draft: false
tags: ["Ethereum Classic", "Ethereum", "LLVM", "smart contract", "development"]
categories: ["Ethereum"]
---

[Ethereum Classic Labs](https://etclabs.org/) (ETC Labs), the developer and maintainer of the Ethereum Classic blockchain (ETC), announced today that it has entered a partnership with a leading blockchain infrastructure developer, [Second State](https://www.secondstate.io/), to create open source toolchain and runtime software that powers the next generation of Ethereum-compatible blockchains, including the Ethereum Classic.

This partnership will bring high-impact enhancements into the ETC ecosystem, and make the ETC blockchain a premier destination for smart contract developers.

![ETC labs and Second State](/images/20190902-etclabs-01.jpeg)

# The toolchain on blockchain, current situation

The values of blockchain networks (and cryptocurrencies) depend on real world applications that run on such networks.

However, today’s Ethereum lacks sufficient tooling support for application developers. For example, Solidity has been the official smart contract language of Ethereum and is the only viable choice for writing a contract for production although it is widely criticized for lack of features especially security safeguards. Application developers expect their favorite languages to be supported on blockchains.

>“We fully support the continued innovation of smart contracts. This project will help make them more secure and more accessible to a greater number of developers, which will benefit both the ETC and ETH communities.” – Terry Culver, CEO of ETC Labs

On the other hand, the current smart contract runtime on Ethereum, EVM, is limited in both features and performance. Today’s Ethereum smart contracts are simple and often insecure programs. The Ethereum community believes that the future is WebAssembly-based virtual machines (eWASM), but unfortunately production-ready eWASM is still a least a year away. In order to bridge the gap between runtime generations, the compiler toolchain must be able to seamlessly support both current EVM and multiple alternatives of future WASM-based EVMs.

Together with Ethereum core developers, Second State brings in much needed engineering and product expertise to innovate both on programming language tools and execution environments. Such innovations will first become available on the Ethereum Classic blockchain.

> “We are proud be part of the development effort to make Ethereum Classic the most technically advanced and developer friendly blockchain in the world.” – Michael Yuan, CEO of Second State

# Building LLVM on Ethereum
The best way to build a future-proof smart contract programming language toolchain is to bring the large and mature LLVM ecosystem into Ethereum.

The [LLVM](https://llvm.org/) project is perhaps the toolchain ecosystem from the fields of traditional software that the blockchain community could benefit most from. LLVM is widely used as the compiler which translates source code into machine code. A lot of our daily gadgets, such as phones, smart TVs, routers, laptops, has received tremendous performance improvement thanks to LLVM optimizations. Collaborative projects [LLVM-EVM](https://github.com/etclabscore/evm_llvm) and [SOLL](https://github.com/second-state/soll) from ETC Labs and Second State aim to bring LLVM into Ethereum.

The LLVM ecosystem provides perhaps the best toolchain ecosystem in the open source world. Thousands of developers are contributing to it daily. A good amount of them stood the test of time, which can be easily ported to the blockchain, saving tremendous amount of resources.

## Impacts of the LLVM-based toolchain
### More programming languages coming to ETC
The developer community wants diverse programming language support for smart contracts. Languages should be chosen for expressiveness, creativity, and better suitability for each project. Thanks to the EVM LLVM project, we will soon be able to support more programming languages, such as Rust, Golang, C and C++, on ETC and beyond.

### Support for current and future EVMs
The LLVM is widely supported in the WebAssembly (WASM) community. The new toolchain will support the current generation EVM on ETC and ETH today. It will support next-generation WASM-based EVMs as they are developed.

### A whole ecosystem of developer tools
The LLVM is attractive to the developers because of its strong ecosystem. Not only have they stood the test of time, they also receive tremendous and continuous supports from the LLVM community around the globe. High quality tools such as debuggers, linkers, optimizers, transpilers, super optimizers, and so on, will be available to the Ethereum community.

### Further optimized gas consumption
LLVM’s collection of time-proven, widely adopted optimizers could further reduce gas consumption of contracts. The partnership aims to develop new EVM-specific gas optimizations to further improve gas consumption in smart contracts, especially on the ETC blockchain.

### Long term community support
The LLVM community is committed to supporting LLVM project to an extended lifespan. Similarly, developers who base their development on LLVM will continue to receive the benefits and supports from the active LLVM community.

# Towards smart contract 2.0
Second State and ETC Labs will also collaborate to design, engineer and improve the smart contract runtime environment of ETC blockchain.

Smart contract runtime is a critical cornerstone component of the blockchain. The technological choices today will shape ETC’s decentralized application ecosystem for the next 5 to 10 years. The design and implementation not only have to be aligned with the underlying blockchain characteristics, but also have to carefully consider performance, reliability, compatibility, interoperability and usability, et cetera. By continuously improving our smart contract platform, decentralized application developers will keep receiving the benefits and support from our latest technology progress.

Second State and ETC Labs are committed to ensuring a high-performance, easy-to-use, reliable, secure and future-proof smart contract execution environment for ETC, providing a quality infrastructure to the Ethereum family’s developer community. We believe that such effort is critical to the success of the decentralized application ecosystem.

# About
## About ETC Labs

![ETC labs](/images/20190902-etclabs-03.jpeg)

[ETC Labs](https://etclabs.org/) is dedicated to providing guidance, resources and technical resources to help the community understand, build and create impactful decentralized applications. Our goal is to empower the community to build impactful dapps and solutions to encourage adoption, utilization, and advancement of Ethereum Classic and embrace the true values of the philosophy and technology.

## About Second State

![Second State](/images/20190902-etclabs-04.jpeg)

[Second State](https://www.secondstate.io/) focuses on building and commercializing open source blockchain infrastructure software. It develops a full stack of developer tools and runtime technologies for smart contract platforms, including the [BUIDL IDE](https://buidl.secondstate.io/). Second State is based in Austin, Texas, and has development offices in Taipei, Beijing, and Australia. It has recently closed a $3M funding round led by Susquehanna International Group. It is also a member of the ETC Labs incubator program.
