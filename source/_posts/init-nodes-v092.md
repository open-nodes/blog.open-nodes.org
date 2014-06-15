title: 初始化节点工作 v0.9.2
date: 2014-06-15 01:47:42
tags:
---

* OS: CentOS 6.X

## Init Server

{% gist bitkevin/2d6f2c74ec1ae9daa79e %}

## Install bitcoin-seeder

{% gist bitkevin/ec1e74c3a67073eb9056 %}

## Install bitcoind

{% gist bitkevin/f831afd413946d32e965 %}

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