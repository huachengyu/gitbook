### Java常见面试题总结
@Date 2016.06.22

> Java基础知识

1.Java 中应该使用什么数据类型来代表价格?

如果不是特别关心内存和性能的话，使用BigDecimal，否则使用预定义精度的 double 类型。

2.怎么将 byte 转换为String，以及注意点？

可以使用 String 接收 byte[] 参数的构造器来进行转换，需要注意的点是要使用的正确的编码，否则会使用平台默认编码，这个编码可能跟原来的编码相同，也可能不同。

3.我们能将 int 强制转换为 byte 类型的变量吗？如果该值大于 byte 类型的范围，将会出现什么现象？

可以做强制转换，但是 Java 中 int 是 32 位的，而 byte 是 8 位的，所以，如果强制转化是，int 类型的高 24 位将会被丢弃，byte 类型的范围是从 -128 到 128。

4.clone方法在哪个类中？Cloneable 还是 Object？

java.lang.Cloneable 是一个标示性接口，不包含任何方法，clone 方法在 object 类中定义。并且需要知道 clone() 方法是一个本地方法，这意味着它是由 c 或 c++ 或 其他本地语言实现的。

5.Java 中 ++ 操作符是线程安全的吗？

不是线程安全的操作。它涉及到多个指令，如读取变量值，增加，然后存储回内存，这个过程可能会出现多个线程交差。

6.a = a + b 与 a += b 的区别？

注意类型强势转换的问题

7.为什么 Java 中的 String 是不可变的？

Java 中的 String 不可变是因为 Java 的设计者认为字符串使用非常频繁，将字符串设置为不可变可以允许多个客户端之间共享相同的字符串。

8.final都能修饰什么，起到什么作用？

final 是一个修饰符，可以修饰变量、方法和类。如果 final 修饰变量，意味着该变量的值在初始化后不能被改变。

9.什么是强类型程序设计语言？

在强类型语言中，编译器确保类型的正确性，例如你无法在String类型中存放数字，反之亦然。Java是强类型语言，因此存在各种数据类型（如int、float、String、char、boolean等）。你只能将兼容的值存入相应的类型中。与此相反，弱类型语言不要求在编译时进行类型检查，它们根据上下文处理值。Python和Perl是两个常见的弱类型程序设计语言的例子，你可以将数字组成的字符串保存在数字类型中。

10.不可变(immutable)类是什么意思？

一个类，如果在创建之后它的状态就不能被改变，那么他就是不可变的。例如Java中的String。一旦你创建了一个String，例如“Java”，你就不能再改变它的内容。任何对这个字符串的改变（例如转换到大写、与另一个String连接）将创建一个新的对象。不可变的对象在并行程序设计中很有用，因为它们可以在进程间被共享，不需要担心同步。事实上，整个函数是程序设计的模型都是在不可变对象上构建的。

11.接口和抽象类有什么区别？

这是所有程序员面试最经典的问题。接口是最纯粹的抽象形式，没有任何具体的东西。抽象类是一些抽象和具体事物的组合体。这个区别在不同语言中可能会不同，例如在Java中你可以扩展(extend)多个接口，但只能继承一个抽象类。

12.迭代和递归有什么区别？

迭代通过循环来重复执行同一步骤，递归通过调用函数自身来做重复性工作。递归经常是复杂问题（例如汉诺塔、反转链表或反转字符串）的清晰简洁的解决方案。递归的一个缺陷是深度，由于递归在栈中存储中间结果，你只能进行一定深度的递归，在那之后你的程序会因为StackOverFlowError而崩溃。这就是在产品代码中优先使用迭代而不是递归的原因。

13.&和&&运算符的区别是什么？

&是位运算符，&&是逻辑运算符。&和&&的一个区别是位运算符（&）可以被用于整型和布尔类型，逻辑运算符（&&）只能被用于布尔类型变量。当你写a & b时，两个整型数的每一位都会进行与运算。当你写a && b时，第二个参数可能会也可能不会被执行，这也是它被称为短路运算符的原因，至少在Java中是这样的。我很喜欢这个问题，经常对初级开发者和毕业生问这个问题。

14.如何得到一个整型数的最后一位？

