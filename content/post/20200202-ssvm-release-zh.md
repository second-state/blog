---
title: "SSVM 0.7 版本将 WebAssembly 引入公有云"
date: 2021-02-02T06:42:14+08:00
draft: false
tags: ["Cloud-native", "Rust", "WebAssembly", "SSVM"]
categories: ["developer","zh","Second State Functions"]
---


[SSVM (Second State VM)](https://www.secondstate.io/ssvm/)是一个非常受欢迎的 WebAssembly 虚拟机，针对服务端的高性能应用程序进行了优化。凭借先进的 AOT（Ahead of Time 编译器）支持，SSVM 已经成为最快的 WebAssembly 虚拟机之一。最新的 [SSVM 0.7](https://github.com/second-state/SSVM/releases/tag/0.7.3) 版本发布了一系列强大且独一无二的功能，这些功能使得 SSVM 进一步差异化，并巩固了 SSVM 作为领先的云原生应用程序运行时的地位。 

![SSVM 0.7 release](/images/20210201-ssvm-0-7-release.png)

## WebAssembly 提案 

SSVM 支持可选的 WebAssembly 功能和提案。这些提案可能成为 WebAssembly 的正式规范。例如，SSVM 支持 WebAssembly 程序的 [WASI（WebAssembly系统接口）规范](https://github.com/WebAssembly/WASI)，从而可以与主机 Linux 操作系统安全地交互。在 0.7 版中，SSVM 支持以下提案。 

* [Reference Types](https://webassembly.github.io/reference-types/core/)。它允许 WebAssembly 程序与主机应用程序和操作系统交换数据。

* [Bulk memory operations](https://github.com/WebAssembly/bulk-memory-operations/blob/master/proposals/bulk-memory-operations/Overview.md)。 WebAssembly 程序可以更快地访问内存，并且在大容量内存操作中性能更好。

* [SIMD](https://github.com/WebAssembly/simd)（单指令，多个数据）。对于有着多核 CPU 的现代设备，SIMD 允许数据处理程序充分利用 CPU。 SIMD 可以大大提高数据应用程序的性能。


此外，SSVM 团队正在探索 wasi-socket 提案，以支持 WebAssembly 程序中的网络访问。 SSVM 有望成为世界上第一个支持 wasi-socket 的WebAssembly 虚拟机。 


##  WASI 的服务端应用扩展 

SSVM 与其他 WebAssembly 虚拟机的主要区别在于对非标准扩展的支持。 [WASI 规范](https://github.com/WebAssembly/WASI)为开发者提供了一种机制，可以有效、安全地扩展 WebAssembly 虚拟机。 SSVM 团队根据实际场景的需求对 0.7 版本的需求创建了以下类 WASI 的扩展。在不久的将来，SSVM 团队会合作、增强和标准化这些扩展和API。 

* [Tensorflow](https://github.com/second-state/ssvm-tensorflow)。开发者可以使用简单的[Rust API](https://crates.io/crates/ssvm_tensorflow_interface) 编写 Tensorflow 推理函数，然后在 SSVM 中安全地以本机速度运行该函数。[Second State Functions](https://www.secondstate.io/faas/) 支持此功能。

* 其他 AI 框架。除了 Tensorflow 之外，Second State 团队还为 AI 框架（例如 ONNX 和 Tengine ）构建类 WASI 的扩展。

* [存储](https://github.com/second-state/ssvm-storage)。SSVM 存储接口允许 WebAssembly 程序读取和写入一个 KV 数据库。 Second State Functions 支持此功能。

* [命令界面](https://github.com/second-state/ssvm_process_interface)。 SSVM 允许 WebAssembly 函数在主机操作系统中执行本机命令。它支持传递参数、环境变量、STDIN / STDOUT 管道以及用于主机访问的安全策略。Second State Functions 中的 [http_proxy](https://www.secondstate.io/articles/internet-of-functions-http-proxy/) 是在 serverless 函数中使用操作系统命令的示例。

* [以太坊](https://github.com/second-state/ssvm-evmc)。 SSVM Ewasm 扩展支持编译为 WebAssembly 的以太坊智能合约。这是是前沿性的以太坊风格 WebAssembly（Ewasm）实现。

* [Substrate](https://github.com/second-state/substrate-ssvm-node)。[SSVM Pallet](https://github.com/second-state/pallet-ssvm) 允许SSVM在任何基于Substrate 的区块链上充当以太坊智能合约执行引擎。


## 基于能力的安全性 

WASI 和类 WASI 的扩展通过声明性的、基于能力的安全模型来控制对主机系统的访问。这种方法允许 WebAssembly 虚拟机实例由显式规则保护，并且独立于运行虚拟机实例的用户。例如，使用 WASI，SSVM 主机应用程序规定虚拟机可以访问主机操作系统中的哪些文件夹。 

SSVM 增加了[对操作系统本机命令的支持](https://www.secondstate.io/articles/command-process/)。沙盒 WebAssembly 字节码应用程序只能访问它要求并声明的操作系统命令，这点很重要。现在，SSVM 主机应用程序可以将允许虚拟机访问的命令列入白名单。随着生态系统和安全策略的发展，我们预计 SSVM 将比当今基于容器的运行时 (例如Docker) 更加安全，适应性更强。 


## 跨平台 

使用服务端的 WebAssembly 虚拟机，开发者可以在一台机器(例如开发者的笔记本电脑或 CI 机器)上编译应用程序，并部署在各种不同的平台上。公有云和边缘云的标准运行时通常是旧版本的 Linux，在开发中已不再广泛使用。例如，Second State 和腾讯云在 [Severless Tensorflow 函数](https://github.com/second-state/tencent-tensorflow-scf)的合作中，SSVM 及其类 WASI 的 Tensorflow 扩展需要在老旧的 CentOS 7 image 上运行。 


SSVM 是在 Ubuntu 20.04 上开发的，以利用先进的LLVM 功能来实现 AOT 编译器。我们为较旧的 Linux 发行版构建和发布了静态链接的 SSVM 二进制文件。 SSVM 二进制文件的 `manylinux1` 构建可以在2007年发布的 CentOS 5 上运行。SSVM 还将移植到各种服务器、嵌入式操作系统和硬件平台上。 


## 云原生支持 

SSVM 是“云原生” WebAssembly 虚拟机，旨在为应用程序开发者提供云服务。SSVM 0.7 作为高性能和安全的运行时引擎，被集成到云原生应用程序框架中。 


* [Second State Functions](https://www.secondstate.io/faas/) 是公共的 Serverless 即服务（FaaS）。它在 SSVM 中安全地运行计算密集型函数。Second State Functions 服务建立在 [Wasm Joey](https://github.com/second-state/wasm-joey) 这一开源项目上，该项目依赖于 Node.js 实例化并管理 SSVM 实例。

* [腾讯 Serverless 云函数](https://intl.cloud.tencent.com/product/scf)与 Second State 合作，通过 SSVM 支持 [Tensorflow 推理函数](https://www.secondstate.io/articles/wasi-tensorflow/)。开发者可以在几分钟之内开始使用 [Serverless 框架模板](https://github.com/second-state/tencent-tensorflow-scf)，发布一个Serverless AI 推理函数。现在还可以参加[线上黑客马拉松赢取奖品](https://mp.weixin.qq.com/s/ur8l_tIGXOXoW7lf1LTSOg)。

* [YoMo](https://github.com/yomorun/yomo) 是一个开源的 Serverless 流框架，用于构建低延迟边缘计算应用程序，建立在 QUIC 传输协议和功能性反应性编程接口之上。Yomo 利用 [SSVM](https://github.com/yomorun/yomo-flow-ssvm-example) 在边缘设置中执行Serverless 函数。


SSVM 将逐步支持 [OCI (Open Container Initiative)](https://opencontainers.org/)规范，该规范将允许由 Cloud Native Orchestration 工具，如Kubernetes，管理 SSVM 实例。 


## 语言支持 

从理论上讲，通过 LLVM 工具链，WebAssembly 可以支持20多种编程语言。但实际上，只有C、C++、Rust 和 AssemblyScript 获得了顶级的支持。即使这些语言，编译器也需要进行修补，以针对非标准扩展生成正确的 WebAssembly 字节码和 API 调用。例如，尽管 SSVM 支持 SIMD 提案，但当前只有基于 C 的编译器才能生成正确的 SIMD WebAssembly 字节码。 Rust 编译器[仍在修补中](https://github.com/rust-lang/stdarch/pull/874/files)，以实现对 WebAssembly 目标的 SIMD 支持。 

除通用编程语言外，SSVM 还支持针对特定行业的细分编程语言(DSL)。例如，SSVM 的一个常见用例是，SSVM 可以在兼容以太坊的区块链上运行智能合约。 SSVM 已经被 Oasis Network 和 Substrate / Polkadot 社区等头部区块链采用为 Ewasm（以太坊的WebAssembly 虚拟机）。 


* Solidity是编写以太坊智能合约的主要编程语言。由 Second State 开发的 [SOLL](https://github.com/second-state/SOLL) 编译器将 Solidity 程序[编译为可在 SSVM 内运行的 WebAssembly 字节码](https://github.com/second-state/SOLL/blob/master/doc/guides/FeatureGuideForSolidity.md)。

* Fe 是以太坊基金会的一种新的简化智能合约语言。 SOLL编译器与以太坊基金会的 solc 工具一起可以将 [Fe 智能合约编译为用于 可在 SSVM 内运行的的 WebAssembly 字节码](https://github.com/second-state/rust-ssvm/blob/master/examples/fe-readme.md)。


SSVM 0.7 是针对 Web、边缘、区块链和 Serverless 计算进行优化的的一个重要版本。 
