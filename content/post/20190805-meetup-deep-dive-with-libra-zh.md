---
title: "Meetup#7：Libra 智能合约技术详解"
date: 2019-08-05T09:42:14+08:00
draft: false
tags: ["blockchain", "developer Community", "Libra"]
categories: ["developer"]
---

* Libra 在国会听证后，该何去何从？
* 在Libra 开发与在以太坊上开发有什么不同？
* ERC 20 token 能否在Libra 上做交互？
* 如何快速入手DApp 开发？

8月3日，在北京举办的技术沙龙《Libra 智能合约技术详解》详细探讨了上述问题。Second State 资深工程师龙智为在场30多名开发者讲解了[如何在Libra 上发布ERC-20 合约](https://blog.secondstate.io/post/20190719-how-to-issue-erc20-token-on-libra-with-move-language-zh/)，受到大家的赞不绝口地评价。

![](/images/20190805-libra-deep-dive-01.JPG)

Michael Yuan 博士为大家讲解了Second State 的新产品BUIDL，一款新的[区块链开发工具](https://buidl.secondstate.io/)。任何人都可以在BUIDL 上在一分钟内开发出自己的DApp。

![](/images/20190805-libra-deep-dive-03.jpeg)

Libra 是本年度最受期待的科技项目，[其最引人瞩目的设计就是智能合约先行的策略](https://blog.secondstate.io/post/20190621-libra-first-impressions-zh/)。而在以太坊上，智能合约仅仅只是一种交易的类型。

在Libra，账户由模块和资源两部分组成。模块类似以太坊智能合约的概念，资源由Move 系统保证安全，不会被复制，重用或丢弃。外部模块对本模块资源的修改受到了严格的限制，只能“move”，不能随意对资源赋值。

变量在以太坊上是可以Copy的，相当于克隆，原变量值不变仍可继续使用；但在Libra 上，用move 方式取值，变量move 给新对象后，原变量失效了。“这借鉴了Rust 的move 语义：读取变量时，必须制定取值放肆，要么是copy，要么是move。”龙智解释道，“这种只能’move‘的设计方式，相较以太坊更能够保证资产安全。”

具体到如何在Libra 上发行ERC-20 token，可以参考Second State 出品的深度剖析Libra 系列文章的第三篇：[《Libra深度剖析③ 如何基于 ERC20 标准在 Libra 上发布金融资产》](https://blog.secondstate.io/post/20190719-how-to-issue-erc20-token-on-libra-with-move-language-zh/)

在Libra 上部署与使用Token 的顺序：

1. 先将 token.mvir (含有 Token、TokenCapability 的 module) 部署到 Libra
2. 要想使用该 token 账户，必须先调用 init.mvir 将 Token.T 发布到账户的 resource 中；
3. 所有者可通过 mint.mvir 给其他拥有 resource Token.T 的账户增发 token；
4. 两个拥有 resource Token.T 的 account 可以通过 transfer.mvir 进行 token 转移。

![](/images/20190805-libra-deep-dive-02.JPG)

这次活动也邀请到了Ares Talk 创始人Ares 讲解Libra 在国会听证后的发展趋势，他认为Libra 完全停止开发性的可能性非常小。

关于这个问题，龙智也从技术角度给出了自己的答案,“从代码更新频率来看，Libra的技术开发一直没有停止”。

本次活动所用资源：

1. [Libra 概览](https://github.com/CyberMiles/education/blob/master/meetups/beijing/6-libra/Libra%20AresTalk.pdf)
2. [使用BUIDL 一站式快速开发DApp](https://buidl.secondstate.io/)
[BUIDL 技术文档](https://docs.secondstate.io/buidl-developer-tool/getting-started)
3. [Libra 技术详解](https://github.com/CyberMiles/education/blob/master/meetups/beijing/6-libra/Libra-dragon.pdf)
[在libra 发起交易完整代码](https://github.com/second-state/libra-research/tree/master/examples/ERC20Token)
4. Libra 技术解析系列文章：
* [Libra深度剖析① 智能合约并非生而平等](https://blog.secondstate.io/post/20190621-libra-first-impressions-zh/)
* [Libra深度剖析② Validator 如何验证交易](https://blog.secondstate.io/post/20190701-libra-first-impressions-zh/)
* [Libra深度剖析③ 如何基于 ERC20 标准在 Libra 上发布金融资产](https://blog.secondstate.io/post/20190719-how-to-issue-erc20-token-on-libra-with-move-language-zh/)

