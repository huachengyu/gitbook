### Java动态代理
@Date 2016.11.24

> 静态代理

* 提前创建一个代理类,实现和业务逻辑同样的接口
* 传递业务实现类的对象,在调用业务方法前后可以做代理的逻辑
* 扩展原有功能,不侵入原有代码
* 如果需要代理的业务类变多,并且实现方法不一样的情况下,对应的代理类会增多

> Java本身动态代理

* 只能代理接口
* 实现java.lang.reflect.InvocationHandler接口
* 通过Proxy.newProxyInstance (obj.getClass().getClassLoader(),obj.getClass().getInterfaces(), InvocationHandler invocationHandler);创建代理类
* 虚拟机自动调用invoke方法

> cglib动态代理

* 运行期间动态生成Java字节码
* 不用实现接口,直接底层生成代理类覆盖父类中的方法
* 实现MethodInterceptor接口的intercept方法
* 调用重写方法比JDK本身的代理速度快
* 加载cglib耗时比JDK本身的时间长
* 不适合反复动态生成新的代理类

