### BAT校招之Java面试
@Date 2016.06.26

> 阿里校招

1.java中所有类的父类是什么？他都有什么方法？

2.java中IO包下面的inputstream运用了什么设计模式？请简述你知道的设计模式?

3.ArrayList跟LinkedList的区别详细说出？

4.session和cookie的区别？

5.说一下快速排序的原理？

6.简述AOP，及其作用？

7.struts2的流程？

8.java有些类中为什么需要实现Serializable接口？

9.hashmap，hashtable的区别？以及如何使用，以及他的一些方法？

10.设计题，利用hashmap对苹果的重量进行排序？

11.struts2拦截器相关问题？struts2接受参数的方式？

12.异常的相关问题？error和exception具体？

13.servlet相关知识，doPost，doGet，一些内置对象？

14.set和list的区别，一些个实现类，继承关系等等？

15.数据库事务隔离机制及其特点是什么？

16.JVM内存heap and stack

17.HTTP报文包含内容？

18.谈谈数据结构中的时间复杂度如何计算的，链表和数组区别

19.写出快排，并说出快排的时间复杂度，还有最差情况是什么情况下

20.浮点数在计算机中如何存储，0.1+0.2为什么等于0.30000000000000004

21.new一个对象时构造函数发生了什么，如果主动return一个对象，那返回的是什么

22.在网络传输数据时，经常需要将二进制数据转换为一个可打印字符串，一般用到的可打印字符集合包含64个字符，因此又称为Base64表示法，现有一个char数组长度为12，要将它表示为Base64字符串，请问Base64字符串至少需要几个char；如果char的长度为20，则需要几个char?

23.假设支付宝红包口令支持1到6位的数字组合，即’0′,’1′,’003’和‘999999’都是合法的红包口令，那么总共可以有多少个合法的红包口令？

24.一个具有513个节点的二叉树，有_种可能的层高?

25.给定一个整数sum，从有N个无序元素的数组中寻找元素a，b，c，d，使得a + b + c + d = sum，最快的平均时间复杂度是_?

26.Hashtable,HashMap,ConcurrentHashMap 底层实现原理与线程安全问题（建议熟悉 jdk 源码，才能从容应答）

27.Java 的引用类型有哪几种

28.抽象类和接口的区别

29.设计模式会哪些

30.工厂方法模式的优点（低耦合、高内聚，开放封闭原则）

31.老年代中数组的访问方式

32.GC 算法，永久代对象如何 GC ， GC 有环怎么处理

33.谁会被 GC ，什么时候 GC

34.如果想不被 GC 怎么办

35.如果想在 GC 中生存 1 次怎么办

36.hashCode() 与 equals() 生成算法、方法怎么重写

37.排序算法与时空复杂度

38.查找树与红黑树

39.jvm 如何分配直接内存， new 对象如何不分配在堆而是栈上，常量池解析

40.HashMap 与线程安全问题

41.JVM如何加载一个类的过程，双亲委派模型中有哪些方法？

42.进程间通信有哪几种方式？

43.GC用的引用可达性分析算法中，哪些对象可作为GC Roots对象？

44.JVM如何GC，新生代，老年代，持久代，都存储哪些东西？

45.Linux下如何进行进程调度的？

46.Linux下你常用的命令有哪些？

47.常用的hash算法有哪些？什么是一致性哈希？

48.数据库中的范式有哪些？

49.Java中的NIO，BIO，AIO分别是什么？

50.现在JVM中有一个线程挂起了，如何用工具查出原因？

51.线程同步与阻塞的关系？同步一定阻塞吗？阻塞一定同步吗？

52.同步和异步有什么区别？

53.如何创建单例模式？说了双重检查，他说不是线程安全的。如何高效的创建一个线程安全的单例？

54.concurrent包下面，都用过什么？

55.了解哪些设计模式？说说都用过哪些设计模式

56.匿名内部类是什么？如何访问在其外面定义的变量？

57.get的字节限制是协议本身限制的吗?

58.hashtable是怎么实现线程安全的?hashtable原理?

59.数据库事务隔离机制及特点?数据库引擎?

60.dns是基于tcp还是udp的

61.连接池使用使用什么数据结构实现?

62.https怎么做到安全的

63.B+树和二叉树查找时间复杂度

64.什么时候会发生jvm堆（持久区）内存溢出?内存溢出了怎么办?

---

> 腾讯校招

1.如何优化数组这个数据结构？

2.Mongo的特点，和mysql的区别

3.contains和compareDocumentPosition区别和使用

4.链表判断环路和查找连接点

5.两个栈实现队列，如何实现多线程并发

6.两个串任意合并是否可以成为第三个串

7.linux内核态和用户态，为什么要这么分？

8.tcp和udp的区别，tcp是怎么做错误处理的？

9.读文件时系统和硬盘会做哪些工作

10.select、poll和epoll

11.SERVLET API中forward()与redirect()的区别？

---

> 百度校招

1.hashmap和hashtable区别

2.对线程安全的理解

3.讲讲web三大架构

4.为什么要用struts做mvc

5.什么技术是关于解耦的

6.AOP是怎么实现的

7.java的代理是怎么实现的

8.http和https的区别

9.get提交和post提交的区别

10.怎么解决中文乱码问题

11.设计模式

12.你对MVC的理解

13.XML和JSON的区别？json和xml哪个流量比较大？

14.抽象类和接口的区别

15.如何快速查出你当前所在地最近的一百家餐馆（不能用遍历）

16.代码实现深度优先和广度优先

17.计算机网络分层，每层所用协议，协议所占端口

18.海量数据查出每天访问百度网站最多的前100个人的IP地址

19.乐观锁悲观锁

20.数据库索引,数据库范式

21.osi七层模型以及tcp/ip四层模型

22.内存溢出和内存泄漏