用取模运算符，数字 % 10返回数字的最后一位。例如2345 % 10会返回5，567 % 10会返回7。类似的，除运算符可以用来去掉数字的最后一位，例如2345 / 10的结果是234，567 / 10的结果是56。这是值得了解的一个重要技巧，可以用来解决类似回文数、反转数的问题。

15.链表和数组有哪些重要区别？

链表和数组都是程序设计世界中重要的数据结构。它们间最明显的区别是，数组将元素存放在连续的地址中，链表将数据存放在内存中任意的位置。这使得链表有巨大的扩展自己的灵活性，因为内存总是分散的。这种情况总是可能的：你无法创建一个数组来存放一百万个整数，但可以用链表来存放，因为空间是存在的，只是不连续。其他的不同都是源于这项事实。例如，在数组中，如果你知道下标，可以用O(1)的时间得到一个元素，但在链表中要花O(n)的时间。

16.有没有可能两个不相等的对象有有相同的 hashcode？

有可能，两个不相等的对象可能会有相同的 hashcode 值，这就是为什么在 hashmap 中会有冲突。相等 hashcode 值的规定只是说如果两个对象相等，必须有相同的hashcode 值，但是没有关于不相等对象的任何规定。

17.两个相同的对象会有不同的的 hash code 吗？

不能，根据 hash code 的规定，这是不可能的。

18.我们可以在 hashcode() 中使用随机数字吗？

不行，因为对象的 hashcode 值必须是相同的。参见答案获取更多关于 Java 中重写 hashCode() 方法的知识。

19.为什么在重写 equals 方法的时候需要重写 hashCode 方法？

因为有强制的规范指定需要同时重写 hashcode 与 equal 是方法，许多容器类，如 HashMap、HashSet 都依赖于 hashcode 与 equals 的规定。

---

> Java集合框架
  
1.List、Set之间的区别

List 是一个有序集合，允许元素重复。它的某些实现可以提供基于下标值的常量访问时间，但是这不是 List 接口保证的。Set 是一个无序集合。

2.队列操作poll() 方法和 remove() 方法的区别？

poll() 和 remove() 都是从队列中取出一个元素，但是 poll() 在获取元素失败的时候会返回空，但是 remove() 失败的时候会抛出异常。

3.Java 中 LinkedHashMap 和 PriorityQueue 的区别是什么？

PriorityQueue 保证最高或者最低优先级的的元素总是在队列头部，但是 LinkedHashMap 维持的顺序是元素插入的顺序。当遍历一个 PriorityQueue 时，没有任何顺序保证，但是 LinkedHashMap 课保证遍历顺序是元素插入的顺序。

4.ArrayList 与 LinkedList 的不区别？

最明显的区别是 ArrrayList 底层的数据结构是数组，支持随机访问，而 LinkedList 的底层数据结构书链表，不支持随机访问。使用下标访问一个元素，ArrayList 的时间复杂度是 O(1)，而 LinkedList 是 O(n)。

5.用哪两种方式来实现集合的排序？

你可以使用有序集合，如 TreeSet 或 TreeMap，你也可以使用有顺序的的集合，如 list，然后通过 Collections.sort() 来排序。

6.Java 中的 LinkedList 是单向链表还是双向链表？

是双向链表，你可以检查 JDK 的源码。

7.Java 中的 TreeMap 是采用什么树实现的？

Java 中的 TreeMap 是使用红黑树实现的。(红黑树的数据结构)

8.Hashtable 与 HashMap 有什么不同之处？

这两个类有许多不同的地方，下面列出了一部分：

（1）HashMap允许key和value为null，而HashTable不允许。

（2）HashTable是同步的，而HashMap不是。所以HashMap适合单线程环境，HashTable适合多线程环境。

（3）在Java1.4中引入了LinkedHashMap，HashMap的一个子类，假如你想要遍历顺序，你很容易从HashMap转向LinkedHashMap，但是HashTable不是这样的，它的顺序是不可预知的。

（4）HashMap提供对key的Set进行遍历，因此它是fail-fast的，但HashTable提供对key的Enumeration进行遍历，它不支持fail-fast。

（5）HashTable被认为是个遗留的类，如果你寻求在迭代的时候修改Map，你应该使用CocurrentHashMap。

