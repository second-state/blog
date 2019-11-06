---
title: "The Next Generation Ethereum Virtual Machine — Ewasm VM"
date: 2019-10-31T10:01:23+08:00
draft: false
tags: ["smart contracts","Halloween"] 
categories: ["en"]
---


[Image: image.png]The next generation Ethereum Virtual Machine — Crosslink 2019 Taiwan

This is a wrap-up for Second State’s VP of Engineering, [Mr. Hung-Ying Tai]（https://github.com/hydai）’s presentation on SOLL, Ewasm VM's current research content and future direction. The sharing is very exciting, including content on EVM bytecode, Webassembly, Ewasm1.0 and Ewasm2.0

EVM bytecode and Webassembly (WASM)

When Ethereum's smart contract transaction is executed, for example, when transfer Token to another address, we read the EVM bytecode into the Ethereum virtual machine, and the EVM bytecode has the following characteristics:

* 256-bit staked-based virtual machine
* Many high-level instructions, such as: SSTORE, SLOAD, SHA3, EC, Call/Create contract
* Different from the physical system architecture (usually 32/64 bits), and 256 bits need to be done by simulation
* Less programming languages (Vyper, Solidity, ...)


Webassembly (WASM) is a binary programming language that allows suites developed in different programming languages to be used in browsers. WASM has the following features:

* Staked-based virtual machine: has a separate area space (scratchpad or memory), accessing the first three objects of the stack (16 EVM accesses)
* Support for 32 / 64 bit operations
* No high-order instructions
* The RISC instruction set can also correspond to the CPU ISA
* Larger community: Mainstream browsers are supported, and there are more programming languages (C++, Rust, GO, ...)

Ewasm 1.0

Let's take a look at the features of Ewasm:

* Ewasm is a subset of wasm
* Floating point arithmetic is not supported because no error is allowed
* You can only import the library of Ethereum and avoid importing ㄒ system library.
* Insert useGAS before each instruction to calculate the cost of GAS

Ethereum Environment Interface

There are many high-level instructions in the EVM like SSLOAD and SHA3. These commands are in Ewasm 1.0. because WASM can dynamically read libraries (modules). Ethereum defines the Ethereum Environment Interface so that clients can use different languages. Corresponding to the library, and it is easier to complete the prototype and upgrade.

The following slide is a list of functions defined by the Ethereum Environment Interface.
[Image: image.png]
Ethereum Environment Interface Definition.

How to remove illegal instructions?

Ewasm uses system contract to remove illegal instructions and add useGas bytecode, such as floating point numbers or illegal imports, in two ways:


* Check the contract's bytecode using the system contract
* Like the current precompiles running on the client, check the contract before deployment

The following slide shows the Ewasm 1.0 stack. Before the contract deployment, the Ewasm bytecode will be checked by Sentinal. After the successful deployment, the client will communicate with Heru (Wasm Engine) through EVM-C if the contract is to be executed.
[Image: image.png]Ewasm Stack


Performance problem

Does Ewasm perform better? The speaker shared the results of both EVM execution of Sha1 and BN128mul. It can be found that EVM is the faster when running BN128mul, mainly because WASM only supports 32/64-bit operations. For 256-bit it requires additional simulation (one 256-bit operation equals to twenty five 64-bit operations), so when running BN128mul in WASM will be slower than EVM.

Ewasm 2.0

Ewasm 2.0's smart contract is called Execution Environments (EE), which is different from Ewasm 1.0.

* EE is all written by WASM
* Because of the cross shard support, each EE is executed on a shard
* EE can only get state root, and the execution of the contract is not the same as before.
* EE is stateless

The following slide shows the ERC20 Token compared to Ewasm 2.0 and Ewasm 1.0 storage. Ewasm 1.0 has a corresponding key for each data, while Ewasm 2.0 only has state root, so it can only interact with state root.
[Image: image.png]
Ewasm 2.0 vs Ewasm 1.0


Phase One and Done

At the current stage of Ewasm 2.0 to phase one and done, there are also tested networks that can perform EE on shard blocks, and Ethereum also has open source Ewasm 2.0 test tool Scout.

Hello World for Ewasm 2.0

The above slide is Eth 2's Hello World EE. You can see that the first line in the main function reads the pre state root, then verify if the block data size is 0, and finally save the state root back. Eth 2's smart contract code are all in this form.


Conclusion

Ewasm 1.0 currently supports EVM 1 and most of the functions have a test net. Second state develops a compiler SOLL, which can compile solidity into Ewasm. [Check it out](https://blog.secondstate.io/post/20191022-soll-compiler-project/) if interested.

Ewasm 2.0 is still under study. The following slide is the direction of the research and contribution shared by the speaker.

Reference

[Crosslink 简报](http://url.hyd.ai/LRFVT)
[webassembly.org (http://webassembly.org/)](https://webassembly.org/)
[Scout](https://github.com/ewasm/scout)
[Second State’s Soll Compiler](https://github.com/second-state/soll)
[Compiling and deploying an ERC20 contract onto the eWASM testnet— YouTube](https://www.youtube.com/watch?v=X-A6sP_HTy0)
[Ewasm overview and the precompile problem: Alex Beregszaszi and Casey Detrio @ Ethereum \\\\ Part 1 — YouTube](https://www.youtube.com/watch?v=YW6hszjjMqo&feature=youtu.be)
[Ewasm overview and the precompile problem: Alex Beregszaszi and Casey Detrio @ Ethereum \\\\ Part 2 — YouTube](https://www.youtube.com/watch?v=a9hbycBMr_A)
[Wasm for blockchain&Eth2 execution: Paul Dworzanski,Alex Beregszaszi,Casey Detrio@Ethereum \\\\ Part 2 — YouTube](https://www.youtube.com/watch?v=iwU10WkWSBY)
[Ewasm for sharding](https://drive.google.com/file/d/19t4qCqEK2RPt0p1XYx-a2FdZSAlCq7H0/view)
[Ewasm updates](https://drive.google.com/file/d/1CRc0qBQTebNKw7NRZXzxbHovrigW0bqf/view)
[Ewasm design](https://github.com/ewasm/design)
[wasm-intro](https://rsms.me/wasm-intro)

Translated from traditional Chinese https://medium.com/taipei-ethereum-meetup/the-next-generation-ethereum-virtual-machine-ewasm-vm-2fe3fd9b94a4
