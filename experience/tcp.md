### Linux下tcp socket通信优化与问题排查
@Date 2018.12.09

#### 一. 优化

##### 1. TPC接收窗口

* 问题 : 当TCP的接收窗口队列阻塞 -> 发送方继续发 -> 接受方丢掉 -> 发送方重传 -> 网络变糟糕
* 解决 : 接收方把接收缓存的大小告诉发送方 -> 接收缓存满了 -> 发送方不能发送

```bash
# 调大接收窗口缓存大小
net.ipv4.tcp_rmem = "40960 873800 41943040"
net.core.rmem_max = 41943040
net.core.rmem_default = 873800

# 打开win scale
net.ipv4.tcp_window_scaling = 1
```

##### 2. TCP拥塞窗口

```bash
# 优化拥塞窗口的初始大小

```

##### 3. TIME_WAIT状态的回收

```bash
# 调整TIME_WAIT的回收时间
$ vi /etc/sysctl.conf

net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_fin_timeout = 30

net.core.somaxconn = 2048
net.core.rmem_default = 262144
net.core.wmem_default = 262144
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.core.somaxconn = 10000
net.core.netdev_max_backlog = 20000

net.ipv4.tcp_rmem = 7168 11264 16777216
net.ipv4.tcp_wmem = 7168 11264 16777216
net.ipv4.tcp_mem = 786432 2097152 3145728
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_fin_timeout = 15
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_max_orphans = 131072
net.ipv4.tcp_max_tw_buckets=180000
fs.file-max = 1000000
```

#### 二. 问题

##### 1. 连接超时

* 问题 : 

```bash
# 查看是否数量很多,代码不同不同请求连接过多,Syn队列溢出,丢弃连接
$ netstat -anp | grep SYN_RECV/TIME_WAIT
```

* 解决 : 更改配置文件数量，打开syncookie

---

* 问题 : 

```bash
# 前面数字一直快速增长,客户端大量请求,造成accept队列满,之后来syn包也会丢弃
# 服务端接收到syn后,先看syn队列,再看accept队列,有一个满则丢弃syn
$ netstat -s | grep -i listen
```

* 解决 : 增加accept队列长度--配置文件, net.core.somaxconn=8192. 计算公式：Len of accept queue = min（backlog + 1，somaxconn）

---

* 问题 : 客户端经常连接失败

```bash
# 四元组：源ip、目的ip、源port、目的port
# 一个客户端连接一个server只能使用固定端口范围
# TIME_WAIT状态的socket不能复用
```

* 解决 : 客户端解决,修改socket配置文件

```bash
# 调动端口使用范围
$ --net.ipv4.ip_local_port_range="1024 65535"
# 复用time_out状态端口
$ --net.ipv4.tcp_tw_reuse=1
$ net.ipv4.tcp_timestamp =1
# 加快TIME_OUT状态端口释放速度
$ net.ipv4.tcp_tw_recyle=1
$ net.ipv4.tcp_timestamp=1
```

##### 2. too many open files

```bash
# 用户程序没有调用close函数，不会自动释放--程序异常
$ netsata -anp | grep CLOSE_WAIT
```