9.Java 中的 HashSet，内部是如何工作的？

HashSet 的内部采用 HashMap来实现。由于 Map 需要 key 和 value，所以所有 key 的都有一个默认 value。类似于 HashMap，HashSet 不允许重复的 key，只允许有一个null key，意思就是 HashSet 中只允许存储一个 null 对象。

10.写一段代码在遍历 ArrayList 时移除一个元素？

该问题的关键在于面试者使用的是 ArrayList 的 remove() 还是 Iterator 的 remove()方法。使用迭代器更加线程安全，因为它可以确保，在当前遍历的集合元素被更改的时候，它会抛出ConcurrentModificationException。

11.ArrayList 和 HashMap 的默认大小是多数？

在 Java 7 中，ArrayList 的默认大小是 10 个元素，HashMap 的默认大小是16个元素（必须是2的幂）。这就是 Java 7 中 ArrayList 和 HashMap 类的代码片段：

```
// from ArrayList.java JDK 1.7
private static final int DEFAULT_CAPACITY = 10;
 
//from HashMap.java JDK 7
static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
```

12.Java 中，Comparator 与 Comparable 有什么不同？

Comparable 接口用于定义对象的自然顺序，而 comparator 通常用于定义用户定制的顺序。Comparable 总是只有一个，但是可以有多个 comparator 来定义对象的顺序。

13.Java集合框架的基础接口有哪些？

Collection为集合层级的根接口。一个集合代表一组对象，这些对象即为它的元素。Java平台不提供这个接口任何直接的实现。

Set是一个不能包含重复元素的集合。这个接口对数学集合抽象进行建模，被用来代表集合，就如一副牌。

List是一个有序集合，可以包含重复元素。你可以通过它的索引来访问任何元素。List更像长度动态变换的数组。

Map是一个将key映射到value的对象.一个Map不能包含重复的key：每个key最多只能映射一个value。

一些其它的接口有Queue、Dequeue、SortedSet、SortedMap和ListIterator。

14.Iterator是什么？

Iterator接口提供遍历任何Collection的接口。我们可以从一个Collection中使用迭代器方法来获取迭代器实例。迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者在迭代过程中移除元素。

15.Enumeration和Iterator接口的区别？

Enumeration的速度是Iterator的两倍，也使用更少的内存。Enumeration是非常基础的，也满足了基础的需要。但是，与Enumeration相比，Iterator更加安全，因为当一个集合正在被遍历的时候，它会阻止其它线程去修改集合。
迭代器取代了Java集合框架中的Enumeration。迭代器允许调用者从集合中移除元素，而Enumeration不能做到。为了使它的功能更加清晰，迭代器方法名已经经过改善。

16.Iterater和ListIterator之间有什么区别？

（1）我们可以使用Iterator来遍历Set和List集合，而ListIterator只能遍历List。

（2）Iterator只可以向前遍历，而LIstIterator可以双向遍历。

（3）ListIterator从Iterator接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。

17.在迭代一个集合的时候，如何避免ConcurrentModificationException？

在遍历一个集合的时候，我们可以使用并发集合类来避免ConcurrentModificationException，比如使用CopyOnWriteArrayList，而不是ArrayList。

18.UnsupportedOperationException是什么？

UnsupportedOperationException是用于表明操作不支持的异常。在JDK类中已被大量运用，在集合框架java.util.Collections.UnmodifiableCollection将会在所有add和remove操作中抛出这个异常。

19.在Java中，HashMap是如何工作的？

HashMap在Map.Entry静态内部类实现中存储key-value对。HashMap使用哈希算法，在put和get方法中，它使用hashCode()和equals()方法。当我们通过传递key-value对调用put方法的时候，HashMap使用Key hashCode()和哈希算法来找出存储key-value对的索引。Entry存储在LinkedList中，所以如果存在entry，它使用equals()方法来检查传递的key是否已经存在，如果存在，它会覆盖value，如果不存在，它会创建一个新的entry然后保存。当我们通过传递key调用get方法时，它再次使用hashCode()来找到数组中的索引，然后使用equals()方法找出正确的Entry，然后返回它的值。
其它关于HashMap比较重要的问题是容量、负荷系数和阀值调整。HashMap默认的初始容量是32，负荷系数是0.75。阀值是为负荷系数乘以容量，无论何时我们尝试添加一个entry，如果map的大小比阀值大的时候，HashMap会对map的内容进行重新哈希，且使用更大的容量。容量总是2的幂，所以如果你知道你需要存储大量的key-value对，比如缓存从数据库里面拉取的数据，使用正确的容量和负荷系数对HashMap进行初始化是个不错的做法。
和HashMap一起问的会有ConcurrentHashMap的底层结构，关于这块他们俩的底层结构，之后会写一个源码分析出来。

