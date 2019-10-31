---
title: "Celebrate Halloween with blockchain smart contract"
date: 2019-10-31T10:01:23+08:00
draft: false
tags: ["smart contracts","Halloween"] 
categories: ["en"]
---

Halloween is finally here! Are you ready for the spookiest night of the year?

[BUIDL IDE  tools](https://secondstate.io/buidl) now has an upgraded UI just for Halloween. Trick or treat! A spooky pumpkin on the top welcomes you to the BUIDL’s Halloween.

![](/images/20191031-halloween-02.png)

Now, for developers, can you tell what the following code is for?

```
pragma lity ^1.2.4;

contract Halloween {
    
  function greet() pure public returns (string) {
    return "Happy Halloween";
  }
}
```

This is a very simple (yet spooky) smart contract example, just like the “Hello World” on the internet.

Let’s figure out how it works.

### start coding

Step 1
Open the BUIDL IDE tool in any browser. http://buidl.secondstate.io/

Step 2
Check the **Providers** tab on the left panel. The icon, if green, means BUIDL IDE is connected to the Second State DevChain. Please refresh the page if the icon is red.

![](/images/20191031-Halloween-04.png)

Step 3

3.1 Clear the content in the **contract** editor section of BUIDL.

3.2 Copy and paste the following code to the **contract** section 

```
pragma lity ^1.2.4;

contract Halloween {
    
  function greet() pure public returns (string) {
    return "Happy Halloween";
  }
}
```

3.2 Click on **Compile** and you will see the following. Then click on **deploy on chain**.

![](/images/20191031-Halloween-03.png)

The contract is now deployed on the DevChain, and you could see the Contract Name , TX and Address on the left tab. (TX records this transaction and it is unique. So is the contract address.)

3.3 

Now click open the **Halloween** contract, and click on the **greeting** button to call its greeting() function. In the LOG window, you will see “Happy Halloween”.

![](/images/20191031-halloween-01.png)

That’s it. Happy Halloween on the blockchain!

### About BUIDL IDE tools

BUIDL is a browser-based IDE that enables developers to create and deploy Decentralized Applications (DApps) on blockchains with ease.‪You can #compile, test, and deploy inside a browser. No software download!‬

Further reading:
* [BUIDL Introduction](https://secondstate.io/buidl)
* [BUIDL Documentation](https://docs.secondstate.io/buidl-developer-tool/getting-started)
* [Second State - Middleware for smart contracts](https://www.secondstate.io/)
