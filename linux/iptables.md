### 防火墙(iptables)配置
@Date 2017.09.11

> 编辑防火墙配置文件

```
vim /etc/sysconfig/iptables
```

> 配置文件示例

```
# Firewall configuration written by system-config-firewall
# Manual customization of this file is not recommended.
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:RH-Firewall-1-INPUT - [0:0]

-A INPUT -i eth0 -s 191.96.249.53 -j DROP
-A INPUT -i eth0 -s 191.96.249.54 -j DROP

-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
-A INPUT -m state --state NEW -m tcp -p tcp --dport 21 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
```

> 增加IP黑名单

```
:RH-Firewall-1-INPUT - [0:0]

-A INPUT -i eth0 -s 191.96.249.53 -j DROP
-A INPUT -i eth0 -s 191.96.249.54 -j DROP
```

> 重启防火墙

```
# service iptables restart
```