### MACOSX Apache ab压力测试
@Date 2018.12.24

#### 一. 介绍

* ab是apachebench命令的缩写, 默认mac下都已经安装了, 但是默认版本会有几个问题, 故写此文章进行总结

#### 二. 问题

##### 1. apr_socket_recv: Connection reset by peer (xx)

* 原因 : 此问题是apache的bug, 在高版本上已经解决, 故我们需要在mac上升级ab的版本

* 解决 : 
    1. 现在较高版本的apache, 下载地址(http://mirrors.hust.edu.cn/apache//httpd/httpd-2.4.34.tar.gz)
    2. 现在apache依赖的包,分别如下
        1. APR : http://mirrors.shu.edu.cn/apache//apr/apr-1.6.5.tar.gz  
        2. APR-Util : http://mirrors.shu.edu.cn/apache//apr/apr-util-1.6.1.tar.gz 
        3. pcre :  https://ftp.pcre.org/pub/pcre/pcre-8.00.tar.gz   
    3. 安装(prefix是指向想要安装的路径)
        1. APR : 
            * 下载的APR压缩包解压缩, 进入APR目录
            * 执行 ./configure –prefix=/usr/local/apr
            * 使用sudo权限执行 make 
            * 使用sudo权限执行 make install 
        2. APR-Util
            * 同样进入解压后的apr-util目录
            * 执行 ./configure –prefix=/usr/local/apr-util -with-apr=/usr/local/apr/ (此命令依赖apr)
            * 使用sudo权限执行 make 
            * 使用sudo权限执行 make install 
        3. pcre
            * 同样进入解压后的pcre目录
            * 执行 ./configure –prefix=/usr/local/pcre
            * 使用sudo权限执行 make 
            * 使用sudo权限执行 make install
        4. 安装ab
            * 进入解压后的httpd-2.3.34目录
            * sudo权限创建/usr/local/httpd/目录
            * 依赖上面安装好的各种包, 执行以下命令 ./configure –prefix=/usr/local/httpd/ -with-apr=/usr/local/apr -with-apr-util=/usr/local/apr-util/ -with-pcre=/usr/local/pcre/
            * 使用sudo权限执行 make 
    4. 替换Mac默认的ab
        1. 在上述3.4中, httpd目录下. 执行以下命令 sudo cp support/ab /usr/sbin .将编译后的最新ab命令copy到系统中.
        2. 此时会遇到最新MacOSX中权限问题, 请看下文[权限异常 Operation not permitted]()
            
##### 2. apr_poll: The timeout specified has expired (70007)

* 原因 : 高版本的ab需要增加-s参数, 执行超时时间, 否则会30s则超时

* 解决 : 
    * ab --help 查询命令帮助
    * -s timeout 使用ab命令时,增加-s参数

##### 3. MacOSX中/usr/sbin报错, Operation not permitted

* 原因 : 系统增加了Rootless机制机制,防止恶意程序执行高权限命令

* 解决 : 
    * 重启Mac系统, 在重启过程中一直按住Command+R, 进入恢复模式.
    * 在恢复模式中无需点击其它操作, 在菜单栏中打开Terminal, 执行以下命令 csrutil disable. 解锁dtrace限制
            
#### 三. ab命令使用

##### 1. 参数详解

```bash
# 显示用法信息,其实就是ab -help
-h

# 打印版本号并退出
-V

# 即requests,用于指定压力测试总共的执行次数
-n

# 即concurrency,用于指定压力测试的并发数
-c

# 即timelimit,等待响应的最大时间(单位：秒)
-t

# 即windowsize,TCP发送/接收的缓冲大小(单位：字节)
-b

# 即postfile,发送POST请求时需要上传的文件,此外还必须设置-T参数
-p

# 即putfile,发送PUT请求时需要上传的文件,此外还必须设置-T参数
-u

# 即content-type,用于设置Content-Type请求头信息,例如：application/x-www-form-urlencoded,默认值为text/plain
-T

# 即verbosity,指定打印帮助信息的冗余级别
-v

# 以HTML表格形式打印结果
-w

# 使用HEAD请求代替GET请求
-i

# 插入字符串作为table标签的属性
-x

# 插入字符串作为tr标签的属性
-y

# 插入字符串作为td标签的属性
-z

# 添加cookie信息,例如："key=value"(可以重复该参数选项以添加多个)
-C

# 添加任意的请求头HEADER,请求头将会添加在现有的多个请求头之后(可以重复该参数选项以添加多个)
-H

# 添加一个基本的网络认证信息,用户名和密码之间用英文冒号隔开
-A

# 添加一个基本的代理认证信息,用户名和密码之间用英文冒号隔开
-P

# 指定使用的代理服务器和端口号
-X

# 使用HTTP的KeepAlive特性
-k

# 不显示百分比
-d

# 不显示预估和警告信息
-S

# 输出结果信息到gnuplot格式的文件中
-g

# 输出结果信息到CSV格式的文件中
-e

# 指定接收到错误信息时不退出程序
-r
```