title: 初始化节点工作 v0.9.2
date: 2014-06-15 01:47:42
tags:
---

# 开发工作

进展：

* bitcoind 0.9.2
  * patch: [open-nodes.org_hub-v0.9.2.patch](https://gist.github.com/bitkevin/a1b4a556fffce6fc527a)
    * 该补丁增加Hub模式
    * 合并补丁：OpenSSL在部分OS发行下ECDSA算法分支不全的问题
* bitcoin-seeder
  * [add --quiet, fix stats output](https://github.com/open-nodes/bitcoin-seeder/commit/d6d3bfcd3ed20756d5524a1e59b1afa2206d4d21)
    * 修复一些小错误，新增安静模式
    
服务：

* 新增比特币节点：`node01.open-nodes.org`
* 新增节点种子DNS：`seeds.bitcoin.open-nodes.org`

# 安装笔记

## Init Server

* CentOS 6.X

### add repos

* EPEL

```
rpm -ivh http://mirror-fpt-telecom.fpt.net/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
```

* Atomic

```
wget -q -O - http://www.atomicorp.com/installers/atomic | sh
rpm --import https://www.atomicorp.com/RPM-GPG-KEY.atomicorp.txt
```

### oh-my-zsh

```
yum install -y git zsh 
wget --no-check-certificate http://install.ohmyz.sh -O - | sh
```

### prepare 

```
yum install -y bind-utils denyhosts system-config-firewall-tui daemontools patch

yum install -y make gcc gcc-c++ zlib-devel boost boost-devel automake openssl openssl-devel

```



## Install bitcoin-seeder

```
mkdir -p ~/source; cd ~/source
git clone https://github.com/open-nodes/bitcoin-seeder.git
cd bitcoin-seeder
make -j4
```

### supervise for bitcoin-seeder

```
mkdir ~/supervise_bitcoin_seeder
cd ~/supervise_bitcoin_seeder
touch run
chmod +x run
```

* `cat run`

```
#! /bin/sh

cd ~/source/bitcoin-seeder
./dnsseed -h seeds.bitcoin.open-nodes.org -n ns01.bitcoin.open-nodes.org
```

* start bitcoin_seeder

```
cd ~/
nohup supervise supervise_bitcoin_seeder/ &
```


## Install bitcoind

### build bitcoind

```
mkdir -p ~/source; cd ~/source

wget https://github.com/bitcoin/bitcoin/archive/v0.9.2.tar.gz
tar zxvf v0.9.2; cd bitcoin-0.9.2

wget https://gist.githubusercontent.com/bitkevin/a1b4a556fffce6fc527a/raw/dfe6e9f42e0a0b8bf767164cb2ebafa1322be7df/open-nodes.org_hub-v0.9.2.patch
patch -p1 < open-nodes.org_hub-v0.9.2.patch

./autogen.sh
./configure --without-miniupnpc --disable-wallet
make -j4
strip src/bitcoind
cp src/bitcoind /usr/local/bin/bitcoind-0.9.2
cd /usr/local/bin
ln -s ./bitcoind-0.9.2 bitcoind
cd ~/source
```

#### setup bitcoind

`/d1/bitcoind` is the home dir for bitcoind.

```
mkdir -p /d1/bitcoind/
chown bitcoind.bitcoind /d1/bitcoind

useradd bitcoind
su - bitcoind
```

* `cat /d1/bitcoind/bitcoin.conf`

```
rpcuser=bitcoinrpc
rpcpassword=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
rpcthreads=32
rpcallowip=*

datadir=/d1/bitcoind
txindex=true

limitdownloadblocks=720

maxconnections=4096
outboundconnections=3072
```
_Don't forget to change **rpcpassword**_.

* supervise for bitcoind

```
mkdir ~/supervise_bitcoind
cd ~/supervise_bitcoind
touch run
chmod +x run
```

* `cat run`

```
#! /bin/sh

SROOT=$(cd $(dirname "$0"); pwd)
cd $SROOT

bitcoind -conf=/d1/bitcoind/bitcoin.conf
```

* start bitcoind

```
cd ~/
nohup supervise supervise_bitcoind/ &
```


## Firewall

* `cat /etc/sysconfig/iptables`, seems like below:

```
# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m udp -p udp --dport 53 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8332 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 8333 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```

restart iptables


```
/etc/init.d/iptables restart
chkconfig iptables on
```

Ipv6's settings is the same as ipv4.