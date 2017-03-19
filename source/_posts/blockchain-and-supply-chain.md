---
title: Blockchain and Supply Chain |区块链在供应物流领域的应用
date: 2017-02-10 10:48:35
tags: blockchain
categories: Blockchain
comments:
toc: true
thumbnail:
banner:
---

总结供应链领域目前的痛点和区块链的现有应有方向。

<!-- more -->
### 前言
供应链是一个由物流、信息流、资金流所共同组成的,并将行业内的供应商、制造商、分销商、零售商、用户串联在一起的复杂结构。而区块链技术作为一种大规模的协作工具,天然地适合运用于供应链管理。

**痛点**
供应链由众多参与主体构成, 不同的主体之间必然存在大量的交互和协作, 而整个供应链运行过程中产生的各类信息被离散地保存在各个环节各自的系统内,信息流缺乏透明度。这会带来两类严重的问题:一是因为信息不透明、不流畅导致链条上的各参与主体难以准确了解相关事项的状况及存在的问题, 从而影响供应链的效率;二是当供应链各主体间出现纠 纷时, 举证和追责均耗时费力, 甚至在有些情况下变得不可行。随着经济 全球化的快速推进, 企业必须在越来越大的范围内拓展市场, 因此, 供应链管理中的物流环节往往表现出多区域、长时间跨度的特征, 使得假冒伪劣产品这样的难题很难彻底消除。

**基于区块链的解决思路**首先,区块链技术能使得数据在交易各方之间公开透明,从而在整个供应链条上形成一个完整且流畅的信息流,这可确保参与各方及时发现供 应链系统运行过程中存在的问题,并针对性地找到解决问题的方法,进而提升供应链管理的整体效率。其次,区块链所具有的数据不可篡改和时间戳的存在性证明的特质能很好地运用于解决供应链体系内各参与主体之间的纠纷,实现轻松举证与追责。最后,数据不可篡改与交易可追溯两大特性相结合可根除供应链内产品流转过程中的假冒伪劣问题。

### 应用方向

1. 商品流和资金流的同步  
The direction IBM and many other in finance are going is to look at permissioned ledgers connecting companies that know and (within limits) trust each other. The blockchain could allow companies to transact, resolve disputes and settle more efficiently than current practices.
2. 溯源防伪  
用户拿到选购的生鲜商品，扫码即可追溯到它产地、采购、加工、储运等完整流程中的真实信息，而且这些信息无法篡改，让消费者吃得放心;
用户购买了钻石饰品，通过其中内置的NFC芯片能了解它从生产、物流、门店甚至海关的完整记录，由于身份绝对的唯一性，一旦有非法的交易活动或是欺诈造假的行为，就会被侦测出来。此外,区块链技术也可用于药品、艺术品、收藏品、奢侈品等的溯源防伪
3. 物流：利用数字签名和公私钥加解密机制,可以充分保证 信息安全以及寄、收件人的隐私。  
在物流过程中, 利用数字签名和公私钥加解密机制, 可以充分保证 信息安全以及寄、收件人的隐私。例如, 快递交接需要双方私钥签名,每个快递员或快递点都有自己的私钥,是否签收或交付只需要查一下区块链即可。最终用户没有收到快递就不会有签收记录, 快递员无法伪造签名, 因此可杜绝快递员通过伪造签名来逃避考核的行为, 减少用户投诉, 防止 货物的冒领误领。而真正的收件人并不需要在快递单上直观展示实名制信息, 由于安全隐私有保障,所以更多人愿意接受实名制, 从而促进国家物流实名制的落实。另外,利用区块链技术,通过智能合约能够简化物流程序和大幅度提升物流的效率。


### 现有公司
- [Everledger](http://www.everledger.io/), which traces diamonds to digitally certify that they comply with the Kimberly Process, uses IBM’s cloud-based blockchain solution.
- [Provenance](https://www.provenance.org/whitepaper) 

- [云象(YunPhant)](http://www.yunphant.com/) 是超级账本（hyperledger）项目成员之一，是全球领先的区块链技术公司。云象区块链依托浙江大学智能计算与系统实验室，新加坡国立大学云象联合实验室，拥有多位计算机博士、硕士，致力于打造全球领先的企业级联盟链技术平台。
由Roger Zimmermann教授和刘振广博士带领的新加坡国立大学云象联合实验室也在着力研究新型的区块链数据库，以达到一百万次每秒的交易速度，且已取得了第一阶段成果。由陈积明教授（长江学者）和何钦铭教授带领的浙江大学云象团队在安全和智能合约方向也取得了丰硕成果。
云象与美国iBlockChain在深圳成立的区块链合资公司是前海区块链国际生态圈联盟秘书长单位，同时云象也是中关村区块链产业联盟成员
![](https://s15.postimg.org/6cx0gbh23/14898918353591.jpg)

### 供应链/贸易金融流程自动化的步骤
1.“将纸质信用证转换成可以自动执行支付的智能合约（transforming letters of credit to smart contracts with automated payments）”；

2.“将提单等纸质文件数字化，并以元数据的形式储存它们（digitizing printed documents, such as bills of lading and storing them as metadata）”；

3.“在每一步创建所有权记录（creating a record of ownership in each step）”

Payments in a supply chain can be triggered by a certain, predefined action, occurring at any point in time.
In general this works as follows: two parties agree to trade goods, and the selling party receives via his wallet a confirmation that he will be paid at a certain point if time. This guarantee is conditional, based on predefined criteria. The container with the goods contains a QR code, this is linked to a smart contract. When the goods arrive at the agreed point, and the pre-defined criteria are met, the smart contract is executed.
This triggers the transfer of ownership to the buyer and payment to the supplier is executed automatically. The great advantage is that when the structure is set up, the costs for a transaction are minimal. They are comparable to physical mail (for example €1 per letter) versus e-mail (a negligible cost per item).

### 难点和挑战

1. 政策：即使你创建了一个表面上看似严格的智能合约,也可以有一个传统的实体法律机构并不会承认交易进行的方式(例如,交易必须继续采用纸质的形式).
2. 数据源：The main challenge, is setting up technology for farmers, field workers and others to collect data and insert it onto a blockchain.
3. 迭代：It is not very easy to insert [a] new technology inside established supply chain systems because the integration challenges are not to be underestimated

### Reference Link
http://link.springer.com/chapter/10.1007/978-3-319-45348-4_19
http://ieeexplore.ieee.org/abstract/document/7538424/?part=1