20.hashCode()和equals()方法有何重要性？

HashMap使用Key对象的hashCode()和equals()方法去决定key-value对的索引。当我们试着从HashMap中获取值的时候，这些方法也会被用到。如果这些方法没有被正确地实现，在这种情况下，两个不同Key也许会产生相同的hashCode()和equals()输出，HashMap将会认为它们是相同的，然后覆盖它们，而非把它们存储到不同的地方。同样的，所有不允许存储重复数据的集合类都使用hashCode()和equals()去查找重复，所以正确实现它们非常重要。equals()和hashCode()的实现应该遵循以下规则：

（1）如果o1.equals(o2)，那么o1.hashCode() == o2.hashCode()总是为true的。

（2）如果o1.hashCode() == o2.hashCode()，并不意味着o1.equals(o2)会为true。

21.我们能否使用任何类作为Map的key？

我们可以使用任何类作为Map的key，然而在使用它们之前，需要考虑以下几点：

（1）如果类重写了equals()方法，它也应该重写hashCode()方法。

（2）类的所有实例需要遵循与equals()和hashCode()相关的规则。请参考之前提到的这些规则。

（3）如果一个类没有使用equals()，你不应该在hashCode()中使用它。

（4）用户自定义key类的最佳实践是使之为不可变的，这样，hashCode()值可以被缓存起来，拥有更好的性能。不可变的类也可以确保hashCode()和equals()在未来不会改变，这样就会解决与可变相关的问题了。

```
// 比如，我有一个类MyKey，在HashMap中使用它。
// 传递给MyKey的name参数被用于equals()和hashCode()中
MyKey key = new MyKey('Pankaj'); //assume hashCode=1234
myHashMap.put(key, 'Value');
// 以下的代码会改变key的hashCode()和equals()值
key.setName('Amit'); //assume new hashCode=7890
// 下面会返回null，因为HashMap会尝试查找存储同样索引的key，而key已被改变了，匹配失败，返回null
myHashMap.get(new MyKey('Pankaj'));
// 那就是为何String和Integer被作为HashMap的key大量使用。
```

22.如何决定选用HashMap还是TreeMap？

对于在Map中插入、删除和定位元素这类操作，HashMap是最好的选择。然而，假如你需要对一个有序的key集合进行遍历，TreeMap是更好的选择。基于你的collection的大小，也许向HashMap中添加元素会更快，将map换为TreeMap进行有序key的遍历。

23.BlockingQueue是什么？

Java.util.concurrent.BlockingQueue是一个队列，在进行检索或移除一个元素的时候，它会等待队列变为非空；当在添加一个元素时，它会等待队列中的可用空间。BlockingQueue接口是Java集合框架的一部分，主要用于实现生产者-消费者模式。我们不需要担心等待生产者有可用的空间，或消费者有可用的对象，因为它都在BlockingQueue的实现类中被处理了。Java提供了集中BlockingQueue的实现，比如ArrayBlockingQueue、LinkedBlockingQueue、PriorityBlockingQueue,、SynchronousQueue等。
(后续也会对队列这块做一个源码分析)

24.队列和栈是什么，列出它们的区别？

栈和队列两者都被用来预存储数据。java.util.Queue是一个接口，它的实现类在Java并发包中。队列允许先进先出（FIFO）检索元素，但并非总是这样。Deque接口允许从两端检索元素。
栈与队列很相似，但它允许对元素进行后进先出（LIFO）进行检索。
Stack是一个扩展自Vector的类，而Queue是一个接口。

25.我们如何对一组对象进行排序？

