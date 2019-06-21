---
title: "Libra深度剖析:智能合约并非生而平等 ①"
date: 2019-04-21T01:01:23+08:00
draft: false
tags: ["Ethereum", "Libra","Smart contract"]
categories: ["Smart Contract"]
author: "Second State"
---

本年度最受期待的科技新闻就是Facebook的加密货币Libra项目。Facebook于2019年6月18日发布了白皮书，同时发布了Libra项目第一版源代码以及技术白皮书。根据白皮书，Libra 的目标是建立去中心化的全球货币，该项目有着非常野心勃勃的加密经济学设计管理规则，同时其联盟中的合作伙伴也非常重磅。

![](/images/20190621-Libra-first-impressions-01.png)

>Libra区块链是一个去中心化的可编程的数据库，其被设计成用于支持一个波动性低的加密货币，它能作为有效的交换媒介服务世界上几十亿人 。——Libra白皮书。

但是，作为技术人员，我们对于其运用区块链技术的方式最感兴趣。为什么Libra项目要求有自己的一条区块链呢？对于应用程序开发者来说机会在哪里？对于企业以及传统的IT公司而言，从中可以得到哪些推断？我们将分三篇文章来讨论这几个问题。

1.	回顾Libra项目开发应用程序使用的是什么方法
2.	深度的解析Libra项目的一项核心应用
3.	怎样开发应用程序的教程

深度剖析Libra系列文章由Second State 出品。Second State 主要提供针对企业的区块链智能合约解决方案，已获得一线VC投资，目前处于未公开阶段。Second State 正在为业内领先的开源项目做贡献，即将发布第一批产品。
 
### 智能合约先行

Libra项目中最引人瞩目的设计功能之一，就是其智能合约先行的策略。就算是像以太坊这样的智能合约平台，智能合约的执行仅仅是一种交易的类型。以太坊原生操作仍然是币（coin）的交易。而Libra不同。智能合约是Libra上的一等公民。

![](/images/20190621-Libra-first-impressions-02.png)

Libra 区块链本身是用Rust 编写的，但是Libra上的应用程序是使用新的编程语言Move来编写。所有来自外部的区块链互动都由Move 程序来处理，在Libra 上，即便是一个币的转账，也是由Move程序来处理的。每一个Libra 节点都会运行一个虚拟机。虚拟机执行程序，并且记录共识达成后的结果。Second State认为智能合约先行的方式能够让Libra项目建立起一个功能多样的基础设施，这也能适用于未来的需要。

### 为什么使用Move 编程语言

那么，为什么我们需要一种新的编程语言？是因为安全和性能的要求。Libra 要建造一条新的链，因为目前市场上的区块链解决方案没有办法满足其对性能和安全的要求。
 
Facebook 和 Libra 想要建立专注于支付和资产数字化的区块链，因此他们创造了Move编程语言，内置了对不可更改且不可复制的资产的支持。Move编程语言是一个用于数字资产管理的DSL（域名特定语言）。

> 比特币在加密方面成就非凡，能够在数字世界创建无法复制的东西，有着非常巨大的价值。——艾里克斯米特，谷歌主席

Move 编程语言的名字来自于Move本身支持的基础操作器。Move操作器负责移动资产。Move  消除了原本常见的两步操作：减掉前面一个账户的余额，然后加到另外一个账户。Move 语言将资产与资源放在一等公民的地位。当然，Move语言也有其他重要的特性，使其在资产管理方面更加安全，更加健壮。

```public deposit(payee: address, to_deposit: Coin) { 

let to_deposit_value: u64 = Unpack(move(to_deposit)); 

let coin_ref: &mut Coin = BorrowGlobal(move(payee)); 

let coin_value_ref: &mut u64 = &mut move(coin_ref).value; 

let coin_value: u64 = *move(coin_value_ref); 

*move(coin_value_ref) = move(coin_value) + move(to_deposit_value); 

}```


