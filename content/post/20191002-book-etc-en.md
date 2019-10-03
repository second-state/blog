---
title: "Treasure Hunt -- Ethereum Classic Edition"
date: 2019-10-02T01:01:23+08:00
draft: false
tags: ["Ethereum Classic","Building Blockchain Apps","book","BUIDL","ETC"]
categories: ["en"]
author: "Second State"
---

The *Building Blockchain Apps* book is to be published by Addison-Wesley in 2019. It provides a comprehensive guide for developers to incorporate the blockchain technology into existing and future applications. You can pre-order it from [Amazon](https://www.amazon.com/Building-Blockchain-Apps-Michael-Yuan/dp/0135172322/) or [InformIT](http://www.informit.com/store/building-blockchain-apps-9780135172322).

![](/images/20191002-book-etc-01.png)

## How to get the 35% discount code

The discount code is “hidden in plain sight” in a smart contract deployed on the Ethereum Classic blockchain. Using [BUIDL for ETC](http://secondstate.io/etc), you can instantiate an instance of this contract and call it’s functions to get its state. So, let’s get started. 

*Step 1*: Launch the BUIDL for ETC from any browser: [https://buidl.secondstate.io/etc](https://buidl.secondstate.io/etc) 

*Step 2*: In the *contract* section, copy and paste in the smart contract code below.

```
pragma solidity >= 0.4.0;

contract PublicComments {

    address owner;
    string public greeting;
    struct Comment {
        string name;
        string email;
        string comment;
    }
    mapping (address => Comment) comments;
    address [] addrs;

    function PublicComments (string _greeting) public {
        owner = msg.sender;
        greeting = _greeting;
    }

    function setGreeting (string _greeting) public {
        if (msg.sender != owner) {throw;}
        greeting = _greeting;
    }

    function addComment (string _name, string _email, string _comment) public {
        comments[msg.sender] = Comment(_name, _email, _comment);
        addrs.push(msg.sender);
    }

    function getComment(address _addr) public constant returns(string, string, string) {
        return (comments[_addr].name, comments[_addr].email, comments[_addr].comment);
    }

    function getAddrs () public constant returns (address []) {
        return addrs;
    }
}
```

*Step 3*: Hit the *Compile* button. And you will see the compiled interface of the contract as below. 

![](/images/20191002-book-etc-02.png)

*Step 4*: Now, instead of deploying a new instance of this contract, we need to instantiate an instance of an existing contract on the blockchain. Enter the contract address `0x52a8e808254e1a3de50ac493a1011d2eded3d1dd` on the line with `0x`, and click on the *At* button. 

![](/images/20191002-book-etc-03.png)

*Step 5*: Now click open the *PublicComments* contract, and click on the *greeting* button to call its `greeting()` function. In the LOG window, you will see the discount code and purchase link stored in the contract. 

![](/images/20191002-book-etc-04.png)

## Build a dapp

Now you have interacted with a smart contract on the blockchain. With BUIDL, you can go one step further and create a web UI around the smart contract. That is called a decentralized app (dapp). Let’s do that as a learning exercise! 

As you probably already noticed, the smart contract has logic to store user comments. We will build a dapp that allows users to leave comments from the web. Since the Ethereum Classic blockchain requires a little gas fee for storing data inside smart contracts, you will need a little ETCs for this exercise. If you do not have ETCs, email us at etc@secondstate.io and we will send you a little. 

If you leave a comment in the smart contract, your comment will be forever stored on the Ethereum Classic blockchain and we will send you a digital gift!

*Step 6*: Go to the *dapp* section. Open the *Resources* tab, and enter the following CSS and JavaScript resources. 

* Javascript: https://code.jquery.com/jquery-3.4.1.min.js
* CSS: https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css

*Step 7*: Copy and paste in the HTML code into the HTML editor. 

```
<div class="container">
    <br/>
    <div class="alert alert-primary" role="alert">If you have MetaMask for ETC, select MetaMask in the accounts widget at bottom right.</div>
    <div class="jumbotron">
        <p class="lead" id="greeting"></p>
        <hr/>
        <form id="form">
            <div class="form-group">
                <label for="name">Name</label>
                <input type="text" class="form-control" id="name" placeholder="">
            </div>
            <div class="form-group">
                <label for="email">Email</label>
                <input type="email" class="form-control" id="email" placeholder="">
            </div>
            <div class="form-group">
                <label for="comment">Comment</label>
                <input type="text" class="form-control" id="comment" placeholder="">
            </div>
            <button type="button" id="submit" class="btn btn-primary">Send Comment</button>
        </form>
        <div id="formSubmitted" style="display:none">Please wait up to 42 seconds for confirmation ...</div>
        <p id="me" style="display:none">Thank you, <span id="myname" class="badge badge-info"></span></p>
    </div>
    <p>You need to pay a tiny amount of ETCs to comment. Make sure that you have at least 0.1 ETC at your current account address: <a target="_blank" href="" id="myAddr"></a></p>
    
    <table class="table table-striped">
        <thead>
            <tr>
                <th scope="col">Name</th>
                <th scope="col">Comment</th>
            </tr>
        </thead>
        <tbody id="likes">
        </tbody>
    </table>
    <p style="text-align:center">Created with <a target="_blank" href="https://www.secondstate.io/etc/">BUIDL for ETC</a>.</p>
</div>
```

*Step 8*: Copy and paste in the Javascript code into the JS editor. 

```
var instance = null;
window.addEventListener('web3Ready', function() {
  var contract = web3.ss.contract(abi);
  instance = contract.at(cAddr);
  reload();
});

function reload() {
    instance.greeting(function (e, r) {
        $("#greeting").html(r);
    });
    
    $("#form").css("display", "block");
    $("#formSubmitted").css("display", "none");
    $("#me").css("display", "none");
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            $("#myAddr").text(address);
            $("#myAddr").attr("href", "https://blockscout.com/etc/mainnet/address/" + address);

            instance.getComment(address, function (ee, result) {
                if (result[0]) {
                    $("#form").css("display", "none");
                    $("#me").css("display", "block");
                    $("#myname").html(result[0]);
                }
            });
            
            var likes = "";
            instance.getAddrs(function (ee, addrs) {
                addrs.forEach(function(addr) {
                    instance.getComment(addr, function (ee, r) {
                        if (!ee) {
                            likes = likes + "<tr><td>" + r[0] + "</td><td>" + r[2] + "</td></tr>";
                            $("#likes").html(likes);
                        }
                    });
                });
            });
            $("#likes").html(likes);
        }
    });
}

$("#submit").click(function() {
    if ((!$("#name").val().trim()) || (!$("#comment").val().trim())) {
      alert("Please enter both a name and a comment");
      return false;
    }
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            $("#form").css("display", "none");
            $("#formSubmitted").css("display", "block");
            instance.addComment ($("#name").val(), $("#email").val(), $("#comment").val(), {
                gas: 1000000,
                gasPrice: 5000000000
            }, function (ee, r) {
                if (ee) {
                    window.alert("Failed. Check if there is at least 0.1 ETC (for gas fee) in your account " + address);
                }
            });
            setTimeout(function () {
                reload ();
            }, 42 * 1000);
        }
    });
    return false;
});
```

*Step 9*: Now, hit the *Run* button. You will see the dapp UI running in the right panel. You can see all the comments in the smart contract, and leave your own comment now! Please leave your email so that we can get in touch!

>As we mentioned, you will need to have a little ETC in your account to pay for gas. Your default account is randomly generated in the *Accounts* tab. You could send some ETCs to it, or your could import one of your existing addresses into the *Accounts* tab. 

*Bonus step*: You could hit the *Publish* button and publish your dapp to the web for anyone to see. 

