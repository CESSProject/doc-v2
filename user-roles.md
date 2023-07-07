# 👥 User Roles

## 共识节点

共识节点是CESS链中参与共识选举、打包区块的重要角色，所有共识节点都具备以下特性：
记录并存储所有交易结果及状态变化
各节点之间分散通信组成一个对等网络
保证链数据安全并且持续增长的共识算法
为区块计算哈希以及用于签名和验证交易的密码学算法
共识节点采用polkadot开源的substrate框架进行开发，具备天然的优势。

### cess-node

cess-node是CESS网络的基本组件之一，是基于substrate框架进行编写的区块链，编译出的二进制程序。
同时如果是在运行TEE Worker的情况下，cess-node是全节点模式运行，且会被选举成为验证人，承担出块和验证区块的任务。

### TEE Worker

TEE Worker主要工作是Marking Data以及生成随机文件给到存储节点存储，所有工作在TEE中完成，具有无法篡改以及可验证的特点，能够很好的保证数据的真实性。
TEE Worker基于Gramine库开发，目前只对intel系列的芯片提供支持。
TEE Worker是与共识节点绑定，使用共识节点的账户签名注册交易后才能进行工作。它对硬件的要求较高，需要硬件上支持TEE功能，并且配备强大的cpu能够让TEE Worker快速完成Marking Data工作，从而快速积累工作量来赚取更多收益。

参考：共识节点的奖励与惩罚机制

## 存储节点

存储节点是CESS网络分布式存储系统的重要角色，所有节点都是对等的，通过使用p2p（基于go-libp2p开发）通信技术组成一个遍布全球的分布式存储网络。
存储节点主要功能包括提供存储空间、存储数据、提供下载，计算数据证明等。存储节点可以控制使用哪块磁盘以及最多使用多少空间来为CESS网络服务，提供的空间越大获得的收益比例越大，占用空间一致的情况下，存储数据获取的收益大于提供空间获取的收益。
存储节点提供数据下载不仅能获得收益还能提高自己的信誉，信誉越高则存储数据的几率越大，获取高收益的概率也会越大。

参考：存储节点的奖励与惩罚机制

CESS网络鼓励存储节点提供一个容量较大（4TiB及以上）的磁盘，推荐使用SSD，推荐网络带宽不低于2M，这样可以更快以及更大比例的赚取收益。

参考：存储节点操作手册

### 链客户端组件

链客户端组件基于社区开源的go-substrate-rpc-client实现，描述了存储节点如何与区块链节点交互，具有查看链状态、交易、监听事件等功能。

### 数据库组件

数据库组件采用的高性能的levelDB，基于开源的goleveldb实现，该组件主要用来缓存链上元数据，起到加速读取的功能。

### p2p通信组件

p2p通信组件基于go-libp2p开发，该组件实现了存储节点之间组成p2p网络，详情参考CESSProject/p2p-go。

- ref: https://app.gitbook.com/o/FsIgz09ubmGQUNERT1yp/s/VlV8XTe49VsbnMG4jMES/learn/network-architecture/jiao-se-roles

Cover five types of user (from the least involved to the most involved):

1. community member / token holder
2. storage (dApp) users
3. dApp developers
4. validator: storage node
5. validator: consensus node
