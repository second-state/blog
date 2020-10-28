---
title: "SSVM 0.7.0：朝更具互操作性的 WebAssembly 迈进"
date: 2020-10-28T09:42:14+08:00
draft: false
tags: ["WebAssembly", "Rust", "SSVM", "WASI", "Interface types", "Reference types"]
categories: ["developer","zh","WebAssembly"]
---


*Second State 虚拟机 0.7.0 版本支持 reference types 和 bulk memory operations *

Second State 虚拟机（[SSVM](https://www.secondstate.io/ssvm/)）是针对服务器端应用优化的开源 WebAssembly 虚拟机实现。


* SSVM 是当今市场上[最快的  WebAssembly 虚拟机](https://www.secondstate.io/articles/ssvm-performance/)之一，部分原因在于其 AOT（提前）编译器的优化。请参考将在 IEEE Software 上发表的的[性能基准](https://arxiv.org/abs/2010.07115)文件。
* 它提供与 population Web 服务框架的无缝集成，例如[Node.js](https://www.secondstate.io/articles/getting-started-with-rust-function/) 和[腾讯云 Serverless](https://github.com/second-state/ssvm-tencent-starter)。
* 它完全支持 WebAssembly 系统接口（[WASI](https://www.secondstate.io/articles/wasi-access-system-resources/)），用于在服务器上进行操作系统访问。
* 它支持细颗粒度和实时按资源计费。
* 它支持非标准主机函数，以访问存储数据库、以太坊区块链和本机操作系统命令。查看有关如何[访问互联网](https://www.secondstate.io/articles/internet-of-functions-http-proxy/)或运行 TensorFlow 推理以进行[面部识别](https://www.secondstate.io/articles/face-detection-ai-as-a-service/)的示例。


现在，随着 [SSVM 0.7.0 版本](https://github.com/second-state/SSVM/releases/tag/0.7.0)发布，SSVM 支持与服务器端应用用例相关的主流 WebAssembly 草案规范。具体而言， SSVM 0.7.0 支持 [reference types](https://github.com/WebAssembly/reference-types/blob/master/proposals/reference-types/Overview.md) 和 [bulk memory operations](https://github.com/WebAssembly/bulk-memory-operations/blob/master/proposals/bulk-memory-operations/Overview.md) 草拟提案。

bulk memory operations 规范相对简单明了。它将指令（操作码）添加到 WebAssembly 虚拟机，以高效地复制和移动大部分内存数据。这提高了数据密集型应用程序的运行时性能。它还使 WebAssembly 程序更易于与外部应用程序进行交互并共享内存。我们将与 rustc，LLVM 和 [SOLL](https://github.com/second-state/SOLL) 等 WebAssembly 编译器项目合作，以在编译好的 WebAssembly 字节码中生成此类指令。

Reference types 提案更为复杂。本文接下来将详细解释它的含义、重要性，以及我们如何改善当今的 SSVM 开发者体验。

## WASI


最重要的  WebAssembly 扩展规范之一是 WebAssembly 系统接口（WASI）。 WASI 为 WebAssembly 字节码程序提供了一种调用操作系统标准库中的函数的方法。这使 WebAssembly 程序可以访问文件系统、环境变量、随机数和其他重要函数。对于将 WebAssembly 用作浏览器沙箱外部的独立应用程序，访问操作系统服务至关重要。


> 如果在2008年已经有了 WASM + WASI，我们根本不需要创建 Docker。 — Docker 联合创始人 Solomon Hykes


但是，为什么止步于使用操作系统标准库呢？为什么不让 WebAssembly 程序访问基础主机系统提供的所有库函数呢？

## 主机函数


在许多应用场景中，不同的 WebAssembly 虚拟机实现已经可以访问其主机环境提供的各种服务。预先定义过、能够访问主机环境的 WebAssembly 函数称为“主机函数”。


* 在 JavaScript 环境中运行 WebAssembly 的 V8 引擎为 WebAssembly 字节码提供调用 JavaScript 函数的方法。浏览器内的 WebAssembly 程序经常这样做，从而与网页 DOM 和 UI 进行交互。
* 以太坊风格的[WebAssembly (Ewasm)](https://github.com/ewasm) 为 WebAssembly 字节码定义主机函数接口，以访问存储在以太坊区块链上的数据，例如用户帐户、余额和区块信息。 这使 WebAssembly 成为以太坊区块链上的智能合约执行引擎。[SSVM 是领先的 Ewasm 实现之一](https://blog.secondstate.io/post/20200416-ewasm-smart-contract-ssvm-en/)。 
* 除了以太坊以外，还有许多选择 WebAssembly 作为执行引擎的区块链项目。这些项目都将区块链数据和函数作为主机函数公开给 WebAssembly。
* SSVM 支持与服务器端用例相关的多种主机函数。例如，它提供了一组主机函数来访问外部键值数据存储区（ RockDB 实例），以及执行操作系统命令的主机函数。 后者非常通用，因为它可以运行任何命令，但是牺牲了可移植性 —— WebAssembly 程序只能在安装了必要命令的主机操作系统上执行。



> WASI 提出的最有趣的想法之一是“基于能力的安全性”。也就是说，你可以在实例化时声明 WebAssembly 虚拟机可以访问哪些操作系统服务。该 WebAssembly 虚拟机的安全规则集可能与运行它的操作系统用户完全不同。我们可以为主机函数调整相同的安全模型。我们可以提供丰富的主机函数选择，但是每个虚拟机实例只能访问它声明和需要的主机函数。


虽然主机函数很有用且受欢迎，但没有统一的创建标准。Reference types 规范将改变这一点。

## Reference types

WebAssembly 规范的简洁性是一大优势。Webassembly 是轻量级的、非常有效，并且可以进行全面的安全性审查。但是，简单的说明也意味着功能有限。默认情况下，WebAssembly 仅支持数字作为输入和输出类型。如果没有复杂的操作来管理 WebAssembly 虚拟机内部以及与虚拟机通信的外部程序中的内存空间，你甚至无法将字符串或数组传递给 WebAssembly 程序。


> 这就是为什么大多数 WebAssembly 的 “hello world”教程根本没有 “hello world” 的原因。取而代之的是执行某种简单的数字运算，例如加法、乘法或生成斐波那契数列。 WebAssembly 中的 “hello world” 字符串输入非常复杂！


 Reference types 提案使 WebAssembly 可以与外部应用程序交换复杂的数据类型。当然，最显而易见的用例是与主机函数交换数据！

## 教程

没有编译器的支持，创建 Reference type 的 demo 非常麻烦。我们写了一个[详细教程](https://github.com/second-state/SSVM/blob/master/doc/externref.md)来介绍创建 WebAssembly 程序的步骤，手把手教你制作其用于 reference type 的字节码，然后用 C 语言创建、编译和注册主机函数以传递这些类型的数据。

尽管几乎所有 WebAssembly 虚拟机项目都致力于支持 reference types ，但在撰写本文时，SSVM 和 wasmtime 是仅有的两个具有完整实现的项目。此处查看[wasmtime demo](https://fitzgeraldnick.com/2020/08/27/reference-types-in-wasmtime.html) 。 主要区别在于 wasmtime 演示了用 Rust 编写的主机函数，而 SSVM 则用 C 编写了主机函数。虽然我们喜欢 Rust，但我们相信 C 为应用程序提供了更多的库选择，例如在 GPU 和自定义硬件上进行AI 推理（inference）。

## 未来已来

Second State 通过尽早实施来积极支持 WebAssembly 标准。但是，像 *bulk memory operations *提案一样，应用开发者必须等待编译器工具链支持，才能充分利用 reference types 。但是，SSVM 的目标是立刻能支持丰富的主机函数。

开发者可以用 Rust、C 甚至 GO 编写本机程序，然后通过命令界面从 SSVM 调用它们。用 Rust 编写的 SSVM 程序可以将类型化的参数或二进制数据传递给命令程序。了解 SSVM 如何实现和调用 TensorFlow 命令进行[人脸识别](https://www.secondstate.io/articles/face-detection-ai-as-a-service/)和 [图片分类](https://www.secondstate.io/articles/image-classification-as-a-service-in-node.js/) 。这种方法让开发者可以很容易上手并使用 SSVM 扩展。

同时，我们正在创建一个主机函数库，以执行 TensorFlow 模型并执行图像和视频操作。这些主机函数直接使用内存段与 WebAssembly 交换数据。与通用命令相比，专门的主机函数性能更高，更加可移植且更易于使用。

一旦 reference types 和 bulk memory operations 提案在编译器支持下完成，我们将移植 SSVM 主机函数，并为社区开发者提供一个为 SSVM 创建和注册主机函数的标准接口！




