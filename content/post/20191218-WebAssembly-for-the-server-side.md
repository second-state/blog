---
title: "Second State releases WebAssembly for the server side"
date: 2019-12-18T10:01:23+08:00
draft: false
tags: ["WebAssembly","Cloud Computing"]
categories: ["en"]
---

*An open source WebAssembly runtime optimized for blockchain, AI, and cloud services*

(12/17/2019, Taipei)The [Second State](https://www.secondstate.io/) team has released a “developer preview” of the [Second State Virtual Machine (SSVM)](https://github.com/second-state/SSVM). It is a WebAssembly (Wasm) compatible runtime engineered from ground up for server side applications. Get it from Github here: https://github.com/second-state/SSVM

Wasm started as a high performance execution engine inside web browsers. It is language agnostic (supporting popular new languages such as Rust), platform agnostic, lightweight, fast, and also provides native hardware access through its modular security model.

> These features make Wasm a great execution engine for cloud-native microservices.

* Compared with Java and JavaScript virtual machines, Wasm supports many more programming languages, is much lighter, and provides native access to hardware features.
* Compared with containers like Docker, Wasm is much lighter and faster. Wasm apps run on any host system without change. Wasm also has a more refined modular security model for accessing native OS and hardware.

The Second State VM is focused on addressing server side use cases.

Wasm follows the well traveled paths of Java and JavaScript. They all started as client side technologies and would end up making great impacts on the server side infrastructure. — Dr. Michael Yuan, CEO of Second State.

This initial release of the SSVM provides a fully Wasm compatible VM with the following two important server-side enhancements.

### Blockchain smart contracts

Through the Second State Ethereum WASI module, SSVM can run in a mode that is compatible with the Ethereum protocol; allowing it to become the drop in replacement of virtual machines in next generation blockchain smart contract platforms.

Known as the Ethereum flavored WebAssembly (Ewasm), the Ethereum WASI module manages application states according to the Ethereum protocol (ie storage variables), prohibits all non-deterministic opcodes (eg floating number ops), measures computational costs on a per opcode basis (gas meter), and integrates with Ethereum’s blockchain data interfaces (ie EEI and EMVC).

Going forward, it is going to support 256 bit integer operations natively, resulting in 10x performance gain for Ethereum applications.

### Hardware AI

The SSVM includes a special WASI module developed in collaboration between Second State and Qualcomm. It provides native access to hardware AI accelerators built into the popular Snapdragon CPUs via the Qualcomm Hexagon SDK.

With the Qualcomm WASI module, SSVM can improve AI application performance by 1000x on hundreds of millions of Snapdragon devices.

### What’s next?

In the coming several weeks, the Second State team will focus on making SSVM easier to use for developers.

* Create a new Ewasm testnet based on the [SSVM](https://github.com/second-state/SSVM) and [the Second State DevChain](https://github.com/second-state/devchain). This testnet will support both Ewasm and traditional EVM smart contracts at the same time.
* Create a hosted SSVM service that enables public developers to deploy and manage microservices written in Rust. The service will be open source and can be hosted by anyone in addition to Second State.
* Demonstrate how the AI accelerator WASI modules can be used on the server side together with the above mentioned hosted SSVM service.
* Continue to work on developer tools for microservices, such as the [BUIDL tool](https://www.secondstate.io/buidl/) for blockchain-based decentralized services.

If you are a developer interested in next generation cloud services, please get in touch via email at contact@secondstate.io and subscribe to the [Wasm medium publication](https://medium.com/wasm)!

