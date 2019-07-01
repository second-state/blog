---
title: "Libra深度剖析② Validator 如何验证交易"
date: 2019-07-01T01:01:23+08:00
draft: false
tags: ["Ethereum", "Libra","Smart contract","virtual machine"]
categories: ["libra"]
---

在上一篇文章中，我们[初步探索了 Libra & Move 语言](https://blog.secondstate.io/post/20190621-libra-first-impressions-zh/) 。在这篇文章中，我们将探讨使用者如何跟 Libra 进行互动。本文以向 Libra 水龙头(Faucet)索取 Libra 币为例子，解释 Libra Client 与 Validator 的内部是如何处理与执行交易的。

　　
### 与 Libra 互动

Libra 技术白皮书用下面这张图来解释 Libra Client 与 Validator 之间的关系：

![](/images/20190701-Libra-first-impression2-01.png)

Libra 将参与区块链的人分成两类：Client(普通的使用者) 与 Validator(LibraBFT 共识节点)。

普通的使用者通过 Client 向 Validator 提交交易与 query。而 Validators 的任务就是处理好共识与执行 Client 发出的请求。

### 使用者向 Faucet 索取 LibraCoin
1. 启动 Libra CLI (Libra command line interface)以后，等待 Libra CLI 连上 测试网(testnet) ，并提示使用者可以进行输入`libra%` 

2. 使用者输入`account mint <Address> <Number of Coins> `后， Libra CLI 首先会将 <Address>, <Number of Coins> 打包成交易中的参数数据(parameter data),并从`language/stdlib/transaction_scripts`下载入 `mint` 程式的 Move IR 档案，作为交易中的交易脚本。(关于交易格式请参考文末的参考资料)。

3. 打包好一笔交易以后，Libra CLI 便会将这笔交易提交给 Validator。

4. Validator 收到这笔交易时，并不会立刻执行它，而是先将其放入 mempool 里面，并与其他的 Validator 分享。

5. 当 Validator 要执行这笔交易时，每位 Validator 将轮流成为领导者(Leader)，领导者会从 mempool 里面挑选等待执行的交易做成一个拟议区块(proposed block)，并广播给其他的 validators，等待投票。在至少 2f+1 个 validator 投票同意拟议区块，领导者将会制作法定人数证书( quorum certificate )并广播给全部的 validators。

6. 经过这个过程，在这个拟议区块中的交易就会被执行，并且提交到版本数据库(versioned database)。

### Validator 是如何执行交易的？
Libra 专门为 Move 语言设计了 Move 虚拟机。当 Validator 要执行交易时，需要通过 Move 虚拟机来执行。

![](/images/20190701-Libra-first-impression2-02.png)

1. 检查签名: 检查交易签名是否跟交易数据以及发送者的公钥吻合。此阶段是直接比对交易的资料，尚未跟版本数据库与 Move 虚拟机互动。

2. 运行预处理程序(Prologue):

依序会进行以下三个检查：a）检查交易中的交易发起者公钥是否与版本数据库中交易发起者账户中留存的验证密钥一致。

`HASH(Sender Public Key) == SenderAddr.LibraAccount.T.AuthenticationKe`

b）检查交易发起者是否有足够的 Libra coin 来支付 gas 费

`Transaction.gasPrice * Transaction.maxGasAmount <= SenderAddr.LibraAccount.T.balance`

c）检查交易的序列号是否跟当前版本数据库中交易发起者账户的序列号一致。

`Transaction.SequenceNumber == SenderAddr.LibraAccount.T.SequenceNumber`

这三个检查都是通过执行 built-in Module `0x0.LibraAccount` 的procedure `prologue()`

因为此阶段为必要的检查，虽然通过 Move 虚拟机执行了`0x0.LibraAccount.prologue()`但并不会向交易发起者收取执行的 gas 费。

3. 验证交易脚本(Transaction Script)和模块(Module)

安全性是 Move 非常重要的设计原则，因此在 Move 提供了字节码验证器来检查即将执行的交易脚本或者部署的模块，以确保 type, reference, resource 的安全。

4. 发布模块：若交易中有需要部署的模块，则在此阶段将模块部署到该地址中。（请参考文末参考资料中的 Libra 存储布局）

5. 运行交易脚本：Move 虚拟机将交易中的参数绑定进交易脚本中的参数(arguments)后，便会开始执行。

若执行成功，将会产生事件(eVENts)并将此次执行结果写回全球状态(global state)中。若执行失败(包含 out of gas、执行错误等)，则会还原本次执行对全球状态的修改。

6. 运行收尾程序(Epilogue)：不论执行成功或失败，都会触发此阶段。

通过呼叫 built-in Module `0x0.LibraAccount` 的procedures `Epilogue()` 来进行收取gas 费和调整序列号的操作：

收取 gas 费

`SenderAddr.LibraAccount.T.Balance -= Transaction.gasPrice * Transaction.gasUse`

调整序列号

`SenderAddr.LibraAccount.T.SequenceNumber = 1`

此阶段跟预处理程序阶段相同，虽然会通过 Move 虚拟机执行`LibraAccount.Epilogue()`，但不会向交易发起者收取运行收尾程序的 gas 费。

### 一个具体的例子

现在我们已经看到Validator如何处理交易，用一个完整的例子来演示如何在testnet上制作或创建一些Libra。该过程从Libra CLI 开始。

当我们启动 Libra CLI 时，会经过下面的流程来初始化 client ，让使用者得以输入指令与 Libra 测试网互动。

在使用者看到`libra%`的输入提示前， Libra CLI 会从配置文件中将 genesis, faucet account, local account 等信息载入，并进行 ClientProxy 的初始化，来连接上测试网。在后续的操作中，使用者的指令将全部通过 ClientProxy 来包装，将使用者的指令与对应的交易合成，并发送给 Libra 的 Validator。

#### LibraCommand

当使用者希望为自己的第一个账户索取 100 个 Libra coin 时，输入`account mint 0 100`，此时 Libra CLI 会解析整个输入字串并调用对应的 LibraCommand 来执行。

指令中的`account` 代表着 Command 要调用`CommandAccount::execute()`来执行`mint 0 100`。此时 CommandAccount又看见指令中的`mint` ,因此会再解析一次，以`0 100` 去调用subcommand `CommandAccountMint::execute()`来执行 。

![](/images/20190701-Libra-first-impression2-04.png)

#### ClientProxy

但`AccountCommandMint::execute()`并不是直接发送交易给 Libra validators，而是通过 Libra CLI 最重要的核心`ClientProxy` 代为操作，下面是具体的流程。

![](/images/20190701-Libra-first-impression2-05.png)

`AccountCommandMint::execute()`会将`0`(第一个账号), `100`(libra coin 的数量) 发送给`ClientProxy::mint_coins()`来执行。

在执行时， ClientProxy 会先确认在本机上的帐号是否存在水龙头（faucet）账号。

a) 若本机不存在水龙头账号，`ClientProxy` 则会调用`mint_coins_with_faucet_service()`，将`mint 0 100`打包成一个链接 (`https://<faucet server>?amount=100&account=0`)，来对远程水龙头服务器发送`mint` 的请求。

