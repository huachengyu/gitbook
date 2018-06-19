> 读[Java并发专题](https://www.jianshu.com/p/27860941a77b)总结

### 一. 基础知识
* 新建线程
  *  继承Thread类，重写run方法
  *  实现Runable接口
  *  实现Callable接口
* 线程状态
  * NEW/Runable/BLOCKED/TIMED _WAITING/WAITING/TERMINATED
  * 调用wait()、join()、LockSupport.lock()方法, 线程会进入到WAITING状态
  * 调用带超时时间的wait(long timeout)、sleep(long)、join(long)、LockSupport.parkNanos()、LockSupport.parkUtil()方法, 线程会进入到TIMED_WAITING状态
  * 调用Object.notify()、Object.notifyAll()方法使线程转换到Runable状态
  * 进入synchronized方法或者synchronized代码块时,线程切换BLOCKED状态
  * 与WAITING状态相关联的是等待队列，与BLOCKED状态相关的是同步队列
* 线程协作
  * join()、join(long millis) : 一个线程等待另一个线程结束
* sleep()与wait()
  * sleep()方法是Thread的静态方法，而wait是Object实例方法
  * sleep()方法并不会失去锁, 不会释放资源,只会释放CPU. wait()方法会释放占有的对象锁,使得该线程进入等待池中,等待下一次获取资源.
  * wait()方法必须等待notify()/notifyAll()通知后, 才会离开等待池
* yield()
  * 让出CPU, 让出的时间片只会分配给当前线程相同优先级的线程, 本线程也会继续参数CPU竞争

### 二. 两大核心&三个性质
* 并发理论JMM
  * 内存一致性
    * 共享内存、CPU缓存
  * 重排序
    * 编译器优化重排序
    * 指令重排序
    * 内存系统重排序
* happens-before原则
* 原子性
  * 一个操作是不可中断的,要么全部执行成功要么全部执行失败
    * lock(锁定)
    * unlock(解锁)
    * read(读取)
    * load(载入)
    * use(使用)
    * assign(赋值)
    * store(存储)
    * write(操作)
  * i++不是原子操作 : 1.读取变量 2.值加一 3.赋值回变量
* 有序性
  * synchronized具有有序性
  * volatile包含禁止指令重排序的语义,其具有有序性
* 可见性
  * synchronized和volatile都具有可见性


### 三. 并发关键字
* synchronized关键字
  * 类的实例对象锁
  * 类本身对象锁
* 对象锁(monitor)机制
  * 每个对象都有一个监视器(monitor)&计数器
  * 锁具有重入性
* CAS锁优化
  * ABA问题 : 添加版本号解决比较值相等问题
  * 自旋时间过长问题(线程不挂起,死循环) : 非阻塞同步, 线程不会挂起, 死循环
* Java对象头
  * 对象是否有锁, 有(轻量级、重量级、偏向锁)标志位
* volatile关键字
  * 修饰的共享变量 : 会有Lock前缀的指令
  * 缓存一致性
    * Lock前缀的指令会引起处理器缓存写回内存
    * 一个处理器的缓存回写到内存会导致其他处理器的缓存失效
    * 当处理器发现本地缓存失效后，就会从内存中重读该变量数据，即可以获取当前最新值
  * 内存屏障
    * 为了实现volatile内存语义时,编译器在生成字节码时,会在指令序列中插入内存屏障来禁止特定类型的处理器重排序
  * 保证可见性,不保证原子性
* final关键字
  * 可以修饰变量、方法、类
  * 修饰基本数据类型变量时,不能对基本数据类型变量重新赋值,因此基本数据类型变量不能被改变
  * 修饰引用类型变量时, 它仅仅保存的是一个引用, final只保证这个引用类型变量所引用的地址不会发生改变,即一直引用这个对象,但这个对象属性是可以改变的
  * 父类的方法被final修饰的时候,子类不能重写父类的该方法
  * final方法是可以被重载的
  * 当一个类被final修饰时, 说明该类是不能被子类继承的
  * JDK中提供的八个包装类和String类都是不可变类
* 写final域重排序规则
  * 禁止对final域的写重排序到构造函数之外
  * 编译器会在final域写之后, 函数return之前,插入一个storestore屏障,这个屏障可以禁止处理器把final域的写重排序到构造函数之外
  * 在对象引用为任意线程可见之前,对象的final域已经被正确初始化过了,而普通域就不具有这个保障
* 读final域重排序规则
  * 在一个线程中, 初次读对象引用和初次读该对象包含的final域, JMM会禁止这两个操作的重排序(仅对处理器生效)
  * 在读一个对象的final域之前,一定会先读这个包含这个final域的对象的引用
  

### 四. Lock体系
* AbstractQueuedSynchronizer(AQS)
  * 独占锁
  * 共享锁
* ReentrantLock重入锁
  * 公平锁
    * 满足FIFO,锁的获取顺序符合请求上的绝对时间顺序
    * 频繁切换上下文, 性能低
  * 非公平锁
    * 会造成永远抢不到锁的情况,饥饿
    * ReentrantLock默认是非公平锁,保证吞吐量
  * ReentrantReadWriteLock读写锁
    * 允许同一时刻被多个读线程访问,但是在写线程访问时,所有的读线程和其他的写线程都会被阻塞
    * 锁降级:遵循获取写锁,获取读锁再释放写锁的次序, 写锁能够降级成读锁
* Condition
  * 等待/通知机制
* 线程中断
  * InterruptedException : 当调用Thread.interrupt()中断一个线程时. 若线程处于阻塞中(Thread.sleep()、Thread.join()、Object.wait()), 线程会取消阻塞抛出异常
* LockSupport
  * 线程的阻塞原语,用来阻塞线程和唤醒线程
  * 每个使用LockSupport的线程都会与一个许可关联,如果该许可可用,并且可在线程中使用,则调用park()将会立即返回,否则可能阻塞'
  * 如果许可尚不可用,则可以调用unpark()使其可用
  * 许可不可以重入
  * void park():阻塞当前线程
  * void unpark(Thread thread):唤醒处于阻塞状态的指定线程
  * synchronzed致使线程阻塞,线程会进入到BLOCKED状态
  * LockSupprt方法阻塞线程会致使线程进入到WAITING状态

### 五. 并发容器
* ConcurrentHashMap
  * 利用了锁分段的思想提高了并发度
  * JDK1.8之前
    * segment继承了ReentrantLock充当锁的角色,为每一个segment提供了线程安全的保障
    * segment维护了哈希散列表的若干个桶,每个桶由HashEntry构成的链表
  * JDK1.8之后
    * 舍弃segment,大量使用synchronized以及CAS
    * synchronized优化: 偏向锁、轻量级锁、重量级锁、锁状态升级
    * 底层数据结构 : 数组 + 链表 + 红黑树
  * 构造方法支持传入(Map的大小、加载因子、并发度)
  * spread()重哈希 : hash值分散的不够均匀的话会大大增加哈希冲突的概率,从而影响到hash表的性能.通过spread方法进行了一次重hash从而大大减小哈希冲突的可能性.
    (h ^ (h >>> 16)) & HASH_BITS : 将key的hashCode的低16位于高16位进行异或运算
* CopyOnWriteArrayList
  * 线程安全,读读之间不会被阻塞
  * 牺牲数据实时性满足数据的最终一致性即可
  * COW : 写时复制的思想来通过延时更新的策略来实现数据的最终一致性,并且能够保证读线程间不阻塞
    * ReentrantLock : 同一时刻只有一个写线程正在进行数组的复制
    * 数组引用是volatile修饰的,因此将旧的数组引用指向新的数组,根据volatile的happens-before规则,写线程对数组引用的修改对读线程是可见的
    * volatile的修饰的仅仅只是数组引用，数组中的元素的修改是不能保证可见性的
    * 适合写少读多的情况,每次写的时候内存占用双份. 只保证最终一致性, 适用于不实时的配置读取等
* ConcurrentLinkedQueue
  * offer
    * 若tail指向的节点的下一个节点（next域）为null的话,说明tail指向的节点即为队列真正的队尾节点,因此可以通过casNext插入当前待插入的节点,但此时tail并未变化
    * 若tail指向的节点的下一个节点（next域）不为null的话,说明tail指向的节点不是队列的真正队尾节点.通过q（Node<E> q = p.next）指针往前递进去找到队尾节点,然后通过casNext插入当前待插入的节点,并通过casTail方式更改tail
  * poll
    * 如果当前head,h和p指向的节点的Item不为null的话,说明该节点即为真正的队头节点(待删除节点),只需要通过casItem方法将item域设置为null,然后将原来的item直接返回即可
    * 如果当前head,h和p指向的节点的item为null的话,则说明该节点不是真正的待删除节点,那么应该做的就是寻找item不为null的节点.通过让q指向p的下一个节点(q = p.next)进行试探,若找到则通过updateHead方法更新head指向的节点以及构造哨兵节点(通过updateHead方法的h.lazySetNext(h))
  * 延迟更新
    * tail : 当tail指向的节点的下一个节点不为null的时候,会执行定位队列真正的队尾节点的操作,找到队尾节点后完成插入之后才会通过casTail进行tail更新.
    当tail指向的节点的下一个节点为null的时候,只插入节点不更新tail
    * head : 当head指向的节点的item域为null的时候,会执行定位队列真正的队头节点的操作,找到队头节点后完成删除之后才会通过updateHead进行head更新.
    当head指向的节点的item域不为null的时候，只删除节点不更新head
* ThreadLocal
  * 存放在ThreadLocalMap, key = Thread.currentThread()
  * Entry是一个以ThreadLocal为key,Object为value的键值对,另外需要注意的是这里的threadLocal是弱引用
  * 当threadLocal外部强引用被置为null(threadLocalInstance=null),那么系统GC的时候,根据可达性分析,这个threadLocal实例就没有任何一条链路能够引用到它.
  这个ThreadLocal势必会被回收,ThreadLocalMap中就会出现key为null的Entry,就没有办法访问这些key为null的Entry的value.
  如果当前线程再迟迟不结束的话,这些key为null的Entry的value就会一直存在一条强引用链：Thread Ref -> Thread -> ThreaLocalMap -> Entry -> value永远无法回收,造成内存泄漏
  * set方法中会使用开放地址法向散列表存储,并且会替换key=null的脏entry,以及插入后清除key=null的脏entry.并且大于阈值就扩容
  * 使用场景
    * 每个不同的线程都拥有专属于自己的数据容器(threadLocalMap),彼此不影响
    * 适用于共享对象会造成线程安全的业务场景
* BlockingQueue
  * ArrayBlockingQueue
    * 由数组实现的有界阻塞队列,FIFO(先进先出)
    * 容量不可改变
    * 默认不保证线程的公平性
    * 保证线程安全:ReentrantLock lock
  * LinkedBlockingQueue
    * 由链表实现的有界阻塞队列,FIFO(先进先出)
    * 吞吐量比较大
    * 插入数据和删除数据时分别是由两个不同的lock(takeLock和putLock)来控制线程安全的
  * PriorityBlockingQueue
    * 支持优先级的无界阻塞队列
  * SynchronousQueue
    * 每个插入操作必须等待另一个线程进行相应的删除操作
    * 当前有线程在插入数据时,线程才能删除数据
  * LinkedTransferQueue
    * 由链表数据结构构成的无界阻塞队列
    * 实现了TransferQueue接口
      * transfer(E e)
        如果当前有线程（消费者）正在调用take()方法或者可延时的poll()方法进行消费数据时,生产者线程可以调用transfer方法将数据传递给消费者线程.
        如果当前没有消费者线程的话,生产者线程就会将数据插入到队尾,直到有消费者能够进行消费才能退出
  * LinkedBlockingDeque
    * 基于链表数据结构的有界阻塞双端队列
  * DelayQueue
    * 存放实现Delayed接口的数据的无界阻塞队列
    * 只有当数据对象的延时时间达到时才能插入到队列进行存储
    
### 六. 线程池
* ThreadPoolExecutor(int corePoolSize,
                    int maximumPoolSize,
                    long keepAliveTime,
                    TimeUnit unit,
                    BlockingQueue<Runnable> workQueue,
                    ThreadFactory threadFactory,
                    RejectedExecutionHandler handler)
  * corePoolSize : 核心线程池
  * maximumPoolSize : 线程池能创建线程的最大个数.当阻塞队列已满时,并且当前线程池线程个数没有超过maximumPoolSize的话,就会创建新的线程来执行任务
  * keepAliveTime : 空闲线程存活时间.当前线程池的线程个数已经超过了corePoolSize,并且线程空闲时间超过了keepAliveTime的话,就会将这些空闲线程销毁
  * unit : 时间单位
  * workQueue : 阻塞队列.可以使用ArrayBlockingQueue、LinkedBlockingQueue、SynchronousQueue、PriorityBlockingQueue
  * threadFactory : 创建线程的工厂类.设置线程池名称
  * handler : 饱和策略
    * AbortPolicy : 直接拒绝提交的任务,抛出RejectedExecutionException异常
    * CallerRunsPolicy : 使用调用者所在的线程来执行任务
    * DiscardPolicy : 直接丢弃任务
    * DiscardOldestPolicy : 丢弃阻塞队列中存放时间最久的任务,执行当前任务
* 线程池合理配置
  * 任务的性质:
    * CPU密集型任务 : 尽量少的数量,CPU(Runtime.getRuntime().availableProcessors()) + 1
    * IO密集型任务 : 需要等待IO操作,数量尽可能多
  * 任务的优先级:高、中、低
  * 任务的执行时间
  * 任务的依赖性:是否依赖其他系统资源,如数据库连接
  * 最好采用有界队列,防止内存资源过多
* ScheduledThreadPoolExecutor
  * 在给定延时后执行异步任务或者周期性执行任务
  * DelayedWorkQueue
    * 采用数组构成,基于堆的数据结构,插入和删除最坏时间复杂度O(logN)
    * 按照执行时间的升序排序, 是一个优先级队列

### 七. 原子操作类
* 悲观锁
  * synchronized
* 乐观锁
  * CAS(compare and swap)
    * AtomicBoolean：以原子更新的方式更新boolean
    * AtomicInteger：以原子更新的方式更新Integer
    * AtomicLong：以原子更新的方式更新Long
    * AtomicIntegerArray：原子更新整型数组中的元素
    * AtomicLongArray：原子更新长整型数组中的元素
    * AtomicReferenceArray：原子更新引用类型数组中的元素
    * AtomicReference：原子更新引用类型
    * AtomicReferenceFieldUpdater：原子更新引用类型里的字段
    * AtomicMarkableReference：原子更新带有标记位的引用类型
    * AtomicIntegeFieldUpdater：原子更新整型字段类
    * AtomicLongFieldUpdater：原子更新长整型字段类
    * AtomicStampedReference：原子更新引用类型,带版本号

### 八. 并发工具
* 倒计时器CountDownLatch
  * CountDownLatch.countDown方法时就会对计数器的值减一, 线程不会阻塞
  * await() : 调用该方法的线程等到构造方法传入的N减到0的时候,才能继续往下执行
  * 一般用于某个线程A等待若干个其他线程执行完任务之后,它才执行
  * 不能复用
* 循环栅栏CyclicBarrier
  * CyclicBarrier在使用一次后,下面依然有效,可以继续当做计数器使用,这是与CountDownLatch的区别之一(可以复用)
  * 调用await(),阻塞等待barrier等于指定数值
  * 一般用于一组线程互相等待至某个状态,然后这一组线程再同时执行
* 控制资源并发访问Semaphore(信号量)
  * acquire() : 获取许可,如果无法获取到,则阻塞等待直至能够获取为止
  * tryAcquire() : 尝试获取许可,如果能够获取成功则立即返回true.否则,则返回false
  * 默认非公平锁
  * 用来做特殊资源的并发访问控制是相当合适的
* 线程间交换数据的工具Exchanger
  * exchange(V x) : 当一个线程执行该方法的时,会等待另一个线程也执行该方法,因此两个线程就都达到了同步点.
  将数据交换给另一个线程,同时返回获取的数据

  
  
  

