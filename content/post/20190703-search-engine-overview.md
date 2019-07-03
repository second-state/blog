---
title: "Blockchain search engine — real-time data for decentralised applications (DApps)"
date: 2019-07-03T09:42:14+08:00
draft: true
tags: ["blockchain", "search", "DApp"]
categories: ["business"]
author: "Timothy McCallum"
---

## Mobile handheld devices

Web traffic from smartphones and tablets first overtook traffic from personal computers (PCs) in mid-2016. Since then mobile handheld devices have become ubiquitous. Whilst their primary use may have started out as a way for us to communicate with each other, the functionality of these devices has grown to the point where they have now become a necessity. 

Not only do they provide access to gaming, music, books and more. They also allow us to access and transact value (online banking) as well as provide us with access to both the online gaming and global online shopping phenomena.

![](/images/20190703-search-engine-overview-01.jpg)

Just as with a given centralized application (App), the adoption of a, blockchain based, decentralized application (DApp) can be accelerated and sustained if provided to the end user via a similarly convenient platform i.e. a mobile handheld device.

---

## DApps need data.

Whilst ubiquitous, in terms of mainstream centralised applications (Apps), mobile devices are not able to participate as full blockchain nodes. This is due, largely, to data storage requirements. In all blockchain protocols each individual node stores all states and processes all transactions. This provides a large amount of security, but in turn requires computational and storage resources which mobile handheld devices do not have. 

> For DApps to thrive, they need a fast and efficient way to access specific blockchain data in real-time.

From a DApp's perspective, it is not actually necessary for that DApp to request and process data from every block, transaction and smart contract state/event.  Generally speaking, a DApp only requires access to a very specific portion of the blockchain. For example, access to a specific smart contract's data, or access to smart contract events within a given range of blocks etc.

## Smart contract search engine

Second State has developed a smart contract search engine and Machine to Machine (M2M) API for end-users and DApps respectively. The overall design is shown in the diagram below.

![](/images/20190703-search-engine-overview-02.png)

The ethos of decentralisation is to empower the individual. In order to achieve this we need to build software products which can run on inexpensive consumer grade (store bought) hardware.

The smart contract search engine can be thought of as a middle layer between a blockchain full node and a decentralised application (DApp). This product is an essential part of the future of DApps.

> The search engine architecture is modular by design; providing flexibility and versatility, as well as scalability.

## Empowering the individual

The ethos of decentralisation is to empower the individual. This smart contract search engine achieves this in the following ways.

### Free and open source software (FOSS)

> The smart contract search engine, in-itself, is completely open source. 

In its purest form, it uses the following FOSS products/code:
* Ubuntu
* Apache2
* HTML
* CSS
* Python
* Web3py
* Elasticsearch
* LetsEncrypt

### Deployment modalities

The smart contract search engine is built using FOSS products/code which allow it to be run on consumer grade (common store bought) hardware. It can be deployed:

* privately on local disk using the file URI scheme
* publicly over HTTPS via Apache2

### Selectivity

The smart contract search engine and API can index and deliver any combination of smart contract data. For example a smart contract and DApp development team can specifically index the one and only smart contract that is relevent to their project/ product. This is done by simply uploading the ABI and Transaction address of their deployed smart contract. This contributes greatly to the performance and efficiency of the smart contract search engine, in terms of harware requirements.

## Usability - web browser

Searching via the frontend user interface can be done by simply typing text into the search box. Whilst still under heavy development, at present there are three separate public smart contract search engine websites running.

