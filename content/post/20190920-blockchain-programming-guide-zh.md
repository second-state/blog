---
title: "如何使用BUIDL 发布一个自己的区块链网站"
date: 2019-09-20T09:42:14+08:00
draft: false
tags: ["blockchain application", "developer Community", "blockchain programming"]
categories: ["developer","zh"]
---

欢迎来到区块链的世界，在区块链应用里，您留下的足迹会永远留存，永远不会消失~

## 小科普

区块链应用通常以智能合约为后端，以HTML 和 JavaScript为前端。智能合约是区块链应用的核心，负责与区块链互动，现下流行的智能合约语言有Solidity 和Lity。

区块链的不可篡改，永久留存特性通常由智能合约来保证。

## 在开始之前，您需要准备以下工作

**#1** 在电脑浏览器打开 [https://buidl.secondstate.io/cmt](https://buidl.secondstate.io/xiaoxiao) （推荐使用Chrome 浏览器）

**#2** 检查开发工具BUIDL是否连接到区块链上

![BUIDL](/images/20190920-BUIDL-demo-01.png)

检查方法：查看页面左下方 Providers 是否是绿色图标，绿色表明已经连接到CyberMiles链上，红色表明尚未连接，请刷新重试。如果多次刷新后，仍然是红色，请及时告知。

**#3** 点击Account，找到当下使用的账号，然后往账号中打入少量 CMT。

如果没有 CMT，也可以复制此账号，发给我们，我们将给您的账号转1个CMT，以支付gas 费。（gas 费是区块链常见的名词，用以向网络中帮忙确认此笔交易的节点或矿工支付“薪水”。）

![BUIDL](/images/20190920-BUIDL-demo-02.png)

这样就把正式开发之前的准备做完了。

## 区块链网页后端——智能合约

**#1** 点击红色的Contract，清空默认代码，将下面的智能合约代码粘贴上去
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

**#2** 点击右上角Compile，在 _greeting 的横线处输入你想在网页上写的话，之后点击 Deploy to the chain，此处需要等待大概20秒。

![BUIDL](/images/20190920-BUIDL-demo-03.png)

这一步实施的是将BUIDL 部署到CyberMiles 链上，部署成功的标志是在页面左上角出现合约名字 Celebration 及TX和Address，页面下方log 出现TX 及 Success 字样。（小科普：TX是这笔交易的哈希值，是唯一的。Address 是合约地址，也是唯一的）

![BUIDL](/images/20190920-BUIDL-demo-04.png)

到这里我们就把区块链网站的后端写完了，这部分叫做智能合约，负责与区块链互动。区块链应用的特性主要由智能合约体现。

## 区块链网页前端

接下来是前端部分。

**#1** 点击绿色的Dapp，看到HTML，CSS，JS三个模块，清空这三个模块的代码。

将下面的代码粘贴到HTML 模块

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
                Recording on the CyberMiles public blockchain. It could take up to 20 seconds for CyberMiles global validator nodes to reach consensus.
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
    <br/>
    All "likes" are permanently recorded on the <a href="https://app.cybermiles.io">CyberMiles public blockchain</a>.
    <br/>
    [<a href="https://docs.secondstate.io/buidl-developer-tool/why-buidl">Learn more</a> | <a href="https://github.com/second-state/buidl/tree/master/demo/celebration">Source code</a>]
</div>
```

将下面的代码粘贴到JS 模块

```
/* Don't modify */
这里的代码不要改，从var instance = null; 开始复制
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
            }, 20 * 1000);
        }
    });
    return false;
}
```

**#2** BUIDL 是一个开源的开发工具，支持添加开源的代码资源，点击Resources 。

* 在JavaScript 点击加号，将下面的JS资源粘贴到弹窗里：https://code.jquery.com/jquery-3.4.1.min.js 并点击确定

* 在CSS 处点击加号，将下面的CSS 资源粘贴到弹窗里：https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css 并点击确定。

添加好后，再次点击Resources，将输入框收回。

![BUIDL](/images/20190920-BUIDL-demo-06.png)

**#3** 点击最左边的Run，会在页面右侧预览网页样式。然后来输入自己的名字，做个测试吧!（输入自己名字后，需要等大概20秒左右。分布在全球的节点达成共识需要一点时间）

![BUIDL](/images/20190920-BUIDL-demo-07.png)

**#4** 点击页面上方的Publish，输入自己网站的名称，比如孩子的小名，然后等待大概30秒，点击launched 就可以看到自己的网页。

![BUIDL](/images/20190920-BUIDL-demo-08.png)

接下来就把自己的网页分享到社交媒体，让亲朋好友给自己点赞留念吧！
