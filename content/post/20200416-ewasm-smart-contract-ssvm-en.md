---
title: "Second State released SSVM version 0.5.0 for the new generation Ethereum virtual machine Ewasm"
date: 2020-04-16T10:01:23+08:00
draft: false
tags: ["smart contract","Ethereum","ETH 2.0","blockchain"]
categories: ["en"]
---

The Ethereum flavored WebAssembly (Ewasm) virtual machine is a key infrastructure component of the next generation Ethereum (Ethereum 2.0) ecosystem. Second State recently released a new version of its leading Ewasm software, empowering developers to write tomorrow’s Ethereum 2.0 smart contracts today!

![](/images/20200416-ewasm-smart-contract-ssvm-01.png)

The [Second State Virtual Machine (SSVM)](https://github.com/second-state/SSVM) is a high-performance, server-optimized WebAssembly runtime. SSVM supports multiple WebAssembly extensions, including Ewasm. In the recently released SSVM 0.5.0 software, the SSVM-evmc extension specially supports Ewasm. For details, see the release notes: https://github.com/second-state/ssvm/releases/tag/0.5.0


## The ssvm-evmc empowers developers


The ssvm-evmc Ewasm VM has passed all of stEWASM's stateful Ethereum test cases. This means that ssvm-evmc software has fully met Ethereum Foundation's specifications on Ewasm. This makes Second State the first team to achieve this important goal outside of the Ethereum Foundation.

Ethereum developers can compile ethereum smart contracts into Ewasm bytecode and deploy them onto any blockchain that is compatible with the ssvm-evmc VM. Public blockchains such as Ethereum, Ethereum Classic, Polkadot, and CyberMiles are all going to be supported. For now, the best blockchain system to experiment with the ssvm-evmc Ewasm VM is [Second State's DevChain](https://github.com/second-state/devchain) based on the open source [CyberMiles blockchain software](https://github.com/CyberMiles/travis).

The ssvm-evmc virtual machine manages changes in the state of the application (that is, storage variables defined by Ethereum) according to the Ethereum protocol. It rejects execution of opcodes with uncertain results, such as general floating point math operations. It provides finely grained calculation of resource consumption, known as gas, based on executed opcodes.

Going forward, the ssvm-evmc will natively support 256-bit integer computation, thus increasing the execution speed of the Ethereum smart contracts by more than 10 times.

According to the [tutorial](https://docs.secondstate.io/devchain/getting-started/run-an-ewasm-smart-contract) published by Second State, any developer can use the open source Second State DevChain software to launch an Ewasm blockchain in 10 minutes. They can then deploy and experiment with Ewasm smart contracts on the actual blockchain.

![](/images/20200416-ewasm-smart-contract-ssvm-02.png)

With the support of Second State's SOLL compiler, Ethereum developers can compile commonly used ERC20 contracts into Ewasm bytecode and deploy them to the Ewasm devchain via ssvm-evmc. [SOLL compiler](https://github.com/second-state/SOLL) supports Solidity and YUL source code.

Second State Devchain is an open source software based on the CyberMiles public blockchain software. The CyberMiles Foundation will also release a new Ewasm testnet based on ssvm-evmc in the near future. Developers will then be able to deploy next generation Ethereum smart contracts onto the CyberMiles public Ewasm testnet.


## What does this mean for Ethereum 2.0?


In the official road map of Ethereum 2.0, its implementation has four phases. Phase 0 and phase 1 do not involve the deployment and execution of smart contracts. However, smart contracts are at the core of the Ethereum blockchain. The primary goal of Phase 2 is to support the smart contract virtual machine. So far, Ethereum 2.0 has a long way to go before it reaches phase 2.

At the DEVCON5 2019 in Osaka, Second State team successfully demoed the SOLL compiler to the Ethererum Foundation team. SOLL was [the world's first compiler toolchain](https://blog.secondstate.io/post/20191022-soll-compiler-project/) to support Ewasm and was subsequently award a cash prize by the Ethereum Foundation.

With the advancement of SOLL compiler + ssvm-evmc virtual machine + Second State Devchain, the [Second State](https://www.secondstate.io/) team continues to build out a full stack of open source tools and runtimes for Ethereum 2.0. Our future is bright!


