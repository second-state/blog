---
title: "Libra深度剖析③ 如何基于 ERC20 标准在 Libra 上发布金融资产"
date: 2019-07-19T01:01:23+08:00
draft: false
tags: ["Ethereum", "Libra","Smart contract","virtual machine"]
categories: ["libra"]
---

本文是 「Libra 深度剖析」系列文章的第 3 篇，也是最后一篇。在之前的两篇文章，我们分别探讨了 Libra 项目的技术意义以及 Libra Client 与 Validator 内部处理与执行交易。

* Part1：[Libra深度剖析①:智能合约并非生而平等](https://blog.secondstate.io/post/20190621-libra-first-impressions-zh/)

* Part2：[Libra深度剖析② Validator 如何验证交易](https://blog.secondstate.io/post/20190701-libra-first-impressions-zh/)

Libra 作为法币稳定币，成为一个金融系统还需要具有债券股票以及各种证券资产与衍生品，本文将从技术角度入手，使用 ERC20 标准在 Libra 区块链上发布金融资产。主要分为以下两部分：

* 编写 token moudule
* 编译、部署 token moudule

希望这个教程可以让你对 Libra 的技术细节有更深刻的了解。


## 编写 token moudule

如何使用 Move IR 编写一个简单的 token module？

为方便理解，我们选择 Ethereum 的 ERC20 token 作为范例，分别执行 mint、balanceOf 和 transfer 三个功能。

开始前，我们需要了解 Libra 与以太坊在处理 resource 的逻辑方面有什么不同。

与 Ethereum global state 不同的是，Libra 并不设置统一集中存储的 global resource，而是将 resource 分散在各个账户存放。因此，以太坊智能合约撰写  "address storage owner = 0x" 这类变量需要用不同的逻辑来实现。每个人拥有多少 token 也分别存放在各自账户的 resource 下，而不是采用 "mapping(address=>uint256) " 这样的统一存储方式处理。

**1、Capability**

目前 Libra 开发团队推荐的处理 global variable 的方式是使用一个 singleton pattern 的 module 来进行模拟。

因此，我们将事件拥有所有者的 (owner) 权限定义为一种只能被发布一次的 resource，比如 "resource T{}"。针对这个 T 进行的操作有两个方法，一是在初始化时执行 "grant()" 用来确保 Token Capability 被移交给所有者；而 "borrow_sender_capability()" 则是检查操作者是否拥有所有者的权限。

```
module TokenCapability {
    resource T {}
    // Grant Token Capability to the owner
    public grant() {}
    // Return an immutable reference to the TokenCapability of the sender if it exists. This will only succeed if the transaction sender is the owner.
    public borrow_sender_capability(): &mut R#Self.T {}
7}
```

**a) grant()**

如何执行 "grant()" ？

首先，我们需要定义两个角色，调用这个函数的交易发起者与实际上的所有者 owner。很可惜的是目前 Move IR 并没有提供类似 "Self.published_address" 的方式来让我们获得发布该 module 的账户，因此我们只能在代码中写死 module 所有者的地址，代码如下：

```
 // Grant Token Capability to the owner
 public grant() {
     let sender: address;
     let owner: address;
     let t: R#Self.T;
 
     sender = get_txn_sender();
 
     owner = 0x1234;
    // 假设 0x1234 是所有者的地址
    assert(move(sender) == move(owner), 77);
    // 检查交易发起者是否为所有者

    t = T{};
    // 为所有者建立新的 Token Capability
    move_to_sender<T>(move(t));
    // 将 TokenCapability.T 转移给所有者。
    return;
}
```

从上面的代码中我们可以发现，只有通过 "sender == owner" 检查才能取得所有者的 resource T，因此我们可以确保 resource T 只会被所有者所拥有，其他的账户都没有机会获得这个 resource T。

此外，"move_to_sender<structure type>(resource)" 是 Move IR 提供的内建函数，它代表了将 resource 移交给交易发起者的账户。

**b) Borrow_sender_capability()**

