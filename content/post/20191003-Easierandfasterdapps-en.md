---
title: "Easier and faster dapps on the Ethereum Classic blockchain"
date: 2019-10-03T01:01:23+08:00
draft: false
tags: ["Ethereum Classic","Building Blockchain Apps","book","BUIDL","ETC"]
categories: ["en"]
author: "Second State"
---

Ethereum, the world’s first and dominant smart contract platform, has the largest dapp ecosystem with more than a million deployed smart contracts and hundreds of millions of dollars in monthly transactions.

Ethereum Classic is the original Ethereum blockchain. With fast confirmation time, ultra-low gas cost, and now with first-class developer tooling from the BUIDL IDE, the Ethereum Classic blockchain is also becoming the best platform for deploying smart contracts and dapps. Checkout how: http://secondstate.io/etc 

“Ethereum Classic is not just the original Ethereum blockchain,” Elizabeth Kukka, Executive Director of Ethereum Classic Labs, said. “It also is the most dapp-friendly Ethereum blockchain with its very low gas fees, store-of-value native cryptocurrency, and fast confirmation times. We look forward to collaborating with Second State to further empower our developer communities.”
The twin challenges of Ethereum
Recently, the Ethereum blockchain is operating near its capacity. The gas price required for developers and users to operate smart contracts is now 10 times the value from earlier in the year. Furthermore, it could take hours for a smart contract transaction to get confirmed on the network.

![](/images/figure1.png)
Figure 1: Even at a high gas price of 17 GWei, it takes many hours to confirm a transaction.

Ethereum founder, Vitalik Buterin, has acknowledged this very issue. Now more than ever, Ethereum Classic is an appealing choice for dapp developers. 

![](/images/figure2.png)
Figure 2: Vitalik’s take on transaction fees and network effect.

However, there is a darth of developer tools on Ethereum Classic. Most dapp developer tools are optimized for the Ethereum blockchain. 

And even on Ethereum, dapp development is already very hard compared with web and mobile app development. Developers and users must be familiar with cryptocurrency, crypto wallet, blockchain nodes, RPC services, web3, a variety of tools, specialized programming languages, frameworks, and services. All of this has hampered dapp adoption greatly.

## BUIDL for ETC

The BUIDL IDE is the one-stop development environment for smart contracts and dapps. You can develop and publish complete dapps from a web browser without any software download or install. 

![](/images/figure3.png)
Figure 3: Traditional Ethereum dev tools vs Second State BUIDL. 

Getting started with Ethereum Classic dapp development is as easy as 1-2-3. For the impatient, watch the 2-minute tutorial video now. 

**Step 1** Open the BUIDL IDE tool in any browser. http://buidl.secondstate.io/etc 

**Step 2** Open the Accounts tab and send a little ETC to your default account. If you do not have ETC, you can ask for some from etc@secondstate.io

*Note: If you have Metamask for ETC, you could opt to use Metamask in the Providers tab. BUIDL and dapps it creates will now use the default account in Metamask to make contract calls and to pay for gas.*

**Step 3** Develop and deploy your smart contract and dapp! In the next section, we will show an example dapp you can deploy on ETC. 

## A thumb up or thumb down dapp

The example dapp is a web application where people can vote thumb up or thumb down on a statement. All the votes are recorded on the blockchain, and since gas is required, only ETC token holders can vote. See a published voting dapp.

![](/images/figure4.png)
Figure 4: The voting dapp in action, vote now. 

![](/images/figure5.png)
Figure 5: The voting dapp in action, voted. 

Now, let’s check out the source code of the application. With BUIDL for ETC, you can have your own voting dapp published on ETC in minutes. 

## Creating the dapp

You will need to create and deploy a smart contract as the backend data service for the dapp, and then create an HTML5 / Javascript application as the front UI to interact with the smart contract. Once you have tested the dapp in BUIDL, you can publish it to the web for the world to use it. All these tasks can be done within BUIDL.

**Step 3.1** Copy and paste the following code to the **contract** section of BUIDL. 

```
pragma solidity >= 0.4.0;

contract Vote {

    string public greeting;
    string public photoUrl;
    mapping (address => int) votes;
    uint ups;
    uint downs;

    function Vote (string _greeting, string _photoUrl) public {
        greeting = _greeting;
        photoUrl = _photoUrl;
    }

    function vote (int _choice) public {
        if (votes[msg.sender] != 0) { throw; }
        if (_choice != 1 && _choice != -1) { throw; }
        votes[msg.sender] = _choice;
        if (_choice == 1) ups++;
        if (_choice == -1) downs++;
    }

    function getVotes () public constant returns (uint, uint) {
        return (ups, downs);
    }

    function getVote (address _addr) public constant returns (int) {
        return votes[_addr];
    }
}
```

The smart contract is very simple. It provides the text and image url to be voted on, and keeps a record of votes. The `vote()` method is called by voters to vote thumb up or down. But notice that the Solidity syntax is version 0.4.2, which is the Solidity compiler version supported by the Ethereum Classic blockchain.

**Step 3.2** Click on **Compile** and you will see the following. Enter your text and image URL to be voted on, and then click on **deploy to the chain**. 

*Note: Please open the Accounts tab and make sure that the default address has a little ETC.*

![](/images/figure6.png)
Figure 6: Deploy the smart contract to Ethereum Classic.

The contract is now deployed on the ETC blockchain, and you can call it’s functions directly from inside BUIDL. 

