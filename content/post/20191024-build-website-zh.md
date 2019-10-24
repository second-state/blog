---
title: "教程|5分钟建一个去中心化的网站"
date: 2019-10-23T20:01:23+08:00
draft: false
tags: ["网页", "web", "BUIDL"]
categories: ["zh"]
---
不用买域名，购买虚拟主机空间，设置域名服务器，工信部备案等等繁琐的流程。

5分钟学会自己建一个去中心化的网站，从此做网站不求人！永久留存，完全去中心化！

![](/images/20191024-build-website-02.png)

这样一个网站可以让你写上自己说的话，并可以实现访问这个网页与网页进行交互，为你的网站点赞！

## 准备工作
1.在电脑浏览器打开[https://buidl.secondstate.io](https://buidl.secondstate.io) (推荐使用Chrome 浏览器)

2.检查开发工具BUIDL 是否连接到区块链上

![](/images/20191020-public-comment-01.png)

检查方法：查看页面左下方 Providers 是否是绿色图标，绿色表明已经连接到Second State DevChain链上，红色表明尚未连接，请刷新重试。

## 完成区块链网页后端——智能合约

1.点击红色的Contract，清空默认代码，将下面的智能合约代码粘贴上去
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

2.点击右上角Compile，在 _greeting 的横线处输入你想在网页上显示的的话，比如，程序员快乐~ 之后点击 Deploy to the chain。

![](/images/20190920-BUIDL-demo-03.png)

这一步实施的是将合约部署到Second State DevChain上，部署成功的标志是在页面左上角出现合约名字 PublicComments 及TX和Address，页面下方log 出现TX 及 Success 字样。（小科普：TX是这笔交易的哈希值，是唯一的。Address 是合约地址，也是唯一的）

![](/images/20190920-BUIDL-demo-04.png)

到这里我们就把区块链网站的后端写完了，这部分叫做智能合约，负责与区块链互动。区块链应用不可篡改，去中心化等特性主要由智能合约体现。

## 网页前端

1.点击绿色的Dapp，可以看到HTML，CSS，JS三个模块，清空这三个模块的代码。

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
    [<a href="https://docs.secondstate.io/buidl-developer-tool/why-buidl">了解更多</a> | <a href="https://blog.secondstate.io/post/20191024-build-website-zh/">教程</a>]
</div>
```
将下面的代码粘贴到JS 模块，从 var instance 开始粘贴。

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
            }, 2 * 1000);
        }
    });
    return false;
}
```

2.BUIDL 是一个开源的开发工具，支持添加开源的代码资源，点击Resources 。

![](/images/20190920-BUIDL-demo-06.png)

在JavaScript 点击加号，将下面的JS资源粘贴到弹窗里：https://code.jquery.com/jquery-3.4.1.min.js 并点击确定。

在CSS 处点击加号，将下面的CSS 资源粘贴到弹窗里：https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css 并点击确定。

3.点击最左边的Run，会在页面右侧预览网页样式。然后来输入自己的名字，做个测试吧!
![](/images/20191024-build-website-01.png)

4.点击页面上方的Publish，输入自己网站的名称，比如1024，然后等待大概30秒，点击launched 就可以看到自己的网页。

最后把自己的网页分享到社交媒体，让亲朋好友给自己点赞，赢取奖品吧！

