### JVM问题常用排查命令
@Date 2017.05.23

> JVM调优问题
* CPU使用率与Load值偏大 : Thread count以及GC count
* 关键接口响应时间很慢 : GC time以及GC log中的STW的时间
* 发生Full GC或者Old CMS GC非常频繁 : 内存泄露

> jps

```
# -m：输出main method的参数
# -l：输出完全的包名，应用主类名，jar的完全路径名 
# -v：输出jvm参数 
# -V：输出通过flag文件传递到JVM中的参数(.hotspotrc文件或-XX:Flags=所指定的文件
jps -mlvV
```

> jstack

```
# 
# 线程的堆栈跟踪，可以得知哪些线程被阻塞或正等待，以便于查找如线程死锁的原因
jstack pid
```

```
# -m：打印java和native frames
jstack -m pid
```

> jinfo

```
# 查看应用启动启动参数
jinfo -flag pid
```

> jmap

```
# 查看堆的情况
jmap -heap pid
```

```
# dump内存二进制信息(dump时系统会被短暂暂停FULL GC)
# live子选项:只输出活的对象
jmap -dump:live,format=b,file=/home/admin/heap.bin pid
jmap -dump:format=b,file=/home/admin/heap.bin pid
```

```
# 堆占用情况
jmap -histo pid | head -10
```

> jstat

```
# 查看GC情况
# S0 -Heap上的Survivor space 0区已使用空间的百分比 
# S1 -Heap上的Survivor space 1区已使用空间的百分比 
# E -Heap上Eden space区已使用空间的百分比 
# O -Heap上的Old space区已使用空间的百分比 
# P -Perm space区已使用空间的百分比 
# YGC -从应用程序启动到采样时发生Yang GC 的次数 
# YGCT -从应用程序启动到采样时Yang GC所用的时间【单位秒】 
# FGC -从应用程序启动到采样时Full GC的次数 
# FGCT -从应用程序启动到采样时Full GC所用的时间 
# GCT -从应用程序启动到采样时用于垃圾回收的总时间【单位秒】
jstat -gcutil pid 1000
```
