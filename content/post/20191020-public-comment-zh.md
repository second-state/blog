---
title: "如何使用BUIDL 发起投票议题"
date: 2019-10-20T01:01:23+08:00
draft: false
tags: ["Building Blockchain Apps","Dapp","BUIDL"]
categories: ["zh"]
---

在区块链世界里，您留下的评论会永远留存，不能被篡改，永远不会消失~

## DApp小科普

区块链应用（DApp）通常以智能合约为后端，以HTML 和 JavaScript为前端。智能合约是区块链应用的核心，负责与区块链互动，现下流行的智能合约语言有Solidity 和Lity。

区块链的不可篡改，永久留存特性通常由智能合约来保证。

## 在开始之前，您需要准备以下工作

*#1* 在电脑浏览器打开 [https://buidl.secondstate.io](https://buidl.secondstate.io)（推荐使用Chrome 浏览器）

*#2* 检查开发工具BUIDL是否连接到区块链上

![](/images/20191020-public-comment-01.png)

检查方法：查看页面左下方 Providers 是否是绿色图标，绿色表明已经连接到Second State DevChain链上，红色表明尚未连接，请刷新重试。如果多次刷新后，仍然是红色，请及时告知。


## 区块链网页后端——智能合约

*#1* 点击红色的Contract，清空默认代码，将下面的智能合约代码粘贴上去

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

    constructor (string _greeting) public {
        owner = msg.sender;
        greeting = _greeting;
    }

    function setGreeting (string _greeting) public {
        require (msg.sender == owner);
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

*#2* 点击右上角Compile，在 _greeting 的横线处输入你想要征集的问题，之后点击 Deploy to the chain

![](/images/20191020-public-comment-03.png)

这一步实施的是将合约部署到Second State DevChain上，部署成功的标志是在页面左上角出现合约名字 PublicComments 及TX和Address，页面下方log 出现TX 及 Success 字样。（小科普：TX是这笔交易的哈希值，是唯一的。Address 是合约地址，也是唯一的）

![](/images/20191020-public-comment-02.png)

到这里我们就把区块链网站的后端写完了，这部分叫做智能合约，负责与区块链互动。区块链应用的特性主要由智能合约体现。

## 区块链网页前端

接下来是前端部分。

*#1* 点击绿色的Dapp，可以看到HTML，CSS，JS三个模块，清空这三个模块的代码。

将下面的代码粘贴到HTML 模块

```
<div class="container">
    <br/>
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
        <div id="formSubmitted" style="display:none">Please wait up to 5 seconds for confirmation ...</div>
        <p id="me" style="display:none">Thank you, <span id="myname" class="badge badge-info"></span></p>
    </div>
    
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
    <p style="text-align:center">Created with <a target="_blank" href="https://docs.secondstate.io/buidl-developer-tool/why-buidl">the BUIDL IDE</a>.</p>
</div>
```

将下面的代码粘贴到JS 模块，从 var instance 开始粘贴。

```
/* Don't modify */
这里不要改
/* Don't modify */
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
                gas: 499000,
                gasPrice: 0
            }, function (ee, r) {
                if (ee) {
                    window.alert("Failed at " + address);
                }
            });
            setTimeout(function () {
                reload ();
            }, 5 * 1000);
        }
    });
    return false;
});
```

*#2* BUIDL 是一个开源的开发工具，支持添加开源的代码资源，点击Resources 。

* 在JavaScript 点击加号，将下面的JS资源粘贴到弹窗里：https://code.jquery.com/jquery-3.4.1.min.js 并点击确定
* 在CSS 处点击加号，将下面的CSS 资源粘贴到弹窗里：https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css 并点击确定。
添加好后，再次点击Resources，将输入框收回。

![](/images/20190920-BUIDL-demo-06.png)

*#3* 点击最左边的Run，会在页面右侧预览网页样式。然后来输入自己的名字，做个测试吧!（输入自己名字后，需要等大概5秒左右的时间确认）

![](/images/20191020-public-comment-05.png)

*#4* 点击页面上方的Publish，输入网站的名称，然后等待大概30秒，点击launched 就可以通过网页的形式向周围的人传播。

![](/images/20190920-BUIDL-demo-08.png)

