---
title: "Smart Contract Meetup 7:  Libra Deep Dive"
date: 2019-08-05T09:42:14+08:00
draft: false
tags: ["blockchain", "developer Community", "Libra"]
categories: ["developer"]
---


* With Libra being scrutinized, if not attacked, at Congressional hearing, where is the anticipated ambitious global stable coin is heading towards?

* What is the difference between the programming on Libra and Ethereum?

* Can ERC 20 token interact with Libra?

* How to get started with DApp programming in a few minutes?

These are discussed in detail at the tech workshop "Libra Smart Contract Deep Dive" held in Beijing, China, on August 3rd.

Dragon Long, a senior engineer with Second State, demoed to 35 developers presented how to publish an ERC-20 smart contract on Libra. As an engineer with 20 years of computer engineering experience and 5 years of blockchain programming expertise, he answered many questions from the audience.

![](/images/20190805-libra-deep-dive-01.JPG)

Dr. Michael Yuan, the founder of a VC funded startup, Second State, [a blockchain solution provider for business](wwww.secondstate.io), spent 15 minutes introduced BUIDL, Second State's freshly launched blockchain IDE. Anyone can [develop their own DApp with BUIDL in less than a minute](https://buidl.secondstate.io/).

![](/images/20190805-libra-deep-dive-03.jpeg)

Libra is the most anticipated technology project of the year, and [its most eye-catching design is its smart contract-first approach](https://blog.secondstate.io/post/20190619-libra-first-impressions/). In Ethereum, comparatively, smart contracts are just a type of transaction.

With Libra, an account consists of modules and resources. Its module is similar to Ethereum's smart contract. Resources are guaranteed by the Move system and can not be copied, reused or discarded. The external module has strict restrictions on the modification of the resources of this module. It can only be "moved" (like moving a chess) and cannot assign values to resources at will.

In comparasion, a variable can be copied on Ethereum, which is equivalent to cloning. The original variable’s value can still be used unchanged; but on Libra, the value is read by move, and after the variable is moved to the new object, the original variable becomes invalid. "This draws on Rust's move grammar: when reading variables, you must choose a value-reading method, either being copy or move." Long explained, "This restrive moveable-only design, compared to Ethereum's solidity, is much more secure in programming when assets is involved.“

For details on how to issue ERC-20 tokens on Libra, you can refer to [the third article](https://blog.secondstate.io/post/20190719-how-to-issue-erc20-token-on-libra-with-move-language/) on Libra s on Second State's blog.


The order in which Tokens are deployed and used on BUIDL:

1. Deploy token.mvir (module with Token, TokenCapability) to Libra
2. To use the token account, you must first call init.mvir to send Token.T to the account's resource;
3. The owner can add tokens to other accounts with resource Token.T via mint.mvir;
4. Two accounts with resource Token.T can transfer tokens via transfer.mvir.

The event also invited Ares, a talk show Ares Talk’s owner, to explain his take on the prospect of Libra after the congressional hearing. He believes it is very unlikely for Libra to be stopped, at least not in the technical respect.

Regarding this, Long also gave his own opinion from a technical point of view, "Judging rom the frequency of Libra’s code update, the technology development is being iterated without stop.“

![](/images/20190805-libra-deep-dive-02.JPG)

### Resources for reference. Come and star Buidl on Github!

1. [Libra workshop slides](https://github.com/CyberMiles/education/blob/master/meetups/beijing/6-libra/Libra%20AresTalk.pdf)

2. [Use BUIDL to write a DApp in a wink](https://buidl.secondstate.io/)

[Documentation](https://docs.secondstate.io/buidl-developer-tool/getting-started)

3.[Libra Deep dive slides](https://github.com/CyberMiles/education/blob/master/meetups/beijing/6-libra/Libra-dragon.pdf)

[The complete code used to initiate a transcation](https://github.com/second-state/libra-research/tree/master/examples/ERC20Token)

4. Libra tech blogs

* [Libra First Impressions Part 1: Smart Contracts are not created equal](https://blog.secondstate.io/post/20190619-libra-first-impressions/)

* [Libra First Impressions Part 2: Deep dive with a Libra transaction](https://blog.secondstate.io/post/20190701-libra-first-impressions/)

* [Libra First Impressions Part 3: How to Issue ERC20 token on Libra with Move Language](https://blog.secondstate.io/post/20190719-how-to-issue-erc20-token-on-libra-with-move-language/)

5.BUIDL github Thank you and please star in Github
[](https://github.com/second-state/buidl)
