### Java VM 启动参数解析
@Date 2017.05.24

---
#### 打印输出相关参数

> -verbose
 * 打印加载类的详细信息
 
> -verbose:gc
 * 打印虚拟机中GC的详细情况
 
> -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/admin/logs/java.hprof
 * 虚拟机在出现内存溢出异常时Dump 出当前的内存堆转储快照

> -XX:+TraceClassLoading
 * 打印加载类的详细信息
 
--- 
#### 涉及堆相关的参数

> -Xms2g
 * 堆的初始值2g

> -Xmx2g
 * 堆的最大值2g
 
> -XX:MinHeapFreeRation=
 * 空余堆内存小于MinHeapFreeRation时，JVM会增大Heap到-Xmx指定的大小
 
> -XX:MaxHeapFreeRation=
 * 空余堆内存大于MaxHeapFreeRation时，JVM会减小heap的大小到-Xms指定的大小

> PS:避免在运行时频繁调整Heap的大小,通常-Xms与-Xmx的值设成一样

> -Xmn1g
 * 新生代堆大小1G
 
> -XX:SurvivorRatio=8
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
    5. Rescan阶段(stop the world) : 暂停应用线程，对对象进行重新扫描并标记
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

---
#### 堆外内存相关参数

> -XX:MaxDirectMemorySize=
* 指定了DirectByteBuffer分配空间限额
* DirectByteBuffer通过内存映射,使Java应用进程直接访问与文件相关联的虚拟地址空间,减少了文件拷贝带来的开销,提高了文件读取效率

> 