#### Docker容器常见问题
@Date 2018.09.20


#### 一. Docker容器内部无法访问宿主机网络(No Route to host)

##### 解决办法

* 关闭防火墙(局域网内推荐)

```bash
# centos 7
systemctl stop firewalld
```

* 在防火墙上开放指定端口

```bash
# 添加端口
firewall-cmd --zone=public --add-port=7001/tcp --permanent
firewall-cmd --reload
```

#### 二. Docker启动报错 : iptables failed

##### 错误提示

```bash
Error response from daemon: driver failed programming external connectivity on endpoint gloomy_kirch : iptables failed: iptables --wait -t nat -A DOCKER -p tcp -d 0/0 --dport 32810 -j DNAT --to-destination 172.17.0.2:80 ! -i docker0: iptables: No chain/target/match by that name.
```

##### 原因分析

在Docker Daemon服务启动之后, 修改了防火墙配置(修改/关闭等), 此时启动Docker容器会造成防火墙网络配置有问题

##### 解决办法

* 重启

```bash
# 先重启防火墙
systemctl restart firewalld
# 再重启Docker 服务
systemctl restart docker
```

#### 三. Docker挂载的目录, 在容器内无读写权限(Permission denied)

##### 环境&现象

CentOS 7.4 环境, 启动Docker时volume容器和宿主机的挂载目录, 但是在容器内部无权限对此目录进行操作

```bash
# 错误现象
ls: cannot open directory '.': Permission denied
```

##### 解决办法

* 关闭CentOS7中安全模块selinux

```bash
# 临时关闭selinux
setenforce 0
```

* 运行容器时, 给容器增加特权

```bash
docker run -i -t -v /soft:/soft --privileged=true 637fe9ea94f0 /bin/bash
```

