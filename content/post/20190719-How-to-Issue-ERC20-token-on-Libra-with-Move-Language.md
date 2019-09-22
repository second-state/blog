---
title: "Libra First Impressions Part 3: How to Issue ERC20 token on Libra with Move Language"
date: 2019-07-19T01:01:23+08:00
draft: false
tags: ["Ethereum", "Libra","Smart contract","virtual machine"]
categories: ["libra","en"]
---

This article is the third and final part of the "Libra First impressions" series. In the previous two articles, we explored the technical implications of the Libra project and the internal processing and execution of Libra Client and Validator.

* [Libra First Impressions Part 2: Deep dive with a Libra transaction](https://blog.secondstate.io/post/20190701-libra-first-impressions/)
* [Libra First Impressions Part 1: Smart Contracts are not created equal](https://blog.secondstate.io/post/20190619-libra-first-impressions/)


## Write your token module
 
In this article, we will show you how to use Move IR to write a simple token module. For easier understanding, we chose Ethereum's ERC20 token as an example, and we focus on three kinds of core functions in this article: `mint`, `balanceOf`, and `transfer`.
 
Before we start, we need to explain the logic of Libra’s processing of resource. Different from Ethereum with its global state, Libra doesn’t have a centrally stored global resource. Instead, it has resources distributed under different accounts. Therefore, using a storage variable like `address storage owner = 0x..` in Ethereum smart contract should be implemented with different logic. Also, everyone’s token balance is stored in the resource of respective accounts instead of using `mapping(address=>uint256)` to process in a centralized way.

**1. Capability**

Right now, Libra developers’ team’s recommended way of dealing with a global variable is to use a singleton pattern’s module.
 
Therefore, we define the possession of the owner as a resource that can only be published once, such as the following `resource T{}`. And we will have two methods to operate on this T, the `grant()` executed at initializing stage to ensure that the Token Capability is handed over to the owner; the `borrow_sender_capability()` is to check if the operator is authorized as an Owner.

```
module TokenCapability {
    resource T {}
 
    // Grant Token Capability to the owner
    public grant() {}
 
    // Return an immutable reference to the TokenCapability of the sender if
    // it exists. This will only succeed if the transaction sender is the owner.
    public borrow_sender_capability(): &mut R#Self.T {}
}
```

**a) `grant()` function**
 
Before we implement `grant` function, we need to define two roles: The transaction sender calling the function and the actual owner. But regretfully, right now the Move IR doesn’t provide `Self.published_address` to allow us to access this module’s account. Therefore, we can only hardcode the module owner’s address in the source code, like in the following code:

```
// Grant Token Capability to the owner
public grant() {
    let sender: address;
    let owner: address;
    let t: R#Self.T;
 
    sender = get_txn_sender();
    // Assume 0x1234 is the owner address
    owner = 0x1234; 
    // Check if the sender is the owner, 77 is just a magic for debugging information.
    assert(move(sender) == move(owner), 77); 
 
    // Create the TokenCapability.T for the owner.
     t = T{};
     // Transfer TokenCapability.T to the owner.
     move_to_sender<T>(move(t)); 
     return;
}
```

From the above program, we can see that only if the `sender == owner` can the resource T of the owner be had. So we can ensure that the resource T will only be owned by the owner, and other accounts will not be able to obtain resource T.
 
Also, `move_to_sender<structure type>(resource)` is a built-in function provided by Move IR, which represents the transfer of the resource to the sender account.
 
**b) `borrow_sender_capability()` function**

And what kind of check should be done to confirm that the transaction sender has the resource of the owner? We will use another built-in function `borrow_global<structure type>(resource account)` provided by Move IR. `borrow_global` will retrieve a reference related to the resource. If the resource is not held under the account, an exception will be triggered, causing the transaction to fail. If successful, it returns a mutable resource reference.
 
```
// Return an immutable reference to the TokenCapability of the sender if
// it exists. This will only succeed if the transaction sender is the owner.
public borrow_sender_capability(): &mut R#Self.T {
    let sender: address;
    let t_ref: &mut R#Self.T;
 
    sender = get_txn_sender();
    t_ref = borrow_global<T>(move(sender));
 
    return move(t_ref);
}
``` 
 
**2. Token**

We have discussed the authorization managing method. Next, let's talk about the Token Module!

