### Java VM 启动参数详解
@Date 2017.05.24

---
#### 打印输出相关参数

> -verbose
 * 打印加载类的详细信息
 
> -verbose:gc
 * 打印虚拟机中GC的详细情况:显示最忙和最空闲收集行为发生的时间、收集前后的内存大小、收集需要的时间等
 
> -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/admin/logs/java.hprof
 * 虚拟机在出现内存溢出异常时Dump 出当前的内存堆转储快照

> -XX:+TraceClassLoading
 * 打印加载类的详细信息
 
> -XX:+PrintGCDetails 
* 详细了解GC中的变化
 
> -XX:+PrintGCDateStamps
* 了解垃圾收集发生的时间,自JVM启动以后以秒计量

> -Xloggc:/home/admin/logs/gc.log 
* GC日志文件的路径 
 
--- 
#### 涉及堆相关的参数

> -server
* server模式下,新生代选择的是并行GC,旧生代选择的是并行GC
* client模式下,新生代选择的是串行GC,旧生代选择的是串行GC

> -Xms2g
 * 堆的初始值2g

> -Xmx2g
 * 堆的最大值2g
 
> -XX:MinHeapFreeRation=
 * 空余堆内存小于MinHeapFreeRation时,JVM会增大Heap到-Xmx指定的大小
 
> -XX:MaxHeapFreeRation=
 * 空余堆内存大于MaxHeapFreeRation时,JVM会减小heap的大小到-Xms指定的大小

> PS:避免在运行时频繁调整Heap的大小,通常-Xms与-Xmx的值设成一样

> -Xmn1g
 * 新生代堆大小1G
 
> -XX:SurvivorRatio=8
* 默认32:1:1
* 新生代的Eden区:From区:To区的比例为8:1:1
 
> -XX:PermSize= (JDK7)
* 永久代大小
  
> -XX:MaxPermSize= (JDK7)
* 永久代MAX大小
 
> -XX:MetaspaceSize= (JDK8)
* 代替PermSize,元空间大小 
 
> -XX:MaxMetaspaceSize= (JDK8)
* 代替MaxPermSize,元空间最大值
 
---
#### 垃圾清理类型

> -XX:+UseSerialGC
* Serial GC
* 使用简单的标记、清除、压缩方法对年轻代和年老代进行垃圾回收,即Minor GC和Major GC
* 适用于CPU配置较低,内存占用较少的单独Client应用模式

> -XX:+UseParallelGC
* Parallel GC
* 收集方式同Serial GC一样
* 产生N个线程来进行年轻代的垃圾收集
* N默认=系统CPU核数

> -XX:ParallelGCThreads=
* 设置Parallel GC线程数量

> -XX:+UseParallelOldGC
* Parallel Old GC
* 方式同Parallel GC
* 主要是年轻代和年老代垃圾回收时都使用多线程收集

> -XX:+UseConcMarkSweepGC
* 并发标记清除（CMS）收集器
* 短暂停顿并发收集器
    1. CMS-initial-mark：初始标记阶段(stop the world)
    2. CMS-concurrent-mark : 和应用线程并发执行,标记可达的对象
    3. CMS-concurrent-preclean(CMS-concurrent-preclean-start,CMS-concurrent-preclean) : 预清理(标记和应用线程是并发执行的,有些对象的状态在标记后会改变)
    4. CMS-concurrent-abortable-preclean : 进一步的预清理,减少Rescan阶段时间.使用到参数(-XX:CMSMaxAbortablePrecleanTime)
    5. Rescan阶段(stop the world) : 暂停应用线程,对对象进行重新扫描并标记
    6. CMS-concurrent-sweep : 并发的垃圾清理
    7. CMS-concurrent-reset : 下一次CMS GC重置相关数据结构
* 对年老代进行垃圾收集
* 年轻代使用的算法和Parallel一样
* 适用于不能忍受长时间停顿要求快速响应的应用
* !!!FULL GC
    1. concurrent-mode-failure : CMS GC时,有新的对象要进入年老代,但是年老代空间不足
    2. promotion-failed : Young GC时,存活对象从Eden区到Survivor区,但是Survivor区空间不足,需要到年老代,年老代空间也不足