如何检查并确认交易发起者拥有所有者的 resource？

借助 Move IR 提供的辅助函数 "borrow_global<structure type>(resource account)" 处理。borrow_global 会去该账户下面调取出 resource，如果该账户下没有持有这个 resource 则会触发意外情况，交易也会失败。若成功则会返回可变的 resource reference。

```
 // 如果存在，则返回对交易发起者的 TokenCapability 的不可变引用。
 // 只有在交易发起者是所有者才会执行成功。
 public borrow_sender_capability(): &mut R#Self.T {
     let sender: address;
     let t_ref: &mut R#Self.T;
 
     sender = get_txn_sender();
     t_ref = borrow_global<T>(move(sender));
 
    return move(t_ref);
 }
```
 
**2、Token**

Libra 的权限管理方式比较特别，上文已着重介绍，接下来撰写 Token module！

```
 module Token {
 
     import Transaction.TokenCapability;
 
  // Token resource, 代表一个账户的总余额.
     resource T {
         value: u64,
     }
 
    // 建立一个新的 Token.T ， value值为0
    public zero(): R#Self.T {
    return T{value: 0};
    }

    // 返回 Token的值
    public value(token_ref: &R#Self.T): u64 {
    return *&move(token_ref).value;
    }

    // 为交易发起者发布初始余额为0的Token
    public publish() {}

    // `mint_to_address` 只能由所有者调用.
    // 这会给收款人一个新的Token ,价值是amount 
    public mint_to_address(payee: address, amount: u64) {}

    // Mint 一个新的Token，价值是`value`.
    mint(value: u64, capability: &mut R#TokenCapability.T): R#Self.T {}
    // 返回Token 余额 `account`.
    public balanceOf(account: address): u64 {}

    // 返回交易发起者的Token 余额.
    public balance(): u64 {}

    // 将 `to_deposit` 的token 存入 the `payee`\'s 账户
    public deposit(payee: address, to_deposit: R#Self.T) {}
    public withdraw(to_withdraw: &mut R#Self.T, amount: u64): R#Self.T {}
    // 将Token从交易发起者转到收款人
    public transfer(payee: address, amount: u64) {}
}
```

**a) Token Resource**

整个 Token module 的结构如上。定义这个 Token 的 resource T {value: u64} 代表了未来每个账户将会持有多少数量 (T.value) 的 token，也要定义两个跟 T 相关的辅助函数 zero() 制作一个数量为零的 Token.T，value() 回传该 Token.T 的实际数值。

**b) Publish**

如同 Capability 一样，每个账户都是分别持有自己的 resource。Libra 的设计逻辑中并不允许在没经过某账户的同意下为其增加额外的 resource，不像以太坊中只要有地址就可以收到别人的转账。因此，我们需要一个辅助函数供 Token 的所有者调用，为他们建立 Token.T 的 resource。这是 Publish 负责的事情。

```
// 为交易发起者 publish 一个初始余额为0的 token

     public publish() {
        let t: R#Self.T;
        // 建立一个新的数值为0的 Token.T
        t = Self.zero(); 
        // 将 Token.T 转移到交易发起者的账户下
        move_to_sender<T>(move(t)); 
        return;
}
```

**c) Minting**

让账户拥有 resource Token.T 的下一步便是发送一些 token，因此接下来将具体解释 mint 功能如何实现！

```
 public mint_to_address(payee: address, amount: u64) {
    let capability_ref: &mut R#TokenCapability.T;
    let mint_token: R#Self.T;

    // 使用 TokenCapability 来确保只有所有者有权限可以增发 token
    capability_ref = TokenCapability.borrow_sender_capability(); 

    // 呼叫下方的 mint() 来建立数量为 amount 的 Token.T
    mint_token = Self.mint(copy(amount), move(capability_ref)); 

    // 将 mint 出来的 Token.T 合并到收款人的名下，这个函数我们在下面解释。
    Self.deposit(move(payee), move(mint_token)); 
    return;
}

mint(amount: u64, capability: &mut R#TokenCapability.T): R#Self.T {
    // 为确保只有交易发起者拥有 TokenCapability.T，直接发布 resource 即可。
    release(move(capability)); 

    // 建立一个有 amount 数量的 Token.T 
    return T{value: move(amount)}; 
}
```

