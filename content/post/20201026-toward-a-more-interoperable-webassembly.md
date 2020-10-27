---
title: "Toward a more interoperable WebAssembly"
date: 2020-10-26T09:42:14+08:00
draft: false
tags: ["WebAssembly", "Rust", "SSVM", "WASI", "Reference types"]
categories: ["developer","en","WebAssembly"]
---



*Reference types and bulk memory operations support on Second State VM 0.7.0*

The [Second State VM (SSVM)](https://www.secondstate.io/ssvm/) is an open-source WebAssembly virtual machine implementation optimized for server-side applications.

* It is one of the [fastest WebAssembly VM](https://www.secondstate.io/articles/ssvm-performance/) on the market today partly because of AOT (Ahead Of Time) compiler optimization. See our [performance benchmark paper](https://arxiv.org/abs/2010.07115) to be published on [IEEE Software](https://www.computer.org/csdl/magazine/so/5555/01/09214403/1nHNGfu2Ypi).
* It provides seamless integration into population web service frameworks, such as [Node.js](https://www.secondstate.io/articles/getting-started-with-rust-function/) and [TencentCloud Serverless](https://github.com/second-state/ssvm-tencent-starter).
* It fully support the [WebAssembly Systems Interface (WASI)](https://www.secondstate.io/articles/wasi-access-system-resources/) for operating system access on the server.
* It supports fine-grained and real-time resource metering.
* It supports non-standard host functions to access storage database, the Ethereum blockchain, and native operating system commands. See an example on how to [access the Internet](https://www.secondstate.io/articles/internet-of-functions-http-proxy/)or to [run Tensorflow inference for face detection](https://www.secondstate.io/articles/face-detection-ai-as-a-service/) from SSVM functions.


Now, with the [SSVM 0.7.0 release](https://github.com/second-state/SSVM/releases/tag/0.7.0), we are supporting popular WebAssembly draft specifications that are relevant to server-side application use cases. Specially, SSVM 0.7 supports the [reference types](https://github.com/WebAssembly/reference-types/blob/master/proposals/reference-types/Overview.md) and [bulk memory operations](https://github.com/WebAssembly/bulk-memory-operations/blob/master/proposals/bulk-memory-operations/Overview.md) draft proposals. 

The bulk memory operations specification is relatively simple and straightforward. It adds instructions (opcodes) to the WebAssembly VM to perform efficient copying and moving of large sections of memory data. That improves runtime performance in data intensive applications. It also makes it easier for WebAssembly programs to interact and share memory with external applications. We will be working with WebAssembly compiler projects, such as rustc, LLVM, and [SOLL](https://github.com/second-state/SOLL), to generate such instructions in compiled WebAssembly bytecode.

The reference types proposal is more complex. In the rest of this article, I will explain what it is, why it is important, and how we can improve the SSVM developer experience today!

## WASI

One of the most important WebAssembly extension specification is that WebAssembly System Interface (WASI). WASI provides a way for WebAssembly bytecode programs to call functions in the operating system’s standard library. That allows WebAssembly programs to access the file system, environmental variables, random numbers, and other important functionalities. Access to operating system services is crucial for using WebAssembly as a standalone application outside of the browser sandbox.


> If WASM+WASI existed in 2008, we wouldn't have needed to created Docker. — Solomon Hykes, Co-Founder of Docker


However, why stop at operating system standard libraries? Why not allow WebAssembly programs to access any library function the underlying host system provides?

## Host functions

Indeed, in many application scenarios, different WebAssembly VM implementations already provide access to various services it’s host environment provides. Those pre-defined WebAssembly functions which access the host environment are called “host functions”.

* The V8 engine, running WebAssembly in a JavaScript environment, has always provided ways for WebAssembly bytecode to call JavaScript functions. In-browser WebAssembly programs frequently do so to interact with the web page DOM and UI.
* The [Ethereum flavored WebAssembly (Ewasm)](https://github.com/ewasm) defines a host functions interface for the WebAssembly bytecode to access data stored on an Ethereum blockchain, such as user accounts, balances, and block information. That allows WebAssembly to become a smart contract execution engine on the Ethereum blockchain. The [SSVM is one of the leading Ewasm implementations.](https://blog.secondstate.io/post/20200416-ewasm-smart-contract-ssvm-en/)
* Besides Ethereum, there are many other blockchain projects that chose WebAssembly as the execution engine. All of them exposes the blockchain data and functions to WebAssembly as host functions.
* The SSVM supports several host functions that are relevant to the server-side use case. For example, it provides a set of host functions to access external key-value data stores (e.g., a RockDB instance), as well as host functions to execute operating system commands. The latter is extremely versatile as it could run any command, but at the cost of portability — the WebAssembly program can only execute on host operating systems with the necessary commands installed. 



> One of the most interesting ideas WASI promotes is “capability based security”. That is, you could declare which operating system services the WebAssembly VM have access to when you instantiate it. That WebAssembly VM could have an entirely different set of security rules than the operating system user who runs it. We could adapt the same security model for host functions. We can provide a rich selection of host functions, but each VM instance only has access to the host functions it declares and needs.


While host functions are very useful and popular. There is no “standard” to create them. The reference types specification is going to change that.

## Reference types

The simplicity of the WebAssembly specification is one of its great advantages. It is lightweight, very efficient, and can be throughly reviewed for security. However, the simple specification also means limited features. By default, WebAssembly only supports numbers as input and output types. You cannot even pass a string or array to a WebAssembly program without complicated maneuvers to manage memory spaces both inside the WebAssembly VM, and in the external program the VM communicates with.


> That is why most WebAssembly “hello world” tutorials do not do “hello world” at all. They do some kind of simple numeric operations such as addition, multiplication, or generating the fibonacci sequence. Taking the “hello world” string input is complicated in WebAssembly!


The reference types proposal enables WebAssembly to exchange complex data types with external applications. The most obvious use case, of course, is to exchange data with host functions!

## A tutorial

Without compiler support, creating a reference types demo is quite involved. We have created [a comprehensive tutorial](https://github.com/second-state/SSVM/blob/master/doc/externref.md) that walks through the steps to create a WebAssembly program, hand craft its bytecode for reference types, and then create, compile, and register host functions in C to pass data in those types. 

While almost all WebAssembly VM projects have committed to support reference types, SSVM and wasmtime are the only two with complete implementations at the time of this writing. The [wasmtime demo](https://fitzgeraldnick.com/2020/08/27/reference-types-in-wasmtime.html) can be found here. The major difference is that wasmtime demonstrated host functions written in Rust while SSVM has host functions written in C. While we love Rust, we believe C offers a much larger selection of libraries for applications such as AI inference on GPUs and custom hardware.

## The future has arrived

At Second State, we actively support WebAssembly standards by implementing them early. However, like the bulk memory operations proposal, application developers must wait for compiler toolchain support before they can fully take advantage of reference types. But, we are not sitting idle and waiting! Second State is committed to support rich host functions NOW.

Developers can write native programs in Rust, C, or even GO, and call them from SSVM through the command interface. The SSVM program, written in Rust, can pass typed arguments or binary data to the command program. Check out our tutorials on how to implement and call [a tensorflow command for face detection](https://www.secondstate.io/articles/face-detection-ai-as-a-service/) and for [image classification](https://www.secondstate.io/articles/image-classification-as-a-service-in-node.js/). This approach is easy for developers to get started and experiment with SSVM extensions. 

At the same time, we are creating a library of host functions to execute Tensorflow models and perform image and video manipulations. Those host functions are exchanging data with WebAssembly directly using memory segments. Those specialized host functions are more performant, more portable, and easier to use than the generic commands. 

Once the reference types and bulk memory operations proposals are finalized with compiler support, we will port SSVM host functions and provide community developers with a standard interface to create and register host functions for the SSVM!

