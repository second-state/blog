---
title: "Second State releases scalable privacy service at Mozilla Open Labs"
date: 2020-06-03T10:01:23+08:00
draft: false
tags: ["Privacy-first","serverless","WebAssembly","Rust"]
categories: ["en"]
---





Building a scalable and privacy-first Internet on [Second State’s serverless infrastructure](https://cloud.secondstate.io/). Get the [email newsletter](webassembly.today) on Rust, WebAssembly, serverless, blockchain, and AI.

## Internet privacy is broken

The Public Key Infrastructure (PKI), invented over 40 years ago, has been the bed rock for security and privacy on the Internet. While PKI algorithms are behind the most internet security  protocols, such as HTTPS and TLS, the idea for individuals to use public keys to exchange data, (eg, PGP), was not adopted in large scale.

Traditional PKI is not scalable. It is a O(n\*m) complexity problem for an individual to encrypt and send each of her files (n) using the public key from each of the recipients (m). Centralized file sharing services, such as Dropbox, reduced the problem complexity to O(n+m) as the individual only needs to upload each file once and to manage her contacts list one person at a time. The complexity could be further reduced to O(m) as the centralized service automates file uploading. The centralized model has proven scalable but also brings significant privacy implications. The service at the center “sees” all data and can be hacked even if they do not do evil themselves.


> Privacy over profit. — Mozilla Foundation


The centralization of Internet and the evasion of privacy is one of the reasons Mozilla called for [“fix-the-Internet”](https://builders.mozilla.community/index.html). Second State is one of the teams that responded to the call and joined the Open Labs to develop a prototype product for a privacy-first Internet.

## A new hope

In recent years, the proxy re-encryption scheme has emerged as a way to reduce the complexity of privacy-first file sharing to N+M. The idea, known as [Orthogonal Access Control](https://dl.acm.org/doi/10.1145/3201595.3201602), is for the individual to encrypt each file once and store the encrypted data on a server. Then, only approved recipients have the ability to download and decrypt the data. A recipient can be added to or removed from the access list at any time. 

There can be one or more server-side repositories facilitating the sharing, but none of the servers can decrypt the data. In fact, the file repositories can be open and anonymous and yet still private to people who holds the appropriate private keys.

[Second State](https://www.secondstate.io/) has developed a suite of open source tools and runtimes for the cloud native Internet. Second State tools enable developers to write [fast, safe, portable, and serverless functions](https://cloud.secondstate.io/server-side-webassembly/getting-started)that can be deployed as web services. In Mozilla Open Labs, the team sets out to build web services that streamline and simplify developer adoption of proxy re-encryption in building privacy-first applications. 

## A solution

As part of its Mozilla Open Labs deliverable, Second State has released a developer preview of its [recrypt-as-a-service](https://github.com/second-state/recrypt-as-a-service) open source software. The software enables developers to create public key management services on the Internet. Here is a typical user story for using this service to share private data. 


* Each individual (Alice, Bob, and Charlie etc) creates an identity on the service via a `create_identity` request.
* Alice can grant Bob access to all her data via a `grant_access` request.
* When Alice creates a confidential document, she creates a new AES encryption key to encrypt it. She generates the AES key via a `create_sym_key` request.
* Alice encrypts and publishes the encrypted document on any public web server.
* When Bob wants to decrypt the document, he asks for Alice's AES key via a `get_sym_key`request.


The Second State is based on [IronCore Labs](https://ironcorelabs.com/)’ open source proxy re-encryption implementation library. This library is written in the high performance Rust programming language. Second State provides a [WebAssembly-based runtime](https://cloud.secondstate.io/server-side-webassembly/why) to provide this library as a safe and scalable Internet service. [Learn more](https://cloud.secondstate.io/server-side-webassembly/getting-started) about Second State’s software platform.

## What’s next

In the era of COVID-19, online privacy is more important than ever. 

* Telemedicine solutions are increasingly used to avoid infections from hospitals visits. More than ever, we need to share personal medical records with multiple members of the care team in a secure and private manner. 
* As societies re-open, data surveillance efforts such as immunization passports and contact tracing are increasingly used to ensure public safety. It is paramount that we do not give central data repositories, such as governments or big corporations under government contracts, the ability to infringe on our privacy. 

Hence, the next phase of Second State’s work in Mozilla Open Labs is to build prototype user interfaces for privacy-first exchange of personal medical information. 

Software developers can now create their own [recrypt-as-a-service](https://github.com/second-state/recrypt-as-a-service). Start building your privacy-first apps today!