增发 token 时，我们应先确保 sender 有增发的权限，如果没有这个权限，transaction 便会失效；然后建立要增发给 payee 的Token.T，最后通过 Token.deposit  函数将新建的 Token.T 与 payee account 下的 resource Token.T 合并。

**d) Balance**

增发 token 后，还缺乏查询名下 Token 数量的方法，这就需要撰写 balance 了！

```
 public balanceOf(account: address): u64 { 
     let token_ref: &mut R#Self.T;
     let token_const_ref: &R#Self.T;
     let token_val: u64;

     // 从该账户下取得 resource reference
     token_ref = borrow_global<T>(move(account)); 
 
     // 因为我们没有计划改动 resource 的数值，因此把可变的 reference 冻结，改成不可变的 reference
    token_const_ref = freeze(move(token_ref)); 

    // 调用 value() 来取得实际的余额
    token_val = Self.value(move(token_const_ref)); 
    return move(token_val);
}


// 这个 balance() 是直接包装 balanceOf() ，提供交易发起者一个简单的接口可以查询。
public balance(): u64 {
    let sender: address;
    let balance_val: u64;
    sender = get_txn_sender();
    balance_val = Self.balanceOf(move(sender));
    return move(balance_val);
}
```

**e) 转账 Transfer**

重头戏当然是转账，transfer 一共分为三个步骤：
* 从交易发起者借用 resource Token.T；
* 将交易发起者的 resource Token.T 分割成要转账的部分与余额 (由 withdraw 函数负责)；
* 将交易发起者转账的部分与付款人的 resource Token.T 合并 (deposit 函数负责)。

因此整个 transfer 函数如下：

```
 public transfer(payee: address, amount: u64) {
     let to_pay: &mut R#Self.T;
     let sender: address;
     let to_withdraw: R#Self.T;
 
     sender = get_txn_sender();
 
     // 借用交易发起者的 resource Token.T
     to_pay = borrow_global<T>(move(sender)); 

     // 分割出要给收款人的部分
     to_withdraw = Self.withdraw(move(to_pay), move(amount)); 

     // 将要给收款人的部分与收款人账户下原有的 Token.T 合并
     Self.deposit(move(payee), move(to_withdraw)); 

    return;
}
```

而 Withdraw 与 Deposit 实现如下：

```
 public deposit(payee: address, to_deposit: R#Self.T) {
    let deposit_value: u64;
    let payee_token_ref: &mut R#Self.T;
    let payee_token_const_ref: &R#Self.T;
    let payee_token_value: u64;

    // 取出要合并的数值
    T{ value: deposit_value } = move(to_deposit);

    // 获得付款人的 Token.T reference 与现有的数值
    payee_token_ref = borrow_global<T>(move(payee));
    payee_token_const_ref = freeze(copy(payee_token_ref));
    payee_token_value = Self.value(move(payee_token_const_ref));

    // 修改付款人的 Token.T 的数值
    *(&mut move(payee_token_ref).value) = move(payee_token_value) + 
    move(deposit_value);
    return;
}

public withdraw(to_withdraw: &mut R#Self.T, amount: u64): R#Self.T {
    let value: u64;

    // 取得交易发起者的 Token.T 数量，并确认是否足够支付这次转账
    value = *(&mut copy(to_withdraw).value);
    assert(copy(value) >= copy(amount), 10);

    // 修改交易发起者的 Token.T 数量，并将分割后的 Token.T 转出去
    *(&mut move(to_withdraw).value) = move(value) - copy(amount);
    return T{value: move(amount)};
}
```

**3、测试 module**

