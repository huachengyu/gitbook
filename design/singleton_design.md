### 单例模式
@Date 2015.08.24

> 适用场景

* 单例对象能保证在一个JVM中,该对象只有一个实例存在
* 某些类创建比较频繁
* 某些资源类只能存在一个类控制

> 同步锁 & 懒加载

* 对方法加上synchronized关键字
* 每次需要加锁,效率低
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/LazySecSingleton.java)

> 双重同步锁 & 懒加载

* 非两步骤同步,减少第一种方式每次都加锁问题
* Java指令中创建对象和赋值操作是分开进行的,JVM不能保证分配内存和实例化对象的执行顺序
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/DoubleLazySecSingleton.java)

> 同步锁 & 非懒加载

* 类加载时即生成实例
* 由JVM classloader加载,保证类加载过程是互斥的,线程安全
* 若构造时抛出异常,则创建失败
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/SecSingleton.java)

> 内部类

* 由JVM classloader加载,线程安全
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/InnerSecClass.java)