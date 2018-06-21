### 影响线程创建的因素
@Date 2018.06.21

> JVM : 内存不够会影响Thread的Create, 特别是C Heap. 以下JVM参数主要影响的是剩余内存的大小
* Xmx
  * 堆大小
* Xss
  * 线程堆栈大小
  * 占用越小,创建的线程数量越多
* MaxPermSize
  * 持久代
  * 存放Class和Meta信息
  * 不会被垃圾回收
  * 默认物理内存的1/64
* MaxDirectMemorySize
  * 堆外内存上限
* ReservedCodeCacheSize
  * 代码缓存区
  * JIT编译的代码

> Kernel
* max_user_processes
  * 用户进程数量限制
  * ulimit -u最大值
* max_map_count
  * 涉及系统内存分配操作, 若超过sysctl_max_map_count则创建线程失败
  * /proc/sys/vm/max_map_count, 默认65530
* max_threads
  * /proc/sys/kernel/threads-max
  * 受到物理内存限制,在系统做fork时, 会初始化此值
  * max_threads = mempages / (8 * THREAD_SIZE / PAGE_SIZE);
* pid_max
  * 分配的PID数量限制
  * /proc/sys/kernel/pid_max