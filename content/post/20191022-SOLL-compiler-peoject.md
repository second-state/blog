---
title: "Second State receives a prestigious award from the Ethereum Foundation for the SOLL compiler"
date: 2019-10-22T01:01:23+08:00
draft: false
tags: ["Ethereum", "EWASM", "eth2.0","SOLL","LLVM","Solidity","WebAssembly"]
categories: ["Ethereum","en","EWASM"]
---

(Oct 20th, 2019, Taipei) - [Second State](https://www.secondstate.io/), a leading provider of open source infrastructure software for blockchain smart contracts, has been awarded a cash prize for its contribution to the open source [SOLL compiler project](https://github.com/second-state/soll). Vitalik Buterin from the Ethereum Foundation presented Hung-Ying Tai from Second State with the $5000 prize at the CrossLink event in Taipei on Oct 20th, 2019. 

![SOLL project](images/20191022-soll-project-01.jpg)

SOLL is the *world’s first*  toolchain that compiles Solidity smart contracts into WebAssembly bytecode and successfully deploys onto Ethereum Foundation’s official Ewasm (Ethereum flavored WebAssembly) testnet.

The Second State team [demonstrated](https://www.youtube.com/watch?v=X-A6sP_HTy0) an early release of its SOLL compiler project at the 2019 Ethereum Foundation Devcon5 at Osaka, Japan. Learn more here (https://www.secondstate.io/devcon5/).

## On the critical path to ETH 2

The ETH 2 roadmap calls for a new smart contract execution engine known as the Ewasm (Ethereum flavored WebAssembly) virtual machine. However, after years of development, the developer toolchain around Ewasm is still missing. Prior to SOLL, there were no easy tools to compile and deploy Solidity smart contracts to Ewasm-based blockchains.

[SOLL](https://github.com/second-state/soll) not only completes the missing toolchain for Ewasm, but also brings modern compiler infrastructure to the Solidity programming language through its support for the LLVM. You can watch a video demonstration on [how to use SOLL to compile an ERC20 contract in Solidity and then deploy it on the official Ewasm testnet](https://www.youtube.com/watch?v=X-A6sP_HTy0). 

With LLVM support, SOLL could not only support multiple  smart contract programming languages on the front end, such as Rust and C++, but also support various VMs on the back end, such as Ewasm and the EVM 1.x. Application development on the blockchain will be more flexible and efficient.

>[Michael Yuan](http://www.michaelyuan.com/), CEO of Second State, explained the rationale behind the SOLL project. He said “the SOLL Project builds a bridge between enterprise developers and the blockchain world. We invite all developers to try out the SOLL for Ewasm toolchain.”

## Beyond Ewasm

A key benefit of an LLVM-based compiler toolchain is the ability to support multiple execution engines on the backend. For example, [the ongoing collaboration between Second State and ETC Labs](https://blog.secondstate.io/post/20190901-etc-partners-with-secondstate/) is working toward an EVM 1.0 backend for SOLL. That allows LLVM-based tools and optimizations to become available on today’s EVM-based blockchains, such as the Ethereum Classic, CyberMiles, and many others. 

>“The SOLL EVM project has a very exciting journey ahead as it will effectively shape the entire dapps ecosystem built around the EVM execution environment,” said Alan Li, the core developer of Ethereum Classic.

Furthermore, the Second State BUIDL tool aims to incorporate the SOLL compiler toolchain into a very easy-to-use web-based IDE. The [BUIDL IDE](https://docs.secondstate.io/buidl-developer-tool/getting-started) can build and deploy complete dapps with smart contract as backend and web3 as front end within minutes.

Further reading

* [Source code repo to the SOLL project](https://github.com/second-state/soll). 
* [A video demonstration of SOLL on Ewasm](https://www.youtube.com/watch?v=X-A6sP_HTy0). 
* [The EVM-LLVM project](https://github.com/etclabscore/evm_llvm)
* [The Second State BUIDL IDE project](https://github.com/second-state/buidl).
* [BUIDL Getting Started guide](https://docs.secondstate.io/buidl-developer-tool/getting-started)

About Second State

Second State focuses on building and commercializing open-source blockchain infrastructure software. It develops a full stack of developer tools and runtime technologies for smart contract platforms, including the BUIDL IDE. Based in Austin, Texas, Second State has development offices in China (Beijing and Taipei) and Australia. A member of the ETC Labs incubator program, it recently closed a $3 million funding round led by Susquehanna International Group.

Founded in 2019, Second State’s vision is that complex, versatile business applications will drive enterprise blockchain adoption. Unlike traditional blockchain software providers who focus on the data ledger, Second State develops and commercializes “blockchain middleware,” which consists of [virtual machine](https://github.com/second-state/lityvm), [rules engine](https://www.litylang.org/business_rules/), [search engine](https://github.com/second-state/smart-contract-search-engine), and data services that empower a new generation of blockchain applications with much-improved user experience and developer productivity.
