### 工厂模式
@Date 2015.12.05

* 适用场景: 出现大量产品需要创建,并且具有共同的接口时,可以通过工厂方法模式进行创建

> 简单工厂模式
* 由一个接口类或者抽象类定义
* 具体不同的实现类实现接口动作
* 有一个工厂类负责生成具体类
* 若创建时传递的type错误,则不能创建对象
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/tree/master/src/main/java/com/algorithm/demo/design/factory/simplefactory)

> 多个方法工厂模式
* 对比简单工厂模式,取消传type参数,改为对应多个创建方法
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/tree/master/src/main/java/com/algorithm/demo/design/factory/multifactory)

> 静态方法工厂模式
* 对比多个方法工厂模式,把工厂创建实力方法改为静态的
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/tree/master/src/main/java/com/algorithm/demo/design/factory/staticfactory)

> 抽象工厂模式
* 上面几种方式,如果扩展程序,需要修改工厂类,违背了闭包原则
* 因此出现抽象工厂模式,需要扩展时就创建新的工厂类
* [DEMO代码链接](https://github.com/huachengyu/algorithm-demo/tree/master/src/main/java/com/algorithm/demo/design/factory/abstractfactory)
