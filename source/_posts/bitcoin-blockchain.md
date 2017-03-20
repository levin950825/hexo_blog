---
title: Bitcoin Blockchain |比特币区块链理解
date: 2017-01-19 11:06:48
tags: blockchain
categories: Blockchain
comments:
toc: true
thumbnail:
banner:
---

Explain the key concepts in bitcoin and its underlying blockchain technology.

<!-- more --> 
Blockchain is a mindset。

#### Memory Keys

* Distributed (a network of nodes)
* Public
* Time-stamped
* Persistent

#### Sending Bitcoin
For A ⇒ B, A broadcasts the message to the bitcoin network (including the amount, the public key of the receiver, etc. Every node that receives it will update their copy of the ledger, and then pass along the transaction message.
Every transaction has a unique digital signature that is created based on the sender's private key, and can be verified by the receiver's public key. To claim ownership of the bitcoin, you must prove that you are the owner of the public key.

Account balances: In fact, no records of account balances are kept at all. **Ownership of the funds is verified through links to previous transactions.**

> To send 5 BTCs to Bob, Alice must reference other transactions where she received 5 or more BTCs.
> That's why it takes so long when you install the BTC wallet software the first time. Because it downloads all the transactions ever made and checks each's velidity.

Owning BTC means that there are transactions in this list that point to your name and haven't been spent.

Because people will likely lose private keys due to hard drive crashes and insufficient backups, Bitcoin currency will eventually be a deflationary one.

#### Still another security hole: Transaction Order
Nodes need to agree on transaction order.
The BTC system orders transactions by placing them in groups called blocks, and linking these blocks together in something called blockchain.
> Note that it is different from the transaction chain.

Transactions in the same block are considered happened at the same time. Any transactions not yet in a block are called unconfirmed. Any node can collect a set of unconfirmed transactions into a block and broadcast it to the rest of the network as a suggestion for what the next block in a chain should be.
Multiple nodes could generate blocks at the same time, so where are multiple versions to choose from. Each valid block must contain an answer to a very special mathematical problem.

Computer runs entire text of the block + an additional random guess through something called a cryptographic hash, **UNTIL** the output is below a certain value.
It takes 10 mins for the bitcoin network to solve a block. The first person who solves a math problem broadcasts their block and gets to have their group of transactions accepted as the next in the chain.

However, while it is rare, a block may be solved by multiple nodes at the same time, then branches occur. How to deal with this? Well, you simply build on the first block you receive and works from there. The tie gets broken when someone solves the next block. The general rule: **Immediately switch to the longest chain available**.
<img src="https://s6.postimg.org/f9rex4dsh/14829171046099.jpg" class="img-shadow" width="300px">
<img src="https://s6.postimg.org/k9ov52jf5/14829171346517.jpg" class="img-shadow" width="300px">
So the blockchain quickly stablize. 
Transactions/Facts are grouped in blocks, and there is only a single chain of blocks, replicated in the entire network.

> What is hashing?
> Hashing is also a common method of accessing data records. Consider, for example, a list of names:
John Smith
Sarah Jones
Roger Adams
To create an index, called a hash table,for these records, you would apply a formula to each name to produce a unique numeric value. So you might get something like:
1345873 John smith
3097905 Sarah Jones
4060964 Roger Adams
Then to search for the record containing Sarah Jones,you just need to reapply the formula, which directly yields the index key to the record. This is much more efficient than searching through all the records till the matching record is found.

#### End of Chain Insecurity
<img src="https://s6.postimg.org/7wc0y5tqp/14829172517203.jpg"class="img-shadow" width="200px">
The fact that there is some ambiguity in the end of the chain

A double-spent attack in this block chain system:
<img src="https://s6.postimg.org/z1ozinzyp/14898931224923.jpg" class="img-shadow" width="450px">

A⇒B will be threw back to the unconfirmed pool, but because A⇒A was confirmed with the same input, it will invalid A⇒B. 

Well, **Math puzzle in each block actually prevent this**, because Alice cannot change one block from the middle. The attach will only be possible if Alice owns more than half of the nodes in the world.
<img src="https://s6.postimg.org/d3siovky9/14898931329803.jpg" class="img-shadow" width="450px">
So the system is only vulnerable to a double spend attach near the end of the chain, which is why it's recommended to wait several blocks before considering received money final. 


#### Mining
The process of looking for blocks is called mining. This is because, just like gold mining, block mining brings an economical reward - some form of money. That’s the reason why people who run nodes in a blockchain are also called miners.
> Note: By default, a node doesn’t mine - it just receives blocks mined by other nodes. It’s a voluntary process to turn a node into a miner node.

Every second, each miner node in a blockchain tests thousands of random strings to try and form a new block. So running a miner in the blockchain pumps a huge amount of computer resources (storage and CPU). That’s why you must pay to store facts in a blockchain. Reading facts, on the other hand, is free: you just need to run your own node, and you’ll recuperate the entire history of facts issued by all the other nodes. So to summarize:

* Reading data is free
* Adding facts costs a small fee
* Mining a block brings in the money of all the fees of the facts included in the block

每个矿工会把十分钟内所有unconfirmed transactions拿过来, 两两pair来产生hash,一层层两两pair产生一个最终hash.

简而言之，比特币区块链的挖矿流程：
广播比特币网络中的每一笔交易，使每个参与者（指矿工）都记录下这笔交易。
每个参与者接收到交易信息后，都要将该笔交易盖上时戳，收入区块。
由于每个矿工都做了工作，谁赢了获得奖励呢？此时参与者们要通过一个计算游戏，谁能最快解出SHA256运算的值，谁就将赢得打包区块的权利，并获得系统的25个比特币奖励。这个数量的设定是每四年减半。（比特币已经到了第七个年头了）
获得记账权的矿工将向全网广播这十分钟内区盖了时戳的交易，其他参与者将核对这些账目
当其他参与者都确认无误后，该区块就确认合法，就进入了下一轮的区块争夺战。多个区块逐渐形成区块链。




