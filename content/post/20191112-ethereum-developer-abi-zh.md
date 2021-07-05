---
title: " ABI | 以太坊开发A2Z "
date: 2019-11-12T09:42:14+08:00
draft: true
tags: ["blockchain", "Ethereum", "developer","smart contract","BUIDL"]
categories: ["ethereum","zh"]
---

快速了解以太坊开发，快速 get 复杂的技术概念！
Second State 推出以太坊开发A2Z 系列，结合区块链开发的实际情况，带你了解以太坊开发的点点面面。

![](/images/20191112-abi-01.png)
这篇文章是该系列的第一篇，为大家讲述智能合约ABI。

简短版：区块链智能合约ABI是智能合约与区块链应用程序之间的接口。智能合约被像BUIDL这样的编译器编译成二进制文件后，会自动生成ABI。ABI是JSON文本文件，记录了智能合约的function 和参数。在用户使用区块链应用程序时，需要通过ABI 来调用智能合约。

## 什么是ABI

ABI是Application Binary Interface的缩写，是其他应用程序调用二进制应用程序文件的接口。ABI 是一种约定俗成的规范。

举例来说，夏天打开空调降温，通常需要一个遥控器。我们通过调节遥控器的按钮，来让空调执行制冷，降低/调高温度等操作。遥控器就相当于一个ABI，上面记录了与之相匹配的空调能够实现的功能与参数：制热/制热，空调能够达到的最高与最低温度是多少。随着空调本身功能的增多，比如增加了睡眠模式，通风等，遥控器（ABI）上的按钮也会随之丰富起来。

ABI 实现了用户与程序的互动。

## 以太坊开发中的ABI

在以太坊/区块链开发中，智能合约ABI 非常常见。智能合约编译后，编译器自动生成ABI 来记录智能合约的function与参数。当区块链应用被使用时，人们通过ABI 来调用智能合约，实现相关功能。

以区块链基础软件技术公司Second State 开发的 BUIDL IDE 内置的默认代码为例，合约代码如下：

![](/images/20191112-abi-02.png)

经过BUIDL 编译后，我们获得了如下的ABI。智能合约生成的ABI是JSON 格式的数组，包含两组参数，一个是参数名称，一个是参数值。在调用智能合约时，通过ABI 了解该智能合约实现了哪些功能，参数又是什么。
```
[{"constant": false,"inputs": [{"name": "x","type": "uint256"}],"name": "set","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "get","outputs": [{"name": "","type": "uint256"}],"payable": false,"stateMutability": "view","type": "function"}]
```

注意：ABI 并不是唯一的。如果智能合约实现的功能一样，那么编译出的ABI 也是一样的。

当我们使用BUIDL 进行下一步DApp 开发工作时，会发现JavaScript 版块记录了当前智能合约的ABI，使应用程序能够调用智能合约，与之互动。

![](/images/20191112-abi-03.png)

## 关于[BUIDL IDE](https://www.secondstate.io/buidl/) —— 一站式区块链开发工具

BUIDL IDE是完全基于浏览器的集成开发工具，开发者可以用BUIDL轻松地创建和部署区块链应用（DApp），无需安装任何软件和区块链钱包，从而节省大量时间。

BUIDL 功能介绍：

* **内置账户管理系统，不需钱包软件；**
* 编写，编译和部署智能合约；
* **开发包含HTML5 和 JavaScript 的DApp；**
* **一键把DApp发布到 Github Pages**；
* 可以部署在以太坊和任何与以太坊兼容的区块链；
* 访问内置Web3库；
* **支持基于规则的智能合约；**
* **内置基于ElasticSearch 的实时智能合约搜索引擎；**


更多资料：

* BUIDL 介绍页
    https://www.secondstate.io/buidl/
* BUIDL IDE
    https://buidl.secondstate.io/
* BUIDL 文档
    https://docs.secondstate.io/buidl-developer-tool/getting-started
* BUIDL GitHub 代码
    https://github.com/second-state