![](/images/figure7.png)
Figure 7: Calling functions on the deployed contract. 

**Step 3.3** Go to the **dapp** section. Click on the **Resources** tab, and add the following as resources. 

* CSS: https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css 
* JavaScript: https://code.jquery.com/jquery-3.4.1.min.js 

**Step 3.4** Next, copy and paste the following HTML code into the HTML editor. 

```
<div class="container">
   <br/>
   <div class="alert alert-primary" role="alert">If you have MetaMask for ETC, select MetaMask in the accounts widget at bottom right.</div>
   <div class="jumbotron">
      <p class="lead" id="greeting"></p>
      <div id="imageDiv" style="display:none">
         <img id="image" src="" class="img-fluid img-thumbnail"/>
      </div>
      <hr/>
      <p id="votes" style="display:none">
         <span id="ups"></span> voted 👍 |
         <span id="downs"></span> voted 👎
      </p>
      <form id="form" class="form-inline" style="display:none">
         <button id="voteUp" type="button" onclick="return vote(1);" class="btn btn-secondary mb-2">👍</button>
         <button id="voteDown" type="button" onclick="return vote(-1);" class="btn btn-secondary mb-2">👎</button>
      </form>
      <div id="formSubmitted" style="display:none">Please wait 20 seconds ...</div>
      <div id="myVoteUp" style="display:none">You have already voted 👍</div>
      <div id="myVoteDown" style="display:none">You have already voted 👎</div>
   </div>
   <p>You need to pay a tiny amount of ETCs to vote. Make sure that you have at least 0.1 ETC at your current account address: <a target="_blank" href="" id="myAddr"></a></p>
   <p style="text-align:center">Created with <a target="_blank" href="https://www.secondstate.io/etc/">BUIDL for ETC</a>. Checkout the <a target="_blank" href="https://docs.secondstate.io/buidl-developer-tool/demo-a-voting-dapp/ethereum-classic">tutorial</a> to create your own!</p>
</div>
```

**Step 3.5** Copy and paste the following JavaScript code into the JS editor. 

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
    instance.photoUrl(function (e, r) {
        if (!e && r) {
            $("#imageDiv").css("display", "block");
            $("#image").attr("src", r);
        }
    });
    instance.getVotes(function (e, r) {
        if (!e && (r[0] > 0 || r[1] > 0)) {
            $("#votes").css("display", "block");
            $("#ups").text(r[0]);
            $("#downs").text(r[1]);
        }
    });

    $("#form").css("display", "none");
    $("#formSubmitted").css("display", "none");
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            instance.getVote(address, function (ee, r) {
                if (r == 1) {
                    $("#myVoteUp").css("display", "block");
                } else if (r == -1) {
                    $("#myVoteDown").css("display", "block");
                } else {
                    $("#form").css("display", "block");
                }
            });
            $("#myAddr").text(address);
            $("#myAddr").attr("href", "https://blockscout.com/etc/mainnet/address/" + address);
        }
    });
}

function vote (choice) {
    web3.ss.getAccounts(function (e, address) {
        if (!e) {
            $("#form").css("display", "none");
            $("#formSubmitted").css("display", "block");
            instance.vote(choice, {
                gas: 400000,
                gasPrice: 5000000000
            }, function (ee, result) {
                if (ee) {
                    window.alert("Failed. Check if there is at least 0.1 ETC (for gas fee) in your account " + address);
                }
            });
            setTimeout(function () {
                reload ();
            }, 20 * 1000);
        }
    });
    return false;
}
```

**Step 3.6** Click on **Run** to see the dapp in action! You can now vote thumb up or down inside BUIDL. 

![](/images/figure8.png)
Figure 8: The voting dapp in action in BUIDL.

**Step 3.7** Finally, you can publish the dapp. Just click on the Publish button and give the dapp a name. Once published, you can share the published URL to the public to vote on your issue!

## Using the dapp

The dapp displays the voting text, image, and current results to the public on the web. The dapp retrieves such information from the blockchain free of charge. However, by design, only ETC holders can vote as the dapp requires a tiny amount of ETC as gas when it calls the smart contract to vote. 

The dapp automatically creates addresses for users. In order to vote, the user must have a little ETC in the selected default address to pay for gas.

![](/images/figure9.png)
Figure 9: The auto-created addresses for users. The user must send a little ETC to the selected default address to pay for gas. 

Or, if the user has Metamask for ETC, she can choose to have Metamask handle accounts and gas payments. 

![](/images/figure10.png)
Figure 10: Use Metamask for ETC to make transactions on the dapp. 

Now, while we consider the requirement for ETC is a feature for this dapp, many dapps will benefit from a lower barrier of entry. Can we waive gas fees altogether for users? Well, in some cases, we can. For gas-less dapps, you could potentially use the Second State DevChain (for development and demonstration only), or the CyberMiles public blockchain. 

* [DevChain tutorial](https://docs.secondstate.io/buidl-developer-tool/demo-a-voting-dapp/devchain) for the voting dapp
* [CyberMiles tutorial](https://docs.secondstate.io/buidl-developer-tool/demo-a-voting-dapp/cybermiles) for the voting dapp

## Conclusion

With tools like BUIDL for ETC, it is now a breeze to develop and deploy dapps on the Ethereum Classic blockchain. All you need is a modern web browser and a little ETC to pay for network operations. What are you waiting for? http://secondstate.io/etc

















