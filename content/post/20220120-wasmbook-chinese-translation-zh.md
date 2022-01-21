---
title: "WasmEdge 文档翻译规范和术语表"
date: 2022-01-20T10:01:23+08:00
draft: false
tags: ["翻译","文档","GitHub"]
categories: ["zh"]
---



本指南希望帮助社区贡献者统一中文文案、排版的相关用法，降低沟通成本，增强译文的规范性和统一性，使文档更加易读。详细指南请参考[中文排版规则](https://github.com/mzlogin/chinese-copywriting-guidelines)。

翻译时请先查阅翻译术语表，涉及到的关键词统一按照规范进行翻译。如有异议或添加新的术语请通过提 GitHub Issue 和维护团队商讨。


* 空格
    * 中英文之间需要增加空格
    * 中文与数字之间需要增加空格
    * 数字与单位之间需要增加空格
    * 全角标点与其他字符之间不加空格
* 标点符号
    * 不重复使用标点符号
    * 破折号前后需要增加一个空格
* 全角半角
    * 中文标点使用全角字符
    * 数字使用半角字符
    * 遇到完整的英文整句、特殊名词，其內容使用半角标点

文档使用 MarkDown 格式，对于想要翻译的文档，点击 Fork 和编辑可查看英文原文的 MarkDown 格式文本，将文本复制到你将要 commit 的内容中，在此文本基础上进行修改和翻译。


# 翻译术语列表

**列表持续完善中，如有缺漏，欢迎提 PR 补充或提出修改建议。**

## 不需要翻译的英文关键词 请保留原文

|英文	|
|---	|
|WasmEdge	|
|WebAssembly	|
|runtime	|
|Wasm	|
|Kubenetes	|
|[Bulk memory operations](https://github.com/WebAssembly/bulk-memory-operations/blob/master/proposals/bulk-memory-operations/Overview.md)	|
|[Reference Types](https://webassembly.github.io/reference-types/core/)	|
|Dapr	|
|Docker	|
|crun	|
|runc	|
|Kind	|
|Knative	|
|serverless	|
|FaaS	|
|API	|
|WASI	|
|Substrate	|
|Pallet	|
|Rust	|
|JavaScript	|
|Go	|
|Swift	|
|AssemblyScript	|
|Kotlin	|
|Grain	|
|Python	|
|Linux	|
|Windows	|
|Mac	|
|seL4	|
|Open Harmony	|
|Raspberry Pi	|
|plug-in	|
|CRI-O	|
|containerd	|
|[SuperEdge](https://wasmedge.org/book/en/kubernetes/kubernetes/superedge.html)	|
|KubeEdge	|
|AWS Lambda	|
|Second State
|MOSN	|
|Yomo	|
|Polkadot	|
|Vercel	|
|Netlify	|
|Slack	|
|demo	|
|crate	|
|NAPI	|
|[rustwasmc](https://github.com/second-state/rustwasmc)	|
|eBPF	|
|host	|
|Jamstack	|
|TensorFlow	|
|ImageNet	|
|QuickJS	|
|TinyGo	|
|host	|
|web	|
|JSON	|
|sidercar	|
|Grain	|
|repo	|
|grayscale	|
|fork	|
|GitHub Pages	|
|Next.js	|
|handler	|
|PowerShell	|
|Guest Linux OS	|
|LLVM	|

除了大段代码块内很明显的解释性语句，所有代码内容请勿翻译。

## 英文中文都需要保留的关键词

>需要保留关键词的地方统一按照`中文翻译（英文关键词）`的格式。

|英文	|译文	|
|---	|---	|
|CNCF	|云原生计算基金会	|
|RTOS	|实时操作系统	|
|AOT	|提前（编译）	|
|JIT	|即时（编译）	|


## 不需要保留英文的关键词

|英文	|译文	|
|---	|---	|
|developer	|开发者	|
|microservices	|微服务	|
|smart contracts	|智能合约	|
|proprietary	|	|
|Ethereum	|以太坊	|
|service mesh	|服务网格	|
|container	|容器	|
|parser	|分析器	|
|resolver	|解析器	|
|image	|镜像	|
|framework	|框架	|
|micokernel	|微内核	|
|app	|应用	|
|application	|应用程序	|
|feature	|特性	|
|use case	|应用场景	|
|cloud native	|云原生	|
|integration	|集成	|
|OS	|操作系统	|
|console	|操作台	|
|library	|库	|
|Golang	|Go 语言	|
|VM	|虚拟机	|
|module	|模块	|
|instance	|系统进程	|
|inference	|推理	|
|[Prerequisite](https://wasmedge.org/book/en/frameworks/serverless/vercel.html#prerequisite)	|前期准备	|
|Package	|包	|
|Macro	|宏	|
|runner	|运行器	|
|memory	|内存	|
|directory	|目录	|
|powered by	|由……驱动	|
|dependency	|依赖项	|

