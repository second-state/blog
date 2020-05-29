
---
title: "Why Rust"
date: 2020-05-29T19:01:23+08:00
draft: false
tags: ["Rust","Why rust","swtich to rust","server side"]
categories: ["en"]
---


The Rust programming language is gaining popularity amongst developers. For 5 years in a row, it is voted as the most beloved programming language in the world. In the most [recent 2020 survey](https://stackoverflow.blog/2020/05/27/2020-stack-overflow-developer-survey-results/)of 65,000 developers, Rust was beloved by a stunning 86.1% of developers. Being loved by developers is a leading indicator that Rust is on the path of mass adoption. It is entering a positive feedback cycle of more developers, more libraries, better tools, larger talent pool, more adoption, and then more developers. Indeed, Rust is already one of the fastest growing programming languages in the world.

![](/images/20200529-rust-01.jpg)

But why? Why do developers love Rust so much even through the early days when its  libraries and tooling are very limited. Here are some use case scenarios where Rust is winning. 


## Large projects that switch to Rust

[Deno](https://deno.land/) is a high performance Typescript and Javascript runtime. It is very much like Node.js. In fact, it is created by [Ryan Dahl](https://github.com/denoland/deno), the creator and original developer of Node.js. Why does Ryan want to essentially re-write Node.js? Well, one big different between Deno and Node is that Deno is written in Rust under the hood. Rust enabled developers like Ryan Dahl to be more productive in his work. 

[Discord switched from Go to Rust](https://blog.discord.com/why-discord-is-switching-from-go-to-rust-a190bbca2b1f) because

1. Memory management in Rust is better than Go. As a real-time chat app, Discord needs to handle high volumes of messages with consistent performance. That is a lot of memory management.
2. Rust has a fast evolving ecosystem. Everything in Rust is being rapidly developed.

[Dropbox rewrote its file sync engine in Rust](https://dropbox.tech/infrastructure/rewriting-the-heart-of-our-sync-engine). Besides the performance advantages of Rust, Rust’s ergonomics and focus on correctness  helped Dropbox improve developer productivity and reduce bugs in this complex piece of software.

[Microsoft is also exploring Rust](https://msrc-blog.microsoft.com/2019/11/07/using-rust-in-windows/). Microsoft needs to eliminate memory-related bugs in software written in languages like C++. Rust is a “memory-safe” language.

 Companies adopted Rust because Rust’s excellent features, such as memory-safe, make their service better.


## Java developers 

[Mike Bursell](https://opensource.com/article/20/5/rust-java), a Java developer, wrote an article to explain why he loves Rust. Bursell has 25 years of programming experience for Java and Perl. His reasons are as follows.

* The abundance of Rust tutorials and books make it easy to get started.
* Rust’s memory management concepts, such as references and ownership, make sense for Java developers.
* Cargo is very helpful. It is like Maven in Java but does more.
* The Rust compiler is amazing. The compiler will provide useful guidance on how to correct errors.

In a nutshell, it is pleasant to switch to Rust from Java. 

## Node.js developers

The [2019 official Rust developer survey](https://blog.rust-lang.org/2020/04/17/Rust-survey-2019.html)shows that most Rust developers are working on web applications. What is it that makes people want to use Rust on the server? Reddit user [**silveredge7**](https://www.reddit.com/user/silveredge7) asked exactly that. Here are his questions and answers we compiled. 

Did your application had any cpu intensive tasks that you decided to go with rust and not any other language ?

A: We need machine learning, cryptography, and image processing workloads today that are CPU intensive. They perform well in Rust. In general, more and more computing tasks will require “native” performance as the CPU speed is not going up anymore. 

Node has libuv so it could very well serve a large number of requests per second. Is rust faster in this regard ?

A: When Rust is used together with Node.js, it is best to let node.js handle the bulk of async I/O. That part of node.js is highly optimized C / native code, and node.js does it well. The best way to use [Rust on the server](https://cloud.secondstate.io/server-side-webassembly/why) is to run Rust code inside node.js as a WebAssembly module. 

I know that Rust programmers love the dev experience it provides but how are the development times when we compare it to something like node?

A: Developer productivity is a big thing. Rust is an emerging language on the upward trajectory. That means more and more libraries, sample code for new algorithms will be written in it. It is the same story all over again with Java vs Perl, Ruby vs Java, and JavaScript (nodejs) vs Ruby and Java. 

## Conclusion

People use Rust due to its high performance, memory safety, productivity, tooling, and most importantly, the vibrant community that keeps making it cool!






