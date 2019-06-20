---
title: "Libra First Impressions 
Part 1: Smart Contracts are not created equal"
date: 2019-06-20T10:01:23+08:00
draft: false
tags: ["hacks","smart contract","Ethereum","Wallet","blockchain"]
categories: ["en"]
---

![](/images/20190619-Libra-First-Impressions-01.png)

One of the most anticipated tech stories of the year is Facebook’s cryptocurrency — [the Libra project](https://libra.org/en-US/). On June 18th, 2019, Facebook released [the Libra white paper](https://libra.org/en-US/white-paper/) with the first iteration of the [project source code](https://github.com/libra/libra) and a [technical white paper](https://developers.libra.org/docs/the-libra-blockchain-paper.html). With a goal of a denationalized global currency ([Hayek 1976](https://en.wikipedia.org/wiki/The_Denationalization_of_Money)), Libra project has ambitious cryptoeconomic designs, governance rules, and an impressive [coalition of partners](https://libra.org/en-US/partners/).

The Libra Blockchain is a decentralized, programmable database designed to support a low-volatility cryptocurrency that will have the ability to serve as an efficient medium of exchange for billions of people around the world. -- Libra white paper.

However, as technologists, we are most interested in Libra’s approach to the blockchain technology itself. Why does Libra require its own blockchain? What are the opportunities for application developers? What're its implications for businesses and traditional IT? In this 3 part article tutorial, we will first review Libra’s approach to application development, then provide a deep dive into one of Libra’s core applications, and finally, guide you through a tutorial on how to create your own applications. 

This article series is produced by [Second State](https://www.secondstate.io/), a VC-funded, enterprise-focused, smart contract platform company. We are still in stealth mode while making contributions to leading open source projects. We are launching our first products soon.

### The smart contract first approach

One of the most striking design features of Libra is its “smart contract first” approach. One could argue that even on a smart contract platform like Ethereum, smart contract executions are just one type of transactions. Ethereum’s “native” operations are still coin transactions. Libra is different. Smart contracts are first class citizens on Libra.

The Libra blockchain itself is written in Rust, but Libra applications are written in a new programming language called [Move](https://developers.libra.org/docs/move-paper). All external interactions with the Libra blockchain are handled by Move programs. On Libra, even a coin transfer is handled by a Move program. Each Libra node runs a virtual machine that executes Move programs and records results when a consensus is reached.

We believe this “smart contract first” approach enables Libra to build a versatile infrastructure that can adapt to future needs.

### Why Move?

So, why do we need a new programming language? We need it for security and performance. Libra is building a blockchain because the current blockchain solutions on the market do not meet its performance and security goals. 

Facebook and Libra want to build a blockchain that is focused on payment and digitalization of assets. So, they created a programming language that has built-in support for immutable and non-duplicable assets. Move is a DSL (Domain Specific Language) for digital asset management. 

> Bitcoin is a remarkable cryptographic achievement and the ability to create something that is not duplicable in the digital world has enormous value. — Eric Schmidt, Google Chairman.

The Move language got its name from a basic operator supported by the language itself. The move operator is responsible for moving assets. It eliminates the two-step operation of subtracting from one account first and then adding to another account. The language is designed to treat assets and resources as first-class constructs. Of course, Move has other important features that make it secure and robust for asset management. 

* The Move language is static typed and checked by the compiler tools for errors and potential issues.
* Move source code is compiled into a static typed IR (intermediate representation) code to be executed by the virtual machine. The IR code can also be checked and verified for correctness by tools.

> In fact, the current Libra documentation only has Move IR examples. The Move source specification has not been released at the time of this article.

The Move language and virtual machine are the key innovations from the Libra project. But, what are the compromises Move has to make compared with traditional smart contract languages like Solidity and Vyper, and blockchain virtual machines like the Ethereum Virtual Machine and WebAssembly?

### The Turing completeness trade off

Most DSL systems are optimized for specific tasks and hence are not suitable for general computing. The Libra project does not explicitly state whether Move will be a Turning complete system. But by optimizing for financial transactions, the Move system is probably not well equipped for, say, cryptocurrency gaming and gambling.

However, that also means that the Libra software is not well suited for most enterprise smart contract use cases.

But there are more. In many ways, Move programs are not smart contracts at all. 

### Are Move programs really smart contracts?

Move programs must be compiled and built into the Libra node software in order for them to be available to users. For a Libra blockchain to support new Move programs, it must stop and ⅔ of all validator nodes must all upgrade their software to support the same MOVE programs. That essentially means a hard fork with service downtime for every Move program to be added to the blockchain platform. That is not smart contract. That is [chaincode](https://hyperledger-fabric.readthedocs.io/en/release-1.4/chaincode.html). 

A defining characteristic of smart contracts is the ability to deploy and execute new code on-demand onto the blockchain through consensus without any downtime. That is especially for public or enterprise blockchains. 

* Public blockchains must allow anyone to deploy and execute smart contract code without permission or service disruption.
* Enterprise blockchains typically utilize smart contracts to make automated business decisions, such as escrowing between parties. Employees and partners must be able to change and deploy contracts as needed without disruption.

As a permissioned blockchain focused on financial transactions, Libra’s initial approach with fixed chaincode could make the system safer and more stable as all Move programs will be reviewed and approved by 67 out of 100 validators. But Libra’s stated goal is to evolve into a public blockchain over the 5 years. We believe that the Move architecture will have to evolve with it. 

### Stay tuned

In this article, we focused on the Move programming language and the smart contracts it enables. Of course, Libra has other interesting technology innovations. Here are a few notable ones. 

* Unlike Ethereum, where each node maintains a global state DB that gets updated after each block, Libra features a versioned state DB. The Libra state DB gets updated after every transaction. The concept of “block” is much less important than “transaction” in Libra. 
* The Libra blockchain’s stated initial performance goal is 1000 TPS (transactions per second). That is certainly enough for a global payment (or e-commerce) business as the VISA network averages about 1700 TPS. There is no unrealistic and irresponsible boasting of “million TPS”. 

In the next article in this series, we will provide a deep dive into one of Libra’s bundled Move programs and show you exactly how it works. Stay tuned. 
