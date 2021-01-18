---
title: "Second State Functions releases a developer preview for high performance serverless functions"
date: 2021-01-14T06:42:14+08:00
draft: false
tags: ["Oasis", "confidential smart contract", "Ethereum", "Hackathon"]
categories: ["developer","en","Oasis"]
---


Second State has recently released a developer preview of its Function-as-a-Service (FaaS), known as [Second State Functions](https://www.secondstate.io/faas/). The Second State Functions enables developers to write simple functions in the Rust programming language for computationally intensive tasks such as image processing and AI inference. The developer simply needs to upload the compiled function to the FaaS service, and the function will become publically available as a web service. The developer preview version of the Second State Functions is free to use. Hundreds of developers have already experienced it and deployed functions on it in the past two weeks. Developers could [get started and win some cool prizes!](https://www.secondstate.io/articles/serverless-functions-in-rust-challenge-one/)

Second State Functions uses [SSVM, the Second State WebAssembly VM](https://www.secondstate.io/ssvm/), as the runtime engine for user-uploaded functions. WebAssembly is an emerging application container for server-side and edge applications. It provides runtime isolation, safety, security, and cross platform portability for user applications without sacrificing performance. WebAssembly is a W3C standard supported by major tech companies. It is a lightweight and high performance application sandbox with a capability-based security model. A recent peer reviewed [research paper published on IEEE Software](https://arxiv.org/abs/2010.07115) demonstrated that WebAssembly VMs have significant performance advantages over popular application containers such as Docker. It further demonstrated that SSVM is by far the fastest WebAssembly VM on the market. The SSVM supports the latest WebAssembly specifications such as SIMD, reference types, and bulk memory operations. 

Furthermore, the SSVM implements an array of WebAssembly extensions that allow WebAssembly programs running inside SSVM to access host operating system and hardware features on the server. For example, the SSVM supports the WebAssembly Systems Interface (WASI) to access the file system and standard libraries. The SSVM also implements WASI-like extensions to access tensorflow libraries, a key-value store, HTTP network, as well as operating system commands. 

Developers write Second State Functions in the Rust programming language, which is known as a safe and high performance language. While Rust is a complex and modern programming language, developers can create useful web services on Second State FaaS using only the basic Rust language features. That approach is known as low-code Rust. A series of developer tutorials showcase such uses of the Rust language through the Second State Rust SDK. 


> Rust has been voted as the most beloved programming language 5 years in a row by Stackoverflow users. 


Specifically, the high performance of Rust and SSVM enables compute-intensive serverless functions. For example, Second State Functions has comprehensive tutorials on how to [create Tensorflow functions](https://www.secondstate.io/articles/wasi-tensorflow/) to classify images or detect faces. 

The developer preview of Second State Functions is available to the public. With it, anyone can create and deploy Rust functions as web services. 