As mentioned above this product can be used to index as little as one contract; for purpose. However, in order to test the scalability on consumer grade hardware, we have begun indexing the entire Ethereum MainNet. At the time of writing, the [Ethereum MainNet Search Engine](https://ethereum.search.secondstate.io/) has:

* 853,073 contracts indexed, 
* 99 unique ABIs uploaded and 
* 10,202 contracts with available data (contracts which adhere to the previously uploaded ABIs). 

![](/images/20190703-search-engine-overview-03.png)

If you would like to see your contract indexed please [upload your ABI and Transaction Hash here](https://ethereum.search.secondstate.io/ethAbi.html).

### CyberMiles MainNet

![](/images/20190703-search-engine-overview-04.gif)

[CyberMiles MainNet Search Engine](https://cmt.search.secondstate.io/)

### CyberMiles TestNet

![](/images/20190703-search-engine-overview-05.gif)

[CyberMiles TestNet Search Engine](https://cmt-testnet.search.secondstate.io/)

### Ethereum MainNet

![](/images/20190703-search-engine-overview-06.gif)

[Ethereum MainNet Search Engine](https://ethereum.search.secondstate.io/)

## Usability - API

Below is an example of how to interact with each of the public search engine search engine APIs using Javascript (JQuery)

### CyberMiles MainNet

This example returns any of the [CyberMiles FairPlay V1](https://github.com/CyberMiles/smart_contracts/tree/master/FairPlay/v1) contracts which have "Win" in their title or description.

```
_data = {"query":{"multi_match":{"fields":["functionData.title","functionData.desc"],"query":"Win"}}}
var _dataString = JSON.stringify(_data);
response1 = $.ajax({
    url: "https://cmt.search.secondstate.io/api/es_search",
    type: "POST",
    data: _dataString,
    dataType: "json",
    contentType: "application/json",
    success: function(response) {
        console.log(response);
    },
    error: function(xhr) {
        console.log("Get items failed");
    }
});
```
Returns

```
1: {_id: "0x0d638513ff5d67da080e5c2657ed3c37adb124d34ea6db2fe0c31aa0027097ff", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
2: {_id: "0xea63dd160164b7356fb04c40f2441a2097c700d20b51e2bfd78e212f0ced4116", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
3: {_id: "0xec607e7839aedf6182cbb7becbf7534c519451e02b4b46756c1426ccb67ffedf", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
4: {_id: "0x94aa083168e823e9de477b49f4f965707a003d880acdf58bbf31595eaa7fbda2", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
5: {_id: "0x8df105256b349a6e99ad0955a49ba30499936adb845e6ca091ba54857f53d637", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
6: {_id: "0x6fcfd8f2a7340a5e8849079429d7ebb0a2c44cb7df98b010d9df09ce4b12bef5", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
7: {_id: "0x00ffd6e223b07f536338198ef96f8c765830620e1409de9b4e26283da38638e4", _index: "cmtmainnet", _score: null, _source: {…}, _type: "_doc", …}
```

### CyberMiles TestNet

This example returns one specific type of contract only. Contracts can be searched by passing in the Sha3 hash of the ABI.

```
queryMarketplaceABI = {"query":{"match":{"abiShaList": "0xca44fb82aad28d1d2c373a2934e8bc280cd418352b2c0e077d8dd715112434f1"}}}
var _dataString = JSON.stringify(queryMarketplaceABI);
$.ajax({
    url: "https://cmt-testnet.search.secondstate.io/api/es_search",
    type: "POST",
    data: _dataString,
    dataType: "json",
    contentType: "application/json",
    success: function(response) {
        console.log(response);
    },
    error: function(xhr) {
        console.log("Get items failed");
    }
});


```

```
0: {TxHash: "0x22ca832885b4e5f15c0b085875be0c1659dbcf1d65cc7c664a88340469908268", abiShaList: Array(1), blockNumber: 2089605, contractAddress: "0xBf2698e6057727EBf96c84B650738aa71C4b5127", creator: "0x9f36535b7a46850b428d6e997d817019b7fc6d69", …}
1: {TxHash: "0xbba681319e7b154fafa2e79f9db8cffdadbb3407bc6469a977b8d46a9428db58", abiShaList: Array(1), blockNumber: 2066741, contractAddress: "0xb6FA823508Dfb7e434F22c435Bb934c50d13e793", creator: "0x6b50000354fa74a4f384b9ff35108eb89d652409", …}
2: {TxHash: "0x93d12c507a6c239f8d96bf83f64b00f14c981626fa4d41f23b4d536447e4a89e", abiShaList: Array(1), blockNumber: 2089615, contractAddress: "0x1d86c6419d40CC30a39D447a5Da0A990eeD8ac0d", creator: "0x9f36535b7a46850b428d6e997d817019b7fc6d69", …}
3: {TxHash: "0x32ebb0b05ed0cb3b146290b88ba8717bb86c7ec142ad47487c83d2b1380f4b83", abiShaList: Array(1), blockNumber: 2066453, contractAddress: "0x36E419FA99848cd6a4c12E62622e6E96F70596ED", creator: "0x6b50000354fa74a4f384b9ff35108eb89d652409", …}
4: {TxHash: "0x2a5bf1205d2fd89ad44d2b5b839f7ef01b6f913c53b63de19f18a0c7a26c703d", abiShaList: Array(1), blockNumber: 2066770, contractAddress: "0xc32E7688793E0b8fd501ca89F36d33b1d6991DD4", creator: "0x6b50000354fa74a4f384b9ff35108eb89d652409", …}
```

### Ethereum MainNet

The following example queries the Ethereum MainNet for a) smart contracts that are ERC20 compliant and b) smart contracts which have a `transfer()` function.

```
queryTransferFunction = {"query":{"match":{"abiShaList": "0x7f63f9caca226af6ac1e87fee18b638da04cfbb980f202e8f17855a6d4617a69"}}}
var _dataString = JSON.stringify(queryTransferFunction);
$.ajax({
    url: "https://ethereum.search.secondstate.io/api/es_search",
    type: "POST",
    data: _dataString,
    dataType: "json",
    contentType: "application/json",
    success: function(response) {
        console.log(response.length + " contracts have the transfer() function");
    },
    error: function(xhr) {
        console.log("Get items failed");
    }
});

queryERC20 = {"query":{"match":{"abiShaList": "0x2b5710e2cf7eb7c9bd50bfac8e89070bdfed6eb58f0c26915f034595e5443286"}}}
var _dataString = JSON.stringify(queryERC20);
$.ajax({
    url: "https://ethereum.search.secondstate.io/api/es_search",
    type: "POST",
    data: _dataString,
    dataType: "json",
    contentType: "application/json",
    success: function(response) {
        console.log(response.length + " contracts are ERC20 compliant");
    },
    error: function(xhr) {
        console.log("Get items failed");
    }
});
```

The results of the above code are as follows.

```
9251 contracts have the transfer() function
8239 contracts are ERC20 compliant
```