如果我们需要对一个对象数组进行排序，我们可以使用Arrays.sort()方法。如果我们需要排序一个对象列表，我们可以使用Collection.sort()方法。两个类都有用于自然排序（使用Comparable）或基于标准的排序（使用Comparator）的重载方法sort()。

26.当一个集合被作为参数传递给一个函数时，如何才可以确保函数不能修改它？

在作为参数传递之前，我们可以使用Collections.unmodifiableCollection(Collection c)方法创建一个只读集合，这将确保改变集合的任何操作都会抛出UnsupportedOperationException。

37.我们如何从给定集合那里创建一个synchronized的集合？

27.与Java集合框架相关的有哪些最好的实践？

（1）根据需要选择正确的集合类型。比如，如果指定了大小，我们会选用Array而非ArrayList。如果我们想根据插入顺序遍历一个Map，我们需要使用TreeMap。如果我们不想重复，我们应该使用Set。

（2）一些集合类允许指定初始容量，所以如果我们能够估计到存储元素的数量，我们可以使用它，就避免了重新哈希或大小调整。

（3）基于接口编程，而非基于实现编程，它允许我们后来轻易地改变实现。

（4）总是使用类型安全的泛型，避免在运行时出现ClassCastException。

（5）使用JDK提供的不可变类作为Map的key，可以避免自己实现hashCode()和equals()。

（6）尽可能使用Collections工具类，或者获取只读、同步或空的集合，而非编写自己的实现。它将会提供代码重用性，它有着更好的稳定性和可维护性。

---

> Spring框架

1.使用Spring框架的好处是什么？

* 轻量：Spring 是轻量的，基本的版本大约2MB。
* 控制反转：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
* 面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
* 容器：Spring 包含并管理应用中对象的生命周期和配置。
* MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
* 事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
* 异常处理：Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

2. 什么是Spring IOC 容器？

Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。

3.IOC的优点是什么？

IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。

4.ApplicationContext通常的实现是什么?

* FileSystemXmlApplicationContext ：此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。
* ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。
* WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。

5. 有哪些不同类型的IOC（依赖注入）方式？

* 构造器依赖注入：构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
* Setter方法注入：Setter方法注入是容器通过调用无参构造器或无参static工厂 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。

6.解释Spring支持的几种bean的作用域?

* singleton : bean在每个Spring ioc 容器中只有一个实例。(默认缺省)
* prototype：一个bean的定义可以有多个实例。
* request：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。
* session：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。
* global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

7.Spring框架中的单例bean是线程安全的吗?

不，Spring框架中的单例bean不是线程安全的。

8.解释Spring框架中bean的生命周期?

* Spring容器 从XML 文件中读取bean的定义，并实例化bean。
* Spring根据bean的定义填充所有的属性。
* 如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
* 如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
* 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
* 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
* 如果有BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用。
* 如果bean实现了 DisposableBean，它将调用destroy()方法。

9.哪些是重要的bean生命周期方法？ 你能重载它们吗？

有两个重要的bean 生命周期方法，第一个是setup ， 它是在容器加载bean的时候被调用。第二个方法是 teardown 它是在容器卸载类的时候被调用。

The bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）。

10.解释不同方式的自动装配 ?

有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入。

* no：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。
* byName：通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
* byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。
* constructor：这个方式类似于byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。
* autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。

11.各个注解的含义？

* @Required：这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。
* @Autowired：注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称多个参数的PN方法。
* @Qualifier：当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的bean。

12.Spring支持的事务管理类型?

* 编程式事务管理：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。
* 声明式事务管理：这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。

13.有几种不同类型的自动代理？

* BeanNameAutoProxyCreator
* DefaultAdvisorAutoProxyCreator
* Metadata autoproxying

14.Aspect 切面

AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在Spring AOP中，切面通过带有@Aspect注解的类实现。

15.在Spring AOP 中，关注点和横切关注的区别是什么？

* 关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。
* 横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

16.通知

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。

Spring切面可以应用五种类型的通知：

* before：前置通知，在一个方法执行前被调用。
* after: 在方法执行之后调用的通知，无论方法执行是否成功。
* after-returning: 仅当方法成功完成后执行的通知。
* after-throwing: 在方法抛出异常退出时执行的通知。
* around: 在方法执行之前和之后调用的通知。