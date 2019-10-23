---
title: "Second State SOLL 编译器项目获得以太坊基金会的现金奖励"
date: 2019-10-22T01:01:23+08:00
draft: false
tags: ["Ethereum", "EWASM", "eth2.0","SOLL","LLVM","Solidity","WebAssembly"]
categories: ["Ethereum","zh","EWASM"]
---

[Second State](https://www.secondstate.io/) 是领先的面向区块链智能合约的开源基础架构软件的提供者，因为对[开源SOLL编译器项目](https://www.secondstate.io/)的贡献，获得了以太坊基金会的现金奖励。2019年10月20日在台北举行的CrossLink活动中，以太坊基金会的Vitalik Buterin向Second State 团队颁发了5000美元的奖金。

![SOLL project](/images/20191022-soll-project-01.jpg)


**SOLL是世界上第一个将Solidity智能合约编译为WebAssembly字节码，并成功部署到以太坊基金会官方Ewasm（以太坊WebAssembly）测试网中的工具链。**

本月初，Second State团队在日本大阪的2019年以太坊基金会Devcon5上[demo 了SOLL编译器项目的早期版本](https://www.youtube.com/watch?v=X-A6sP_HTy0)。[点此了解更多](https://www.secondstate.io/devcon5/)。

## 走向ETH 2 的重要路径

根据ETH 2 路线图的规划，ETH 2 需要一种新的智能合约执行引擎，称为Ewasm（以太坊WebAssembly）虚拟机。但是，经过多年的开发，针对Ewasm 的开发工具链仍然缺失。在SOLL 之前，没有简单的工具可以将Solidity 智能合约编译并部署到基于Ewasm 的区块链上。

通过对LLVM 的支持，SOLL 不仅完善了Ewasm 缺少的工具链，还把现代编译器基础结构引入了Solidity 编程语言。

有了对LLVM 的支持，SOLL 不仅可以在前端支持多种智能合约编程语言，例如Rust和C ++，而且可以在后端支持各种VM，例如Ewasm和EVM1.x。区块链上的应用程序开发将更加灵活和高效。

>Second State首席执行官[Michael Yuan](http://www.michaelyuan.com/) 博士解释了SOLL 项目背后的基本原理，“ SOLL 项目在企业开发人员和区块链世界之间架起了一座桥梁。我们欢迎所有开发人员使用为Ewasm 设计的SOLL 工具链。”

## 超越Ewasm

能够在后端支持多个执行引擎，这是基于LLVM的编译器工具链的主要优点。例如，Second State与ETC Labs之间正在进行的合作，正在朝着SOLL的EVM 1.0 后端努力。这使得基于LLVM的工具和优化功能可以在现有的基于EVM的区块链上使用，例如以太经典（Ethereum Classic），CyberMiles等。

>以太经典的核心开发Alan Li表示：“ SOLL EVM 项目的前进非常令人兴奋，将有效地塑造以EVM执行环境为基础构建的整个DApp 生态系统。”

此外，Second State将把SOLL 编译器工具链合并到非常易于使用的基于Web 的[BUIDL IDE](https://docs.secondstate.io/buidl-developer-tool/why-buidl) 中。BUIDL IDE 以智能合约作为后端，以web3作为前端，可以在数分钟内构建和部署完整的DApp。

延伸阅读

1. [SOLL项目源代码repo](https://github.com/second-state/soll)

2. [关于Ewasm的SOLL的视频演示](https://www.youtube.com/watch?v=X-A6sP_HTy0)

3. [EVM-LLVM项目](https://github.com/etclabscore/evm_llvm)

4. [Second State BUIDL IDE项目](https://github.com/second-state/buidl) 

5. [BUIDL入门指南](https://docs.secondstate.io/buidl-developer-tool/getting-started)
