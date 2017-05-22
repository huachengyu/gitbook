### 记一次Java进程突然消失问题
@Date 2017.05.22

> 现象: 线上同一个应用部署了多台服务器,有的机器运行过程中突然告警,发现服务进程消失.

* 看程序本身的日志,没有异常输出
* 查询磁盘空间是否不足,没有此问题
* 增加如下启动参数,查看GC日志,发现程序无GC出现

```
   -Xloggc:/home/admin/logs/gc.log 
   -XX:+PrintGCDetails 
   -XX:+PrintGCDateStamps 
   -XX:+HeapDumpOnOutOfMemoryError 
   -XX:HeapDumpPath=/home/admin/logs/java.hprof 
```

* 在上述重启服务时,发现一个现象,程序重启没有多久就消失掉. 此时使用[dmesg](http://baike.baidu.com/link?url=u18Smrel0tAb8KiRlAeiQgWfzu8pcxKMpckoXr7odQs18eaHvYRKhoSb3J_amMHBSN5HbAWy0uP0RYwAH1gFX_)
查看Linux系统日志, dmesg只能查看到最近的系统缓存日志,所以在现场能还原时查看最合适.

#### 重点:
 * 会发现dmesg的日志中出现oom kill的字样,由此可以判断应该是机器内存被占满,系统自动选择一个占用内存最大的进行kill掉
 * 复现确认
    1. 在启动应用之后,使用free命令查看机器内存占用情况, 发现total和used相差无几
    2. 查看JVM启动参数以及机器内存配置,发现-Xmx配置和机器内存相等,由此可以断定是应用启动堆内存沾满机器内存,造成系统KILL
   
> 解决: 修改JVM启动参数,最好改成系统内存大小的1/2或者2/3左右即可

> 注意: 引起系统OOM-KILL的其他情况还有比如使用VIM编辑大文件, 如果文件过大也会造成撑满机器内存
 
 