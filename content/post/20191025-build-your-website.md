---
title: "Build your own decentralized website with BUIDL IDE"
date: 2019-10-25T20:01:23+08:00
draft: false
tags: ["web", "BUIDL"]
categories: ["en"]
---

Have you ever tried to build your own website?

You need to 

* Get Web Hosting
* Register a Domain name(usually ~$15/year)
* Set up your website from the web host
* ········

Way too many steps! But with BUIDL IDE tools, you can build a decentralized website all by yourself in 5 minutes. The website will not only allow you to write any words you want but also allow your friends to access this page and interact with you. Click [here](https://opendapps.secondstate.io/bitcoin_1571992686948.html) to see an example.

![](/images/20191025-build-your-own-website-01.png)

Share your website link and invite them to leave a comment or “thumb up”. 
## The basic knowledge

Blockchain applications with smart contracts as the back end, with HTML, CSS, and Javascript as the front end. Smart contracts replace the usual connection between website and the web host on our decentralized website.

## start coding

**Step 1**
Open the BUIDL IDE tool in any browser. [http://buidl.secondstate.io/](http://buidl.secondstate.io/)

**Step 2**
Check out if the Providers work. The icon, if green, means BUIDL IDE is connected to the Second State DevChain. Please refresh the page when the icon is red.
![](/images/20191020-public-comment-01.png)

**Step 3**

3.1 Copy and paste the following code to the contract section of BUIDL.

```
pragma solidity >= 0.4.0;

contract Celebration {

    string public greeting;
    mapping (address => string) likes;
    address [] addrs;

    constructor(string _greeting) public {
        greeting = _greeting;
    }

    function addLike (address _addr, string _name) public {
        likes[_addr] = _name;
        addrs.push(_addr);
    }

    function getLikeName(address _addr) public view returns(string) {
        return likes[_addr];
    }

    function getAddrs () public view returns (address []) {
        return addrs;
    }
}
```

3.2 Click on Compile and you will see the following. Enter your text in `_greeting`, and then click on deploy on chain.

![](/images/20190920-BUIDL-demo-03.png)

The contract is now deployed on the DevChain, and you could see the Contract Name, TX and Address on the left tab. (TX records this transaction and it is unique. So is the contract address.)

![](/images/20190920-BUIDL-demo-04.png)

We have completed the back end of the website. Smart contract will connect the blockchain.

**Step 4**

4.1 Go to the dapp section. Clear the codes in the HTML, CSS, and JS editor. (Note: Please do not clear the code of “Don't modify” section)

4.2 Copy and paste the following HTML code into the HTML editor.

```
<div class="container">
    <br/>
    <div class="jumbotron">
        <p class="lead" id="greeting"></p>
        <hr/>
        <form id="form" class="form-inline">
            <div class="form-group mx-sm-3 mb-2">
                <input type="text" class="form-control" id="name" placeholder="Your Name">
            </div>
            <div class="alert alert-primary" role="alert" id="recording" style="display:none">
                Recording on the Second State DevChain.Please wait up to 5 seconds for confirmation.
            </div>
            <button type="button" id="submit" onclick="like();" class="btn btn-primary mb-2">Like👍</button>
        </form>
        <p id="me" style="display:none">Thank you, <span id="myname" class="badge badge-info"></span></p>
    </div>
    <p id="likes"></p>
</div>

<div style="text-align:center">
    <br/><br/>
    Created by the <a href="https://buidl.secondstate.io/">BUIDL IDE</a>
     <br/><br/>
    [<a href="https://docs.secondstate.io/buidl-developer-tool/why-buidl">Learn more</a> | <a href="https://blog.secondstate.io/post/20191025-build-your-website/">Check out the tutorial</a>]
</div>
```

4.3 Copy and paste the following JavaScript code from var instance = null into the JS editor.

```
/* Don't modify */
Do not change the code Paste from `var instance = null; `
/* Don't modify */

var instance = null;
window.addEventListener('web3Ready', function() {
  var contract = web3.ss.contract(abi);
  instance = contract.at(cAddr);
  reload();
});

function reload() {
    document.querySelector("#greeting").innerHTML = instance.greeting();
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            var myName = instance.getLikeName(address);
            if (myName) {
                document.querySelector("#form").style.display = "none";
                document.querySelector("#me").style.display = "block";
                document.querySelector("#myname").innerHTML = myName;
            }
            
            var likes = "Liked by ";
            var addrs = instance.getAddrs();
            addrs.forEach(function(addr) {
                instance.getLikeName(addr, function (ee, r) {
                    if (!ee) {
                        likes = likes + "<span class=\"badge badge-success\">" + r + "</span> ";
                        document.querySelector("#likes").innerHTML = likes;
                    }
                });
            });
        }
    });
}

function like () {
    document.querySelector("#submit").disabled = true;
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            document.querySelector("#recording").style.display = "block";
            document.querySelector("#submit").innerHTML = "Please Wait ...";
            instance.addLike(address, document.querySelector("#name").value, {
                gas: 400000,
                gasPrice: 0
            }, function (e, result) {
                console.log(e + " : " + result);
                // ... ...
            });
            setTimeout(function () {
                reload ();
            }, 2 * 1000);
        }
    });
    return false;
}
```

4.4 Click on the Resources tab, and add the following as resources.

* CSS: https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css
* JavaScript: https://code.jquery.com/jquery-3.4.1.min.js

![](/images/20190920-BUIDL-demo-06.png)

4.5 Click on Run to see your dapp in action and enter your name in BUIDL to test if this works. You will be able to see your name on the right.

![](/images/20191025-build-your-own-website-01.png)

4.6 Finally, you can publish the dapp.

Just click on the Publish button to name the webpage. Once published, you can share the published URL online for everyone to give you a “like”.
