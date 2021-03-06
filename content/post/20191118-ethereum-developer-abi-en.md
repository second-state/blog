---
title: " Ethereum Development A2Z| Smart Contract ABI "
date: 2019-11-19T09:42:14+08:00
draft: true
tags: ["blockchain", "Ethereum", "developer","smart contract","BUIDL"]
categories: ["ethereum","en"]
---
Learn Ethereum development and acquire complex technical concepts in a snap!

Second State Inc is to release Ethereum development A2Z series articles to help you learn Ethereum in a short time starting from the basics, with real blockchain development cases.


![](/images/20191112-abi-01.png)


This article is to introduce Smart Contract ABI, the first piece in the series.

Blockchain Smart Contract ABI is the interface between smart contracts and blockchain applications. After a smart contract is compiled into a binary file by a compiler like [BUIDL](https://buidl.secondstate.io/), an ABI for this smart contract is automatically generated, recording the functions and parameters of the smart contract. When a user uses a blockchain application, the smart contract will be called with this ABI.

## What is an ABI 

ABI is the abbreviation for Application Binary Interface, which is the interface for applications to call binary application files. ABI is a convention that is customary.

Just like we press the buttons on the remote control to  lower/adjust the temperature, we use ABI to control a program’s parameters. The remote control is equivalent to an ABI, which records the functions and parameters, like the air conditioner’s heating/cooling, and the highest and lowest temperature that the air conditioner can achieve. As the functions of the air conditioner expand: the sleep mode, ventilation, etc., the buttons on the remote control (ABI) also grow.

An ABI enables user's interaction with the program.

## ABI in the development of Ethereum

In the Ethereum/Blockchain development, smart contract ABI is very common. After a smart contract is compiled, the compiler automatically generates an ABI to record the functions and parameters of the smart contract. When blockchain applications are used, people use ABIs to call smart contracts to implement related functions.


![](/images/20191112-abi-02.png)

Take the default code in the [BUIDL](https://buidl.secondstate.io/) IDE developed by Second State Inc, a blockchain infrastructure software technology company, as an example. The code of the smart contract is as follows:

```
[{"constant": false,"inputs": [{"name": "x","type": "uint256"}],"name": "set","outputs": [],"payable": false,"stateMutability": "nonpayable","type": "function"},{"constant": true,"inputs": [],"name": "get","outputs": [{"name": "","type": "uint256"}],"payable": false,"stateMutability": "view","type": "function"}]
```
The ABI generated by the smart contract is an array of JSON format, with a parameters name-value pair. When the smart contract is called, it is via ABI we know what certain functions the smart contract implement and what the parameters are.

Note: ABI is not unique. The smart contract implementations with the same functionality can have the same ABI.

When we use [BUIDL](https://buidl.secondstate.io/) for subsequent DApp development work, we find that the JavaScript section records the ABI of the current smart contract, enabling the application to call smart contracts and interact with them.

![](/images/20191112-abi-03.png)

## About [BUIDL IDE](https://www.secondstate.io/buidl/) - An One-Stop Blockchain Development Tool

The BUIDL IDE is a highly integrated and fully browser-based development tool that allows developers to easily create and deploy blockchain applications (DApps) with BUIDL, saving a lot of time by eliminating the need to install any software and blockchain wallets.

BUIDL functions:

* **Built-in account management system, no wallet software required;**
* Write, compile and deploy smart contracts;
* **Development DApps with HTML5 and JavaScript;**
* **One-click publishing of DApps to Github Pages**；
* Deployment on Ethereum and any blockchain compatible with Ethereum;
* Access to the built-in Web3 library；
* **Support of rule-based smart contracts;**
* **Built-in real-time smart contract search engine based on ElasticSearch;**


More Info:

* BUIDL Intro Page
    https://www.secondstate.io/buidl/
* BUIDL IDE
    https://buidl.secondstate.io/
* BUIDL Documentation
    https://docs.secondstate.io/buidl-developer-tool/getting-started
* BUIDL GitHub Repo
    https://github.com/second-state



