---
title: Understand Blockchain |理解区块链
date: 2017-01-25 21:38:08
categories:
- Blockchain
tags: ##标签，多标签格式为 [tag1,tag2,...]
- blockchain
keywords: ##文章关键词，多个的格式为 keyword1,keywords2,...
description: ##文章描述, 主要是SEO用
toc: true
---

简单介绍下区块链(blockchain)的关键概念

<!--more -->
区块链科技来源于比特币，如果你还不是很了解比特币，比较建议先去查询下比特币的科普视频。然后再可以先去看下我之前的一个学习笔记[Bitcoin Blockchain |比特币区块链理解](http://blog.peiyingchi.com/2017/01/19/bitcoin-blockchain/)

### 基本原理
基本概念包括：

* 交易（Transaction）：一次操作，导致账本状态的一次改变，如添加一条记录；
* 区块（Block）：记录一段时间内发生的交易和状态结果，是对当前账本状态的一次共识；
* 链（Chain）：由一个个区块按照发生顺序串联而成，是整个状态变化的日志记录。

如果把区块链作为一个状态机，则每次交易就是试图改变一次状态，而每次共识生成的区块，就是参与者对于区块中所有交易内容导致状态改变的结果进行确认。
<img src="https://s6.postimg.org/46ce59hw1/14832797016611.png" class="img-shadow" width="500px">


节点需要将新区块的前一 个区块的哈希值、当前时间戳、一段时间内发生的有效交易及其梅克尔树 根值等内容打包成一个区块,向全网广播。

<img src="https://s6.postimg.org/859lo36j5/14834176626345.jpg" class="img-shadow" width="550px">

<img src="https://s6.postimg.org/j69c6uo5t/14834178266616.jpg" class="img-shadow" width="550px">

Blockchain 2.0 的典型特征如下：

1. 智能合约（smart contract）：区块链系统中的应用，是已编码的、可自动运行的业务逻辑，通常有自己的代币和专用开发语言
2. DAPP:包含用户界面的应用，包括但不限于各种加密货币，如以太坊钱包
3. 虚拟机：用于执行智能合约编译后的代码。

>智能合约程序不只是一个可以自动执行的计算机程序：它自己就是一个系统参与者。它对接收到的信息进行回应，它可以接收和储存价值，也可以向外发送信息和价值。 这个程序就像一个可以被信任的人，可以临时保管资产，总是按照事先的规则执行操作。

### 常用缩略语

| 缩略语  | 完整                   |
| ---- | -------------------- |
| PoW  | 工作量证明(Proof of Work) |
|PoS|权益证明(Proof of Stake)
|DPoS|股份授权证明(Delegate Proof of Stake)
|PBFT|实用拜占庭容错(Practical Byzantine Fault Tolerance)
|P2P|点对点(Peer to Peer)
|DAPP|分布式应用(Decentralized Application)
|KYC|客户识别(Know Your Customer)
|RSA|RSA加密算法(RSA Algorithm)
|ECC|椭圆加密算法(Elliptic Curve Cryptography)
|BaaS|区块链即服务(Blockchain as a Service)|

### 非对称加密算法
非对称加密算法是指使用公私钥对数据存储和传输进行加密和解 密。公钥可公开发布,用于发送方加密要发送的信息,私钥用于接收方 解密接收到的加密内容。公私钥对计算时间较长,主要用于加密较少的 数据。常用的非对称加密算法有RSA和ECC。非对称加密算法的过程如图 2-3所示。区块链正是使用非对称加密的公私钥对来构建节点间信任的。
<img src="https://s6.postimg.org/cd4dwu7yp/14834174393360.jpg" class="img-shadow" >

### POW v.s. POS
#### POW: Proof of Work | 工作量证明
一方（通常称为证明人）出示计算结果，这个结果众所周知是很难计算的但却很容易验证的。通过验证这个结果，任何人都能够确认证明人执行了一定量的计算工作量来产生这个结果

#### POS：Proof of Stake | 股权证明
 
这又是什么意思呢？简单来说，就是一个根据你持有货币的量和时间，给你发利息的一个制度，在股权证明POS模式下，有一个名词叫币龄，每个币每天产生1币龄，比如你持有100个币，总共持有了30天，那么，此时你的币龄就为3000，这个时候，如果你发现了一个POS区块，你的币龄就会被清空为0。你每被清空365币龄，你将会从区块中获得0.05个币的利息(可理解为年利率5%)，那么在这个案例中，利息 = 3000 * 5% / 365 = 0.41个币，这下就很有意思了，持币有利息，非常好！（需要注意的是，5%的年利率仅仅是小编举例，并非每个POS模式的币种都是5%，比如点点币PPCoin就是1%年利率）

### 分类

根据参与者的不同，可以分为公开（Public）链、联盟（Consortium）链和私有（Private）链。
公开链，顾名思义，任何人都可以参与使用和维护，典型的如比特币区块链，信息是完全公开的。
如果引入许可机制，包括私有链和联盟链两种。
私有链，则是集中管理者进行限制，只能得到内部少数人可以使用，信息不公开。
联盟链则介于两者之间，由若干组织一起合作维护一条区块链，该区块链的使用必须是有权限的管理，相关信息会得到保护，典型如银联组织。
目前来看，公开链将会更多的吸引社区和媒体的眼球，但更多的商业价值应该在联盟链和私有链上。
根据使用目的和场景的不同，又可以分为以数字货币为目的的货币链，以记录产权为目的的产权链，以众筹为目的的众筹链等。
<img src="https://s6.postimg.org/c4beknkk1/14834271525238.jpg" class="img-shadow" >


### 误区

目前，对区块链的认识还存在不少误区。
首先，**区块链不是数据库**。虽然区块链也可以用来存储数据，但它要解决的问题是多方的互信问题。单纯从存储数据角度，它的效率可能不高，笔者也不推荐把大量的原始数据放到区块链上。
其次，**区块链不是要颠覆现有技术**。作为基于多项已有技术而出现的新事物，区块链跟现有技术的关系是一脉相承的，在解决多方合作和可信处理上多走了一步，但并不意味着它将彻底颠覆已有的商业模式。很长一段时间里，区块链的适用场景仍需摸索，跟已有系统必然是合作共存的关系。


### 应用场景
With smart contracts, for the first time, you can build business process automation software that cuts across different stakeholders, and can be relied on by all. Blockchain technology can get everyone looking at the same core data set.

未来几年内，可能深入应用区块链的场景将包括：

* 金融服务：主要是降低交易成本，减少跨组织交易风险等。该领域的区块链应用将最快成熟起来，银行和金融交易机构将是主力推动者。
* 征信和权属管理：这是大型社交平台和保险公司都梦寐以求的，目前还缺乏足够的数据来源、可靠的平台支持和有效的数据分析和管理。该领域创业的门槛极高，需要自上而下的推动。
* 资源共享：airbnb 为代表的公司将欢迎这类应用，极大降低管理成本。这个领域创业门槛低，主题集中，会受到投资热捧。
* 投资管理：无论公募还是私募基金，都可以应用区块链技术降低管理成本和管控风险。虽然有 DAO 这样的试水，谨慎认为该领域的需求还未成熟。
* 物联网与供应链：物联网是很适合的一个领域，短期内会有大量应用出现，特别是租赁、物流等特定场景。但物联网自身的发展局限将导致短期内较难出现规模应用

<img src="https://s6.postimg.org/4tbauses1/14839471054639.png" width="650px">

<img src="https://s6.postimg.org/8e76e0jbl/14845303553536.jpg" width="650px">
<div style="text-align: right">  *(click on the picture to enlarge)* </div>

### 未来区块链生态
![](https://s6.postimg.org/v0h34hmfl/14835824591159.jpg)

### 整体设计
![](https://s30.postimg.org/ssvnxgtr5/14898915018403.jpg)

## 参考资料：
http://book.8btc.com/books/1/digital_giant_chain/_book/
http://8btc.com/doc-view-182.html


