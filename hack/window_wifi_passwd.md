### Windows下netsh wlan命令
@Date 2017.04.18

> windows系统下,CMD命令获取已连接的WIFI信息(含密码)

```
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do  @echo %j | findstr -i -v echo | netsh wlan show profiles %j key=clear
```

> 详细分析

1. 列出当前系统用户已经连接过的WIFI的配置文件

* CMD :

```
netsh wlan show profiles
```

* OUTPUT :

```
接口 WLAN 上的配置文件:


组策略配置文件(只读)
---------------------------------
    <无>

用户配置文件
-------------
    所有用户配置文件 : Dlink802
    所有用户配置文件 : TP-LINK_4EDC
```

2. 循环遍历上面取出来的WIFI连接名字

* CMD :

```
for /f "skip=9 tokens=1,2 delims=:" %i in ('netsh wlan show profiles') do  @echo %j | findstr -i -v echo
```

* OUTPUT :

```
 Dlink802
 TP-LINK_4EDC
```

3. 根据第二条列出来的WIFI链接名字,查找对应的WIFI配置文件

* CMD : 

```
netsh wlan show profiles %j key=clear
```

* OUTPUT :

```
上面CMD命令中%j替换成Dlink802,就是寻找当前系统下的Dlink802对应的WIFI信息,j%是一个变量,接受第二条命令的输出结果替换.

接口 WLAN 上的配置文件 Dlink802:
=======================================================================

已应用: 所有用户配置文件

配置文件信息
-------------------
    版本                   : 1
    类型                   : 无线局域网
    名称                   : Dlink802
    控制选项               :
        连接模式           : 自动连接
        网络广播           : 只在网络广播时连接
        AutoSwitch         : 请勿切换到其他网络
        MAC 随机化: 禁用

连接设置
---------------------
    SSID 数目              : 1
    SSID 名称              :“Dlink802”
    网络类型               : 结构
    无线电类型             : [ 任何无线电类型 ]
    供应商扩展名           : 不存在

安全设置
-----------------
    身份验证         : WPA2 - 个人
    密码                 : CCMP
    身份验证         : WPA2 - 个人
    密码                 : 未知
    安全密钥               : 存在
    关键内容            : MIMAwangji2015@@

费用设置
-------------
    费用                : 无限制
    阻塞                : 否
    接近数据限制        : 否
    过量数据限制        : 否
    漫游                : 否
    费用来源            : 默认
```
