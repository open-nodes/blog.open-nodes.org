title: '超级节点部署文档-bitcoind-v0.9.2.1-ubuntu_14.04_LTS'
date: 2014-09-24 11:20:08
tags:
---

### 软件需求

* OS: Ubuntu 14.04 LTS
* Bitcoind: v0.9.2.1


### 事前必读

* **bitcoin**安装目录为: `/root/.bitcoin`
* root身份执行本安装过程，若非root，请注意`sodo`切换
* `supervise`模式运行bitcoind
* 若是全新ubuntu 14.04 LTS，可以直接运行下面的安装shell
  * `wget --no-check-certificate https://raw.githubusercontent.com/open-nodes/InstallStaff/master/scripts/v0.9.2.1/ubuntu-14.04-LTS-bitcoind-0.9.2.1.sh -O - | sh`
  * 若已有部分服务，请根据需要选择执行
* 跳高句柄限制数后，需要退出登录并再次登入

### 安装文档&命令

https://github.com/open-nodes/InstallStaff/blob/master/scripts/v0.9.2.1/ubuntu-14.04-LTS-bitcoind-0.9.2.1.sh

完成上述过程后，退出并重新登录。执行命令：`nohup supervise /root/supervise_bitcoind/ &`，以启动bitcoind。

首次同步若很慢，可能需要重启bitcoind，命令: **kill \`pgrep bitcoind\`**

### 其他

如果任何问题，请提出issue：[https://github.com/open-nodes/InstallStaff/issues](https://github.com/open-nodes/InstallStaff/issues)

欢迎Pull Request：

* 其他操作的系统的安装文档
* 安装过程的bug修复