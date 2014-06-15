title: Bitcoin-Seeder开发与部署
date: 2014-06-15 16:25:06
tags:
---

What is _bitcoin-seeder_？

Bitcoin-seeder is a crawler for the Bitcoin network, which exposes a list of reliable nodes via a built-in DNS server.

### Progress
 
* 修复一些小错误，新增安静模式
  * [add --quiet, fix stats output](https://github.com/open-nodes/bitcoin-seeder/commit/d6d3bfcd3ed20756d5524a1e59b1afa2206d4d21)
* 添加DNS种子

需最新代码，请直接fork open-nodes的库：[https://github.com/open-nodes/bitcoin-seeder](https://github.com/open-nodes/bitcoin-seeder)。

### DNS Seeds节点

新加节点`seeds.bitcoin.open-nodes.org`，已对外提供服务：

```
$ dig seeds.bitcoin.open-nodes.org @8.8.8.8
;; QUESTION SECTION:
;seeds.bitcoin.open-nodes.org.	IN	A
;; ANSWER SECTION:
seeds.bitcoin.open-nodes.org. 59 IN	A	68.192.97.155
seeds.bitcoin.open-nodes.org. 59 IN	A	207.229.74.8
seeds.bitcoin.open-nodes.org. 59 IN	A	173.13.182.10
seeds.bitcoin.open-nodes.org. 59 IN	A	199.91.66.218
seeds.bitcoin.open-nodes.org. 59 IN	A	107.170.170.56
seeds.bitcoin.open-nodes.org. 59 IN	A	76.27.19.151
seeds.bitcoin.open-nodes.org. 59 IN	A	71.184.157.228
seeds.bitcoin.open-nodes.org. 59 IN	A	82.7.132.159
seeds.bitcoin.open-nodes.org. 59 IN	A	123.202.95.155
seeds.bitcoin.open-nodes.org. 59 IN	A	188.226.199.191
seeds.bitcoin.open-nodes.org. 59 IN	A	89.155.84.38
seeds.bitcoin.open-nodes.org. 59 IN	A	89.178.144.93
seeds.bitcoin.open-nodes.org. 59 IN	A	94.21.29.112
seeds.bitcoin.open-nodes.org. 59 IN	A	50.23.69.147
seeds.bitcoin.open-nodes.org. 59 IN	A	85.214.90.1
seeds.bitcoin.open-nodes.org. 59 IN	A	195.154.106.147
seeds.bitcoin.open-nodes.org. 59 IN	A	188.40.130.146
seeds.bitcoin.open-nodes.org. 59 IN	A	24.146.176.168
seeds.bitcoin.open-nodes.org. 59 IN	A	80.229.152.39
seeds.bitcoin.open-nodes.org. 59 IN	A	96.246.183.224
seeds.bitcoin.open-nodes.org. 59 IN	A	119.186.235.61
seeds.bitcoin.open-nodes.org. 59 IN	A	108.180.208.106
seeds.bitcoin.open-nodes.org. 59 IN	A	92.113.73.202
seeds.bitcoin.open-nodes.org. 59 IN	A	79.112.106.192
seeds.bitcoin.open-nodes.org. 59 IN	A	23.226.177.19
seeds.bitcoin.open-nodes.org. 59 IN	A	96.49.96.90



$ dig -t ns seeds.bitcoin.open-nodes.org @8.8.8.8
;; QUESTION SECTION:
;seeds.bitcoin.open-nodes.org.	IN	NS
;; ANSWER SECTION:
seeds.bitcoin.open-nodes.org. 7996 IN	NS	ns01.bitcoin.open-nodes.org.
```