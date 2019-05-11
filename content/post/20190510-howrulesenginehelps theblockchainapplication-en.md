---
title: "How does rules engine help the blockchain adoption"
date: 2019-05-10T15:01:23+08:00
draft: false
tags: ["rules engine","business"]
categories: ["en"]
---

When you want to integrate blockchain into your business but you dont have so much development resources, do you just give up?

Don't worry! Rules engine comes to rescue!

What is a rule engine?

Is a component embedded in the application that separates business decisions from the  code applied.

The core of the rules engine is to obtain knowledge (knowledge)
Apply knowledge to specific data (fact)
Use "production rules" IF <conditions> THEN <actions> Rule expression logic (any logic can be expressed this way)

What is "rule"?
A rule consists of conditions and actions. When all the conditions match, the rule may start Conditions ie LHS (left hand side)
Actions ie RHS (right hand side or consistency)
Rule is in control of the data in the application ( fact )

What is "Rete algorithm"?

Rete is the Latin word fir "net", meaning t"he network". The Rete matching algorithm is an efficient method for matching a large number of pattern sets with a large number of object sets. The network filtering method is used to find all objects and rules that match each mode.

Blockchain Rules Engine - a declarative language for non-programmers

Rules engines are already available in many “non-blockchain” enterprise programming languages and frameworks. For example, the Drools, Jess and the Pega business rules platforms. Rules engines are applied in industries including finance, commerce, travel and government. How does rules engine can be used to exonerate you from blockchain programming? 

Smart contract code is immutable; once deployed the smart contract's logic can not be changed. Hence when deployed, only a finite set of possible conditions and logical outcomes are coded. In addition, the real life senarios is very complitcated and changes with the time. "Complex and inferencing rules could be extremely difficult to program, test, and validate, using standard programming languages like Solidity. The long sequence of highly nested IF / THEN statements is fragile and error-prone. " According to Tim McCallum, Second State's core developer. Therefore, rule engine comes into play.

The rules engine provides modularity, which is important because most business operator and analysts who are familiar with and create the business logic, do not know programming. Modularity allows them to change the logic without needing to write code.  

The rules engine allows non-programmers to write, comprehend and maintain smart contracts. When deployed, the rules engine employs the Rete algorithm; the above-mentioned pattern matching algorithm used to determine the correct order of rule execution.

With integrated rules engine, blockchain smart contract has great potention to enable exsiting and new business models, removing a lot of the friction in technology adoption.  

Ethereum's solidity language is too general purpose. 

Second State's Lity virtual machine is optimized for business computing allowing the blockchain to run business rules engine.