```
module Token {
    import Transaction.TokenCapability;
 
    // Token resource, representing the total balance of an account.
    resource T {
        value: u64,
    }
 
    // Create a new Token.T with a value of 0
    public zero(): R#Self.T {
        return T{value: 0};
    }
 
    // Return the value of a Token
    public value(token_ref: &R#Self.T): u64 {
        return *&move(token_ref).value;
    }
 
    // Publish an initial 0 balance Token for the sender
    public publish() {}
 
    // `mint_to_address` will only be called by the owner.
    // This mints a new Token worth `amount` to the payee.
    public mint_to_address(payee: address, amount: u64) {}
 
    // Mint a new Token worth `value`.
    mint(value: u64, capability: &mut R#TokenCapability.T): R#Self.T {}
 
    // Return the Token balance of `account`.
    public balanceOf(account: address): u64 {}
 
    // Return the Token balance of the transaction sender.
    public balance(): u64 {}
 
    // Deposits the `to_deposit` token into the `payee`'s account
    public deposit(payee: address, to_deposit: R#Self.T) {}
 
    public withdraw(to_withdraw: &mut R#Self.T, amount: u64): R#Self.T {}
 
    // Transfer the token from the sender to the payee.
    public transfer(payee: address, amount: u64) {}
}
```

**a) Token Resource**
 
The entire Token module will be the above structure; we will first define the resource T  {value: u64} of this Token, representing the balance(T.value) of tokens that each account will hold in the future. Two helper functions related to T are also defined: zero() makes a Token. T with zero quantity and value() returns the actual value of the Token.T.

**b) Publish**
 
Like Capability, each account holds its own resource separately and Libra's design logic does not allow additional resources to be added without the consent of this account, unlike in Ethereum, where airdrop can happen with just knowing the address.

Therefore, we need to provide a helper function for our token user to allow our token user to call and create Token.T resource for themselves. This is what Publish does.

```
// Publish an initial 0 balance Token for the sender
public publish() {
    let t: R#Self.T;
    // Create new Token.T with 0 value
    t = Self.zero(); 
    // Transfer Token.T to the sender account
    move_to_sender<T>(move(t)); 
    return;
}
```
 
** c) Minting **
The next step of allocating account with resource Token.T is to mint some token. So let’s look at how the `mint` function is implemented.

```
public mint_to_address(payee: address, amount: u64) {
    let capability_ref: &mut R#TokenCapability.T;
    let mint_token: R#Self.T;
    // Use TokenCapability to ensure only owner is authorized to mint token 
    capability_ref = TokenCapability.borrow_sender_capability(); 
    // Call `mint()` function to generate a new resource Token.T.
    mint_token = Self.mint(copy(amount), move(capability_ref)); 
    // Merge the minted token into the name of payee, This function will be explained below.
    Self.deposit(move(payee), move(mint_token)); 
    return;
}
 
mint(amount: u64, capability: &mut R#TokenCapability.T): R#Self.T {
    // We only want to confirm that the sender has TokenCapability.T. Therefore, we release the resource directly.
    release(move(capability));
    // Create a new resource Token.T worths `amount`.
    return T{value: move(amount)};
}
```

In the process of minting tokens, we will ensure that the sender has the right to mint. If not, this transaction will fail. Then create the Token.T to be issued to the payee. Finally, the Token.T newly created by the Token.mint function is merged with the resource Token.T originally under the payee account by Token.deposit function.
 
**d) Balance**

After issuing the token, we don’t yet have a way to query the amount of the tokens. So let's write the balance!

```
public balanceOf(account: address): u64 {
    let token_ref: &mut R#Self.T;
    let token_const_ref: &R#Self.T;
    let token_val: u64;
    // Get a resource reference from the account
    token_ref = borrow_global<T>(move(account)); 
    // Because we don't need to change the value of the resource, we change the mutable reference into immutable.
    token_const_ref = freeze(move(token_ref)); 
    // Get actual value of the balance
    token_val = Self.value(move(token_const_ref)); 
    return move(token_val);
}

// This balance() is a wrapper for balanceOf() , which provides a simple interface for the sender to query.
public balance(): u64 {
    let sender: address;
    let balance_val: u64;
    sender = get_txn_sender();
    balance_val = Self.balanceOf(move(sender));
    return move(balance_val);
}
```

**e) Transfer**

Finally, we came to the most important step: transfer. To implement transfer in Libra, we need to split transfer operation into three steps:
1. Borrow the ownership of resource Token.T from Sender.
2. Split Sender's resource Token.T into two parts to be pulled and the rest. (See the withdraw function for more details)
3. Merge Sender's extracted resource Token.T with Payee's resource Token.T. (See the deposit function for more details)
 
So the entire transfer function is implemented like this:

``` 
public transfer(payee: address, amount: u64) {
    let to_pay: &mut R#Self.T;
    let to_withdraw: R#Self.T;
    let sender: address;

    sender = get_txn_sender();
    // Borrow sender’s resource Token.T
    to_pay = borrow_global<T>(move(sender)); 
    // Split sender's balance into two parts: Token.T {value: amount} and Token.T {value: origin balance - amount} 
    to_withdraw = Self.withdraw(move(to_pay), move(amount)); 
    // Merge Token.T {value: amount} with Payee's own resource Token.T.
    Self.deposit(move(payee), move(to_withdraw)); 
    return;
}
```

