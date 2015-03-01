title: 节点服务器的选型
date: 2015-03-01 10:59:36
tags:
---

## 选型

下面数据是open-nodes的节点长期运行的经验结论，给出的是最低配要求，否则节点将无法长期稳定运行。

* 1 ~ 200连接数：1核CPU，4G内存，5MBits带宽
* 200 ~ 1000连接数：2核CPU，8G内存，10MBits带宽

其对内存要求最高，比较耗费内存；带宽需质量优质，延迟小，ping值稳定；CPU占用一般，通常至强系列1~2核即足够。

尝试4G内存运行500连接数的节点，测试结果是可以运行，但很难长期稳定。国内大多数IDC服务商都是机械磁盘，性能比SSD固态盘低一些。固态盘的机器内存不够时，数据打到swap分区上依然达到可以接收的性能，而机械盘服务器就不太行了，机械盘服务器的内存应该还要大一些。

## 实例数据

下面是一台长时间稳定运行的比特币节点(托管服务商是Linode, Plan: 8GB)，当前连接数是`872`。

```
$ bitcoin-cli getinfo
{
    "version" : 90201,
    "protocolversion" : 70002,
    "blocks" : 345628,
    "timeoffset" : 0,
    "connections" : 872,
    "proxy" : "",
    "difficulty" : 46684376316.86029053,
    "testnet" : false,
    "relayfee" : 0.00001000,
    "errors" : ""
}
```

内存占用`5.870g`，CPU负载`0.3`。

```
top - 02:45:12 up 64 days, 22:27,  1 user,  load average: 0.27, 0.29, 0.31
Tasks: 107 total,   2 running, 105 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.0 us,  1.6 sy,  4.4 ni, 93.8 id,  0.0 wa,  0.0 hi,  0.1 si,  0.2 st
MiB Mem:  8018.238 total, 7748.098 used,  270.141 free,   31.430 buffers
MiB Swap: 1023.996 total,  850.188 used,  173.809 free. 1439.020 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
 3254 root      25   5 8451.9m 5.870g  12.8m S  34.9 75.0  24632:38 bitcoind
```
 
 ## 近30天的系统负载图
 
 ![linode graphs last 30 days](https://cloud.githubusercontent.com/assets/514951/6429325/c16b3a52-c004-11e4-8350-ef0647d73e3d.png)