```public withdraw_from_sender(amount: u64): Coin { 

let transaction_sender_address: address = GetTxnSenderAddress(); 

let coin_ref: &mut Coin = BorrowGlobal(move(transaction_sender_address)); 

let coin_value_ref: &mut u64 = &mut move(coin_ref).value; 

let coin_value: u64 = *move(coin_value_ref); 

RejectUnless(copy(coin_value) >= copy(amount));

*move(coin_value_ref) = move(coin_value) - copy(amount); 

let new_coin: Coin = Pack(move(amount));

return move(new_coin); 

}```

(Move 操作器示例)

Move编程语言是静态的，并且由编译器工具来发现错误和潜在的问题。

Move源代码被编译为由虚拟机执行的静态的IR (intermediate representation)代码。IR代码由工具进行检查并验证是否正确。

```public main(payee: address, amount: u64) { 

let coin: 0x0.Currency.Coin = 0x0.Currency.withdraw_from_sender(copy(amount)); 

0x0.Currency.deposit(copy(payee), move(coin)); 

}```

实际上，目前的Libra 资料仅仅有Move IR代码的案例。Move源代码的细节，在本文章发表时还未公布。

Move 编程语言和虚拟机是Libra项目的关键创新之处，但是Move 编程语言与传统的solidity 和vyper 智能合约语言，以及以太坊虚拟机和WebAssembly区块链虚拟机相比，所做出的妥协有哪些呢？

### 图灵完备性的牺牲
 
大多数DSL 特定语言系统都会就具体的任务进行优化，因此并不适用于广泛意义上的计算。Libra 并未直接表示Move编程语言是不是一个图灵完备的体系，但是，Move 专门针对金融交易进行优化，Move 系统可能并不适合用在开发加密货币游戏或博彩。

也就是说，Libra 软件对于大多数企业智能合约用例来说并不合适。

但还有其他的方面，Move编程语言在很大程度上不是智能合约。
 
### Move程序是真的智能合约吗？

Move程序必须进行编译，并且集成到一个Libra节点软件当中，对普通用户来说才是可用的。Libra区块链如果要支持新的Move程序，必须要暂停整条链，并且所有三分之二的验证人节点进行软件升级，才能够支持同样的Move程序，这在本质上意味着，每次要添加新的Move程序到区块链，都要进行硬分叉，期间伴随着区块链服务暂停。这不是智能合约，而是[chaincode]()。

智能合约的一个决定性的特点就是它有能力按照要求在区块链在并不需要暂停服务的情况下，通过共识，部署并且执行新代码，这个对于企业区块链或者是公链来说非常重要。

•	公链必须允许任何人在不需要得到授权也不需要暂停服务的情况下部署并执行智能合约代码。
•	企业级的区块链特别需要使用智能合约来创造自动化的商务决策。比如在不同几方之间，进行担保交易。雇员与合伙人必须能够按照实际需要来修改和部署智能合约，同时不需要暂停整条链
 
Libra项目作为一个，专注于金融交易的准入式区块链，它的最初的方案，是使用不可更改的chaincode。这样能够让整个系统更加安全，而且更加稳定，因为所有的Move程序都会至少要得到100个验证人节点中的67位的检查和允许。但是Libra项目所声称的目标是在接下来五年当中，进化成一条公链。我们相信Move架构会与区块链共同进化。

### 写在最后

本篇文章中我们聚焦讨论了Move编程语言以及由其驱动的智能合约。当然，Libra还有其它很有意思的技术创新。比如以下值得关注的点：

1.	以太坊每个节点都维护了一个全球性的数据库，在每个区块更新之后，数据库也会跟着更新；而Libra与以太坊不同，Libra 的数据库是基于不同版本的。Libra的状态数据库在每个交易完成后得到更新。对Libra来说，相较交易的概念而言，区块的概念没有那么重要。
2.	Libra 区块链表示，其最初的性能目标是每秒1000笔交易。很显然，这对于一个全球性的支付企业或者全球性的电商场景而言，已经足够了，因为VISA平均的TPS 在1700 左右。Libra 没有不切实际、不负责任地吹嘘百万TPS。
 
在接下来的系列文章中，我们将提供对Move 程序的深度解读，并向读者展示Move具体是如何工作的。敬请期待。
 