> -XX:CMSMaxAbortablePrecleanTime=
* CMS GC在concurrent-abortable-preclean阶段使用的参数
* 当abortable-preclean阶段执行达到这个时间时才会结束

> -XX:+CMSClassUnloadingEnabled
* 避免Perm区满引起的Full GC,开启CMS回收Perm区 

> -XX:CMSInitiatingOccupancyFraction=
* 此参数值是一个比例(参考下一条)

> -XX:+UseCMSInitiatingOccupancyOnly
* 如果设置了此参数
* 只有当年老代占用达到了-XX:CMSInitiatingOccupancyFraction参数所设定的比例时才会触发CMS

> -XX:ParallelCMSThreads=
* 设置CMS收集器的线程数量

> -XX:+UseG1GC
* G1垃圾收集器
* JDK7+,长远目标时代替CMS收集器
* 并行的、并发的和增量式压缩短暂停顿的垃圾收集器
* G1收集器和其他的收集器运行方式不一样,不区分年轻代和年老代.它把堆空间划分为多个大小相等的区域,当进行垃圾收集时,它会优先收集存活对象较少的区域

> -Dsun.rmi.dgc.server.gcInterval=2592000000
> -Dsun.rmi.dgc.client.gcInterval=2592000000
* 存在RMI调用时,默认会每分钟执行一次System.gc,可以通过此参数来设置大点的间隔

---
#### 堆外内存相关参数

> -XX:MaxDirectMemorySize=
* 指定了DirectByteBuffer分配空间限额
* DirectByteBuffer通过内存映射,使Java应用进程直接访问与文件相关联的虚拟地址空间,减少了文件拷贝带来的开销,提高了文件读取效率
* 默认会调用Full GC来回收

> -XX:+DisableExplicitGC
* 禁用Full GC显示调用
* 会出现OOM：系统各方面性能良好,无Full GC且DirectByteBuffer所占用的空间大于-Xmx分配的空间.
因为DirectByteBuffer会不断地在Native堆分配空间,它的引用进入了Old区,Old区保存大量的引用,而不能被回收,最终会导致Native堆空间不足
* 可以通过设置-XX:MaxDirectMemorySize=几十M,使OOM来得更早一些

> -XX:+ExplicitGCInvokesConcurrent
* 使用此参数,在进行堆外内存Full GC时,使用CMS并发GC,减少单纯的Full GC的卡顿时间,提高GC效率

---

#### other options

> -Dsun.net.client.defaultConnectTimeout=10000 
> -Dsun.net.client.defaultReadTimeout=30000
* 设置默认的Http请求的连接超时时间和读取超时时间

> -Dfile.encoding=UTF-8
* 设置文件流编码

> -agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n
* 开放8000端口,支持远程DEBUG

---

#### SpringBoot启动参数样例
> 机器配置4核8G

```
java -server 
-Xms2g 
-Xmx2g 
-Xmn1g 
-XX:MetaspaceSize=256m 
-XX:MaxMetaspaceSize=256m 
-XX:MaxDirectMemorySize=1g 
-XX:SurvivorRatio=10 
-XX:+UseConcMarkSweepGC 
-XX:CMSMaxAbortablePrecleanTime=5000 
-XX:+CMSClassUnloadingEnabled 
-XX:CMSInitiatingOccupancyFraction=80 
-XX:+UseCMSInitiatingOccupancyOnly 
-XX:+ExplicitGCInvokesConcurrent 
-Dsun.rmi.dgc.server.gcInterval=2592000000 
-Dsun.rmi.dgc.client.gcInterval=2592000000 
-XX:ParallelGCThreads=4 
-Xloggc:/home/admin/logs/gc.log 
-XX:+PrintGCDetails 
-XX:+PrintGCDateStamps 
-XX:+HeapDumpOnOutOfMemoryError 
-XX:HeapDumpPath=/home/admin/logs/java.hprof 
-Djava.awt.headless=true 
-Dsun.net.client.defaultConnectTimeout=10000 
-Dsun.net.client.defaultReadTimeout=30000 
-Dfile.encoding=UTF-8
```