一个 mvir 的档案含有两个区块，分别是 modules 与 script，在 modules 中会撰写交易中需要部署的所有 modules，script 是在这次交易中我们想执行的程序。

**a) Test Script**

在我们的范例中，通过使用交易 script 的区块来进行测试。在这个测试中，我们把交易发起者作为所有者，并且 mint 1314 个 token 给交易发起者，最后检查交易发起者的余额是否跟 mint 的数值：1314 一致。

```
script:
import Transaction.TokenCapability;
import Transaction.Token;
main() {
    let sender: address;
    let balance_val: u64;
    let sender_balance: u64;
    sender = get_txn_sender();
 
    // Grant owner\'s capability
    TokenCapability.grant();

    // Publish an Token account
    Token.publish();

    // Mint 1314 tokens to the owner
    Token.mint_to_address(copy(sender), 1314);

    // Check balance == 1314
    balance_val = Token.balanceOf(copy(sender));
    assert(copy(balance_val) == 1314, 2);
    sender_balance = Token.balance();
    assert(move(sender_balance) == move(balance_val), 1);
    return;
}
```

**b) 测试**

在撰写完 modules 与 script 后，依据 Libra 团队的建议，需将档案放入 "language/functional_tests/tests/testsuite/modules/" 下，并执行 "cargo test -p functional_tests <file name>"，Libra 就会加载并将执行刚才所撰写的合约，结果如下图：

![](/images/20190719-How-to-Issue-ERC20-token-on-Libra-with-Move-Language-01.jpeg)


## 编译、部署到 local testnet

如今 Libra testnet 尚未开放直接部署 modules，只能通过建立自己的 local testnet 来进行测试。现在部署的工具还在非常早期的阶段，对开发者的使用上也不是十分友好，以下是整理后的部署流程。

1. 编译 Libra 后，可以在 "targe/debug/" 资料夹下找到 compiler 和 transaciton_builder 两个工具；
2. 通过使用 compiler 将撰写的 mvir 编译成 program，"./target/debug/compiler -o <output_file_name> <input.mvir>"；
3. 通过 transaction_builder 把 sender, program、argument 等封装成 raw transaction，"./target/debug/transaction_builder <sender_address> <sequence_number> <path_to_program_file> <output_transaction_file_name> --args [<Arguments>]"；
4. 最后进到 libra cli 中使用submit <sender_address/sender_account_ref> <path_to_transaction_file>对 Libra cli 发出交易。
 
注：我们也编写了几笔交易的 scripts 用来操作 Token
[请参考此链接](https://github.com/second-state/libra-research/tree/master/examples/ERC20Token/transaction_scripts)：
https://github.com/second-state/libra-research/tree/master/examples/ERC20Token/transaction_scripts

**部署与使用 Token 的顺序**

1. 先将 token.mvir (含有 Token、TokenCapability 的 module) 部署到 Libra；
2. 要想使用该 token 账户，必须先调用 init.mvir 将 Token.T 发布到账户的 resource 中；
3. 所有者可通过 mint.mvir 给其他拥有 resource Token.T 的账户增发 token；
4. 两个拥有 resource Token.T 的 account 可以通过 transfer.mvir 进行 token 转移。


## 开启允许部署 modules 的权限

Libra 在编译时期 (compilation-time) 时从 genesis file 里面读取是否可以设定部署 modules 的权限。因此，为把 modules 部署到 local testnet，我们需要在编译前修改这项设定。

在 "language/vm/vm_genesis/genesis/vm_config.toml" 这个档案，只需将 "[publishing_options]" 中的 "type=Locked" 改为 "type=Open" 即可。更改完以后，一定要重新编译使设定生效。

参考链接:
1. [Singleton Pattern Capability](https://community.libra.org/t/any-notion-of-global-variables/355/2)
2. [TokenCapability](https://github.com/second-state/libra-research/blob/master/examples/ERC20Token/token.mvir#L3)
3. [Token Source Code](https://github.com/second-state/libra-research/tree/master/examples/ERC20Token)
