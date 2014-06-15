title: 基于bitcoind-v0.9.2开发工作
date: 2014-06-15 16:26:52
tags:
---

* base on version v0.9.2

### Progress

#### patch: [open-nodes.org_hub-v0.9.2.patch](https://gist.github.com/bitkevin/a1b4a556fffce6fc527a)
* 增加Hub模式
  * 新增 `-limitdownloadblocks`，表示可以从该节点下载多少高度以内的块，通常可以设置为2016，可显著减少带宽消耗
  * 新增 `-outboundconnections`，对外最大连接数，去掉硬编码限制（8个），通常4M对称带宽可以设置为64或128，需同时增大参数`-maxconnections`
* 合并补丁：OpenSSL在部分OS发行下ECDSA算法分支不全的问题
* 添加DNS种子
