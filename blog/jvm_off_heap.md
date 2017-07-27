### JVM堆外内存分析
@Date 2017.07.20

> 事件回顾

在对应用进行压测的时候,观察物理内存占用以及JVM堆中对象情况,发现物理内存占用很大,堆中对象却很少.怀疑是堆外内存占用问题.

> 工具介绍(gperftools)

* 下载工具libunwind-1.0.tar.gz和gperftools-2.5.tar.gz并进行编译安装(具体可以查找教程)
* 在安装之后并配置好gperf路径
* 在JVM启动应用之前,执行以下命令
```
export LD_PRELOAD=/usr/local/lib/libtcmalloc.so
export HEAPPROFILE=/home/admin/logs/pro
```
* 会在HEAPPROFILE指定的目录中生成.heap文件
* 使用以下命令可以查看堆外内存文件的构成(支持导出gif或者pdf)
```
pprof --text /opt/taobao/install/ajdk-8.2.3-b46/bin/java pro_149025.0001.heap
pprof --pdf /opt/taobao/install/ajdk-8.2.3-b46/bin/java pro_149025.0001.heap > xxx.pdf
```
* pprof默认显示方法占用,但是有的时候只显示内催地址,不显示具体方法,不容易排查,则可以用下面的工具dump出来内存地址和方法的一个关系.生成的是16进制的起始地址和16进程的地址长度.可以写个脚本计算之后与pprof结果对比
```
https://github.com/jvm-profiling-tools/perf-map-agent
```