b) 当本机存在水龙头账号时，`ClientProxy` 则会采用完全不同的路径来执行`mint`请求，

改为调用`ClientProxy::mint_coins_with_local_faucet_account()`，此时会真的建构一个 Libra 交易：

通过`vm_genesis::encode_mint_program()`方法，取得放置在`language/stdlib/transaction_scripts`下 `mint.mvir`。(`mint.mvir` 是 Libra 团队用 Move 语言预先写好的交易脚本。)

接着调用 `ClientProxy::create_submit_transaction_req()`，将交易脚本 (mint.mvir), sender address, gas price, max gas amount 等信息打包到 Libra 交易。

c) 使用`GRPCClient::submit_transaction()`将这个交易发送给 Validator。

注：如果使用者在调用`account mint 0 100`时，要求等交易完成后才返回，这时`ClientProxy`会调用`wait_for_transaction()`卡住输入区并显示`waiting…` 直到交易完成后才会再次提示使用者可以进行下一步的操作。


### 附：参考资料

**1、交易格式**

Libra 详细定义了一笔交易应该有以下栏位：

![](/images/20190701-Libra-first-impression2-06.png)

Sender Address：交易发起者地址，将用来查询在 Ledger 中此地址的 LibraAccount 的资讯。

Sender Public Key：交易发起者公钥，将用来验证此交易是否由发送者所签署；以及检查此公钥是否与此地址中 LibraAccount 留存的验证密钥相符。

Program Move：模块(Move Module) 或者交易脚本(Transaction Script)。

Gas Price：此交易的单位 Gas 的价格。

Max Gas Amount：此交易的 Gas 上限。

Sequence Number：序列号，需要与当前地址中的`SLibraAccount.T.SequenceNumber`相符，是用来检查是否发生重播攻击(replay attack )等攻击的验证栏位。

**2、Libra 存储布局**

每个地址分别存在模块部分(Module Section)与资源部分(resource section)。

模块部分代表了每个地址可以拥有多种 Libra 模块，唯一的限制是在该地址中只能存在一个同名的 Module。

以下图为例，0x0 已经存在了 Account 模块，当使用者将发布在模块的交易发布到另一个Account 模块到 0x0 时，执行交易会中止并报错。

![](/images/20190701-Libra-first-impression2-07.png)

在 Libra 中，不同地址拥有完全独立的 namespace ，因此在不同的地址部署同一个模块会产生不同的模块名字。比如上图中虽然`0x0` 跟`0x4`都有同名的Account 模块，但实际上在使用时，我们可以看到`0x4` 里面存在着`0x0.Account.T {...}`与`0x4.Account.T {... }` ，他们分别表示完全不同的资源空间。



Second State 主要提供[针对企业的区块链智能合约解决方案](www.secondstate.io)，已获得一线VC投资，目前处于未公开阶段。Second State 正在为业内领先的开源项目做贡献，即将发布第一批产品。
