title: '超级节点部署文档-bitcoind-v0.10.0-ubuntu_14.04_LTS'
date: 2015-02-28 17:00:08
tags:
---

## 软件需求

* OS: Ubuntu 14.04 LTS
* Bitcoind: v0.10.0

## 安装

* **bitcoin**安装目录为: `/root/.bitcoin`
* root身份执行本安装过程，若非root，请注意`sodo`切换
* `supervise`模式运行bitcoind

### 安装文档&命令

具体安装细节均在shell文件中：[ubuntu-14.04-LTS-bitcoind-0.10.0.sh](https://raw.githubusercontent.com/open-nodes/InstallStaff/master/scripts/v0.10.0/ubuntu-14.04-LTS-bitcoind-0.10.0.sh)

* 启动命令：`nohup supervise /root/supervise_bitcoind/ &`
* 停止命令：需要先终止`supervise`进程，再停止bitcoind: `bitcoin-cli stop`
* 终止命令: **kill \`pgrep bitcoind\`**，若进程卡死，则可强制退出: **kill -9 \`pgrep bitcoind\`**

### 首次安装

* 若是全新ubuntu 14.04 LTS，可以直接运行下面的安装shell
  * `wget --no-check-certificate https://raw.githubusercontent.com/open-nodes/InstallStaff/master/scripts/v0.10.0/ubuntu-14.04-LTS-bitcoind-0.10.0.sh -O - | sh`

也可按照脚本手动执行上述安装命令。执行上述脚本后，检测是否运行：`bitcoin-cli getinfo`。

### 升级安装

若从旧版本升级至`v0.10.x`，仅需要重新编译bitcoind即可：

````bash
# 下载v0.10.0源码
cd ~/
mkdir -p source
wget --no-check-certificate https://github.com/bitcoin/bitcoin/archive/v0.10.0.tar.gz -O v0.10.0.tar.gz
tar zxf v0.10.0.tar.gz
cd bitcoin-0.10.0

# 打补丁，开启超级节点模式
wget --no-check-certificate https://raw.githubusercontent.com/open-nodes/InstallStaff/master/scripts/v0.10.0/open-nodes.org_hub-v0.10.0.patch -O open-nodes.org_hub-v0.10.0.patch
patch -p1 < open-nodes.org_hub-v0.10.0.patch

# 编译
./autogen.sh
./configure --without-miniupnpc --disable-wallet
make -j4

# 覆盖旧版bitcoind
strip src/bitcoind
cp src/bitcoind /usr/bin/bitcoind-0.10.0
cp src/bitcoin-cli /usr/bin/bitcoind-cli
chmod 755 /usr/bin/bitcoind-0.10.0
cd /usr/bin
ln -s ./bitcoind-0.10.0 bitcoind
cd ~/
````

#### 升级注意事项

* rpcallowip不兼容更新，请注意替换。详情如下表格：

0.9.x and before	 | 0.10.x
------------------|---------------
-rpcallowip=192.168.1.1	 | -rpcallowip=192.168.1.1 (unchanged)
-rpcallowip=192.168.1.*	 | -rpcallowip=192.168.1.0/24
-rpcallowip=192.168.* | -rpcallowip=192.168.0.0/16
-rpcallowip=* (dangerous!) | -rpcallowip=::/0 (still dangerous!)

* v0.10.x的区块数据与v0.9.x之前的将不再兼容，也就是说v0.10.0的数据文件无法用于老版本，一旦升级则无法降级至v0.9.x


### 其他

如果任何问题，请提出issue：[https://github.com/open-nodes/InstallStaff/issues](https://github.com/open-nodes/InstallStaff/issues)

欢迎Pull Request：

* 其他操作的系统的安装文档
* 安装过程的bug修复
