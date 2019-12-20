---
title: "只需 5 分钟，教你如何编写并执行一个 Rust + WebAssembly 程序"
date: 2019-12-06T09:42:14+08:00
draft: false
tags: ["Rust", "WebAssembly"]
categories: ["ethereum","zh"]
---


> 在探讨 WASM 在服务端的巨大潜力时，我们提到 WASM 的一大优势就是支持有影响力的新锐编程语言，例如 Rust 。这篇文章将展示如何编写并执行一个 Wasm Rust 程序，只有代码。
本文作者： Second State 的研究员、开源核心开发 Tim McCallum。

以下为正文：

该演示是使用 Ubuntu Linux 操作系统和 Google 的 Chrome 浏览器进行的。其他组合尚未经过测试。

### 第1步：安装 Apache2 和 Rust

运行以下所有 Ubuntu 系统设置命令（更新，安装 Apache2 和 Rust ）
```
sudo apt-get update
sudo apt-get -y upgrade
sudo apt-get -y install apache2
sudo chown -R $USER:$USER /var/www/html
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
rustup target add wasm32-wasi
rustup override set nightly
```
### 第2步：创建一个新的 Rust 项目

创建一个快速的 Rust 项目

```
cd ~
cargo new --lib triple
cd triple
```
### 第3步：为 Wasm 配置 Rust

通过将下面的配置添加到 〜/ triple / Cargo.toml 文件的 lib 部分来配置 rust

```
[lib]
name = "triple_lib"
path = "src/triple.rs"
crate-type =["cdylib"]
```
### 第4步：指定构建目标

通过在 〜/ .cargo.config 中创建一个新文件并添加以下配置来完成 Rust 的配置

```
[build]
target = "wasm32-wasi"
```
### 第5步：写 Rust

编写一个快速的 Rust 程序并将其保存为 Triple.rs（在 〜/ triple / src 目录中）
```
#[no_mangle]
pub extern fn triple(x: i32) -> i32 {
 return 3 * x;
}
```
### 第6步：构建 Wasm 代码

将 Rust 代码构建到 Wasm 中，然后将 Wasm 文件复制到 Apache2 Web 服务器区域
```
cd ~/triple
cargo build - release
cp -rp ~/triple/target/wasm32-wasi/release/triple_lib.wasm /var/www/html/triple.wasm
```
### 第7步：制作 HTML 网页

在 var / www / html / 目录中创建一个名为 Triple.html 的新文件，并使用以下代码填充它。

```
<html>
	<head>
		<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous" />
		<script>
    if (!('WebAssembly' in window)) {
      alert('you need a browser with wasm support enabled :(');
    }
(async () => {
  const response = await fetch('triple.wasm');
  const buffer = await response.arrayBuffer();
  const module = await WebAssembly.compile(buffer);
  const instance = await WebAssembly.instantiate(module);
  const exports = instance.exports;
  const triple = exports.triple;
  var buttonOne = document.getElementById('buttonOne');
        buttonOne.value = 'Triple the number';
        buttonOne.addEventListener('click', function() {
          var input = $("#numberInput").val();
          alert(input + ' tripled equals ' + triple(input));
        }, false);
})();    

  </script>
	</head>
	<body>
		<div class="row">
			<div class="col-sm-4"></div>
			<div class="col-sm-4">
				<b>Rust to Wasm in under 5 minutes - Triple the number</b>
			</div>
			<div class="col-sm-4"></div>
		</div>
		<hr />
		<div class="row">
			<div class="col-sm-2"></div>
			<div class="col-sm-4">Place a number in the box</div>
			<div class="col-sm-4"> Click the button</div>
			<div class="col-sm-2"></div>
		</div>
		<div class="row">
			<div class="col-sm-2"></div>
			<div class="col-sm-4">
				<input type="text" id="numberInput" placeholder="1", value="1">
				</div>
				<div class="col-sm-4">
					<button class="bg-light" id="buttonOne">Triple the number</button>
				</div>
				<div class="col-sm-2"></div>
			</div>
		</body>
		<script
  src="https://code.jquery.com/jquery-3.4.1.js"
  integrity="sha256-WpOohJOqMqqyKL9FccASB9O0KwACQJpFTUBLTYOVvVU="
  crossorigin="anonymous"></script>
	</html>
  ```

### 第8步：单击鼠标执行写好的 Rust 代码

在 triple HTML 页面：http://12.345.456.78/triple.html 上访问计算机的IP。

然后单击 “Triple the number” 按钮。
![](images/20191220-rust-webassembly-01.png)

将显示以下提示。
![](images/20191220-rust-webassembly-02.png)

（如图所示：8的三倍等于24）

到这里我们就完成了一个Rust + WebAssembly 程序，你也来试试吧！

作者：Michael_Yuan
链接：https://juejin.im/post/5de62000e51d4557f852a141
