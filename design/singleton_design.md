### 单例模式
@Date 2015.08.24

> 同步锁 & 懒加载
* 对方法加上synchronized关键字
* 每次需要加锁,效率低
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/LazySecSingleton.java)

> 同步锁 & 非懒加载
* 类加载时即生成实例
* 由JVM classloader加载,线程安全
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/SecSingleton.java)

> 双重同步锁 & 懒加载
* 非两步骤同步,减少第一种方式每次都加锁问题
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/DoubleLazySecSingleton.java)

> 内部类
* 由JVM classloader加载,线程安全
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/blob/master/src/main/java/com/algorithm/demo/design/singleton/InnerSecClass.java)