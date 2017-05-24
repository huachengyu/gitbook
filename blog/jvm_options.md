### Java VM 启动参数解析
@Date 2017.05.24

> -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/home/admin/logs/java.hprof
 * 在应用挂掉时输出dump文件
 
> -XX:+TraceClassLoading
 * 打印加载类的详细信息

> -verbose
 * 打印加载类的详细信息
 
> -verbose:gc
 * 打印虚拟机中GC的详细情况
 
> -Xms
 * 初始堆大小

> -Xmx
 * 最大堆大小
 
> -Xmn
 * 新生代堆大小
 
> -XX:SurvivorRatio=8
 * 打印加载类的详细信息
 
> -Xms
 * 初始堆大小

> -Xmx
 * 最大堆大小
 
> -Xmn
 * 年轻代堆大小