`withdraw()` and `deposit()` are implemented as follows:

```
public deposit(payee: address, to_deposit: R#Self.T) {
    let deposit_value: u64;
    let payee_token_ref: &mut R#Self.T;
    let payee_token_const_ref: &R#Self.T;
    let payee_token_value: u64;
 
    // Withdraw the value to be merged
    T{ value: deposit_value } = move(to_deposit);
 
    // Get the payee’s Token.T reference and the current value
    payee_token_ref = borrow_global<T>(move(payee));
    payee_token_const_ref = freeze(copy(payee_token_ref));
    payee_token_value = Self.value(move(payee_token_const_ref));
 
    // Modify the payee’s Token.T value
    *(&mut move(payee_token_ref).value) = move(payee_token_value) + move(deposit_value);
    return;
}
 
public withdraw(to_withdraw: &mut R#Self.T, amount: u64): R#Self.T {
    let value: u64;
    // Get the sender's balance and confirm if it is enough to pay for this split
    value = *(&mut copy(to_withdraw).value);
    assert(copy(value) >= copy(amount), 10);
    // Modify the sender's Token.T to update its balance
    *(&mut move(to_withdraw).value) = move(value) - copy(amount);
    return T{value: move(amount)};
}
```

**3. Test our module**

A mvir file will contain two sections: module, and script. In the module section, we will write all the modules we want to deploy in this transaction, and the script is the program we want to execute in this transaction.

**a)Test Script**
 
In our example, we will test by using the section of the transaction script. In this test, we use sender as the owner and mint 1314 tokens to the sender and check if the sender's balance is consistent with our mint's value 1314.

```
script:
import Transaction.TokenCapability;
import Transaction.Token;
 
main() {
    let sender: address;
    let balance_val: u64;
    let sender_balance: u64;
 
    sender = get_txn_sender();
 
    // Grant owner's capability for the sender
    TokenCapability.grant();
 
    // Publish a Token account to the sender. This makes the sender be the owner.
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

**b) Test modules**

After writing the complete modules and scripts, the easiest way for testing our modules is following the Libra team's suggestion: put the mvir file under `language/functional_tests/tests/testsuite/modules/` and execute `cargo test -p functional_tests <file name>`, Libra will load the contract we just wrote and list the results on the screen.

 
As shown below:

![](/images/20190719-How-to-Issue-ERC20-token-on-Libra-with-Move-Language-01.jpeg)

## Compile and deploy to local testnet
 
Because Libra testnet is not yet open to be deployed with modules directly, this can only be explored by setting up one’s own local testnet. The tools deployed today are still immature, and not very developer-friendly. The following is the deployment process we sorted out.
 
1. After compiling Libra, you can find two tools under the `targe/debug/` folder: compiler and transaciton_builder.
2. We need to compile the mvir into a program through the compiler. Command: `./target/debug/compiler -o <output_file_name> <input.mvir>
3. Then package the sender, program, argument, etc. into a raw transaction via transaction_builder. Command: `./target/debug/transaction_builder <sender_address> <sequence_number> <path_to_program_file> <output_transaction_file_name> --args [<Arguments>]`
4. Back to libra cli and use `submit <sender_address/sender_account_ref> <path_to_transaction_file>` to send a transaction to Libra cli.

Use scenario:
We also wrote several transaction scripts for Token operations, please refer to this link: https://github.com/second-state/libra-research/tree/master/examples/ERC20Token/transaction_scripts

**The process of Token deployment**

1. First of all, you need to deploy token.mvir (contains two modules: Token, TokenCapability) to Libra.
2. Before using Token, users should call init.mvir to publish Token.T to their account resource.
3. The owner can mint tokens to other accounts via mint.mvir.
4. Two accounts with resource Token.T can transfer tokens via transfer.mvir.

**Enable publishing modules to local testnet**

By default, Libra disables the module deployment. And libra will read this property from the genesis file at compilation time. In order to deploy modules to local testnet, we have to modify this setting before compiling.
 
In the `language/vm/vm_genesis/genesis/vm_config.toml` file, modify the `type=Locked` of `[publishing_options]` to `type=Open`. If you've compiled libra, don't forget to recompile after the change to enable the settings.
 
Reference Link:
1. [Singleton Pattern Capability](https://community.libra.org/t/any-notion-of-global-variables/355/2)
2. [TokenCapability](https://github.com/second-state/libra-research/blob/master/examples/ERC20Token/token.mvir#L3)
3. [Token Source Code](https://github.com/second-state/libra-research/tree/master/examples/ERC20Token)





