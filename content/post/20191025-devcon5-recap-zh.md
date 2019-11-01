---
title: "一文带你了解Dev出on5"
date: 2019-10-24T09:42:14+08:00
draft: false
tags: ["blockchain", "Ethereum", "Devcon5","libra","Uniswap"]
categories: ["ethereum","zh"]
---

以太坊基金会 Devcon 是以太坊生态系统中最重要的开发者、设计师和研究人员的年度聚会。今年在日本大阪举行的 Devcon5 是第五届以太开发大会，与往年一样，Devcon5 是以太坊开发方向的信号灯。

注：Second State 受邀在 Devcon5 上进行了演讲，我们演示了世界上第一个在 Ewasm 官方测试链上编译和部署 Solidity 智能合约的工具链。

## 最大的新闻是没有新闻

去年在布拉格的 Devcon4 中，人们对 ETH 2 （安全性）以及以太坊扩容解决方案（通常称为 Plasma）非常乐观。布拉格 Devcon4 的总体感觉是：终于弄清了如何构建可扩展的去中心化智能合约平台了！但是，时间快进到如今 2019 年的大阪 Devcon5，许多乐观情绪都已不见。ETH 2 的深入研究仍然是进行时。过去一年中的许多探索，都未能达到我们期望的结果。

在大阪，大多数人似乎都同意整个 ETH 2 的工作需要重启，关于 ETH 2 什么时候能够发布的信息几乎没有。也许，Devcon5 最重大的新闻，是压根没有实实在在的 ETH 2 新闻和落到实处的计划。Devcon5 会议的第一天，[CoinDesk](https://www.coindesk.com/scam-or-iteration-at-devcon-ethereum-diehards-still-believe-in-2-0) 就此主题发表了一篇有争议的文章。

## ETH 1.x 回来了

正因为离 ETH 2 可能还有好几年，现在人们开始重新专注在如何让今天的以太坊区块链更适合应用程序开发者使用。例如，它将尝试通过更改 gas 费，收取“state 租金”并优化节点客户端存储，以减少节点上 state 数据的膨胀。

有的讨论是关于如何在 ETH 1.x 区块链上启动下一代基于 WebAssembly 的以太坊虚拟机（称为以太坊 WebAssembly 或 Ewasm）。Ewasm 可以显着改善以太坊上的应用程序开发者体验。但是，Ewasm 工具链和执行引擎仍在开发中。

![](/images/20191024-devcon5-racap-01.png)

我们 [Second State](https://www.secondstate.io/) 团队所做的贡献之一，就是创建了世界上第一个能够在官方的 Ewasm 测试网上编译和部署 Solidity 智能合约的[工具链SOLL](https://github.com/second-state/soll)。我们的工具链基于 LLVM 基础架构，使其能够跟上编译器社区的未来发展。[点击了解](https://blog.secondstate.io/post/20191014-secondstate-at-devcon5-zh/)更多我们为改善 EVM 体验所做的努力。

## 与 ETC 和解

以太坊经典（ETC）区块链是原始的 ETH 1.x 区块链。ETC 在保持应用程序层与 ETH 2 的兼容性的同时，仍然致力于实现 PoW 共识。

随着 ETH 2 转向 PoS 和多链架构（分片），ETC 很有可能会继承 ETH 挖矿社区并成为 ETH 2 生态系统中的 PoW 价值存储（store-of-value）代币。

![](/images/20191024-devcon5-racap-02.png)

在 Devcon5 上，ETC Labs 在主旨演讲里探讨了合作和治理。和解的信号呼应了 Devcon5 召开前几天在温哥华举行的[ETC 峰会](https://blog.secondstate.io/post/20191006-etc-summit-recap-zh/)上的氛围。很高兴看到 ETH 和 ETC 社区重归于好。我们期待看到 ETC 的更多创新和贡献！

## Layer 2：第 2 层扩容

随着比特币闪电网络的成功启动，开发者 本来对 ETH 的第二层扩展解决方案非常乐观。但是，在过去的一年中，ETH 是第 2 层解决方案的首选方案 Plasma 遇到了许多问题和挫折。在 Devcon5，我们仍然没有有效的 Plasma 解决方案。

但是，一种全新的方案出现了。 Uniswap 团队展示了专为 Uniswap 交易所应用程序设计和优化的第 2 层解决方案。称为 [Unipig 交易所](https://unipig.exchange/welcome)，即时交易确认非常快速。在以太坊测试网上，它的目标是达到 2000 TPS。这种“特定应用程序的第 2 层”方案是否适用于以太坊上最流行的应用程序？答案可能会在明年的 Devcon6 上揭晓。

## 可组合性问题

随着以太坊发展成为去中心化金融（DeFi）应用的公链，人们对 ETH 2 技术架构提出了严重的担忧。许多 DeFi 应用程序依靠智能合约之间的交互来起作用。例如，贷款合约可能需要与代币合约进行交互以管理其借出的代币。ETH 2 分片具有不同智能合约并可能存在于不同区块链上，是否能够继续保证智能合约之间的确定性交互作用？我们仍然可以构建包含多个智能合约的应用程序吗？这就是 ETH 2 的可组合性，这点在 Devcon5 上引发了很多讨论。

Devcon 期间，Vitalik Buterin 发表了一篇文章，讨论如何解决这个问题。他的答案是什么呢？大致是说，除了某些条件不行，应用程序开发者仍然可以编写 Defi 智能合约。但是，一些开发者仍然没被说服，因为这些条件可能正是一些 DeFi 应用程序所必须的。感兴趣的读者可以阅读[ Vitalik 的文章及评论](https://ethresear.ch/t/cross-shard-defi-composability/6268)。

## 持续专注开发者体验

以太坊成功的关键，始终是多元化且体量大的开发者社区。这也是为什么 Devcon 一开始就称为 Devcon 的原因。

![](/images/20191024-devcon5-racap-03.png)

本届 Devcon5 持续专注耕耘开发者社区，大会为开发者相关的内容分配了大量的会话和发言时间。Consensys 的 Joseph Rubin 在主旨演讲中，提出了以太坊力争达到 1 百万个开发者的目标。

Second State 还为新手开发者提供了工具[BUIDL IDE](https://www.secondstate.io/buidl)，以帮助所有开发人员快速上手以太坊应用程序开发。试试 BUIDL IDE，您可以几分钟内在公链上发布您的第一个基于 Web 的 dapp！

## Libra ，房间里的大象

2019 年的一项重大新闻，是 Facebook 通过 Libra 稳定币项目进军加密货币领域。来自 Cosmos 基金会和以太坊基金会的成员共同宣布了 OpenLibra 项目，该项目旨在创建类似 Libra 的稳定币，不同的是 OpenLibra 具有开源软件和开放治理。但是，以太坊社区中的许多人都不看好这个它。

![](/images/20191024-devcon5-racap-04.png)

尽管 Facebook Libra 最近遭到监管机构和合作伙伴的质疑，我相信 Libra 作为一项加密货币实验和软件创新将在未来几年内不会过时。感兴趣的读者可以查看我们有关 [Libra 的MOVE 语言和VM 的文章](https://blog.secondstate.io/tags/libra/)。

## 路在何方

总体而言，Devcon5 上的经历是比较愉快的。大阪是举行会议的好地方-治安优良，干净整洁，食物美味可口，交通非常便利。虽然 ETH 2 和 Plasma 开发遭受了挫折，但社区仍然继续吸引开发者并寻找前进的方法。

相信随着以太坊 1.x 资金的增加（来自以太坊基金会和 ETC Labs），我相信我们将在以太坊应用程序层持续看到创新和改进，包括新的 Ewasm 虚拟机，工具和特定于应用程序的第 2 层解决方案。对以太坊的未来，我们保持乐观。





