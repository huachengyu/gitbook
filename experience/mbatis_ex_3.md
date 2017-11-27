### Mbatis if 判断的坑
@Date 2016.11.13

> 问题现象

* 我们一般在Mbatis中做Update更新时、都会加上使用if test判断、防止更新空值、一般正常都是像name这种既判断NULL又判断''
* 但是对于int类型的sex字段、如果加上 sex != '' 条件时、当sex值为0时、此sql不会被拼加上、也不会被执行更新

```
<if test="name !=null and name !='' ">
    name = #{name},
</if>
# 对于int变量的正确判断姿势
<if test="sex !=null">
    sex = #{sex},
</if>
```

> 原因分析

* 根据以上问题现象、现在让我们来看一下Mbatis源码、为什么会出现这种情况

    * 1: 首先在mbatis中找到啊以上包路径、此路径为解析script标签时用到的类
    
    ```
    org.apache.ibatis.scripting
    ```
    
    * 2.1: 过程不在细说、对于mbatis来说、script标签的动态SQL、都是遍历条件做SQL拼接而成的
    * 2.2: 我们直接看org.apache.ibatis.scripting.xmltags.IfSqlNode这个类、此类是script标签中if标签的判断方式、其中evaluateBoolean返回true时、if标签下的SQL拼接生效
    
    ```
    public boolean apply(DynamicContext context) {
        if (evaluator.evaluateBoolean(test, context.getBindings())) {
            contents.apply(context);
            return true;
        }
        return false;
    }
    ```
    
    * 3: 再具体看org.apache.ibatis.scripting.xmltags.ExpressionEvaluator ：evaluateBoolean 这个方法、我们看到了如果是number类型、会与0做判断比较
    
    ```
    public boolean evaluateBoolean(String expression, Object parameterObject) {
        Object value = OgnlCache.getValue(expression, parameterObject);
        if (value instanceof Boolean) return (Boolean) value;
        if (value instanceof Number) return !new BigDecimal(String.valueOf(value)).equals(BigDecimal.ZERO);
        return value != null;
    }
    ```
    
    * 4.1: 由此可以发现、当动态sql参数是int类型、并且传入0时、是使用OgnlCache获取的value、而OgnlCache是对OGNL的封装
    * 4.2: 在[OGNL的官网](https://commons.apache.org/proper/commons-ognl/language-guide.html)上面可以看到如下解释
    
    ```
    Any object can be used where a boolean is required. OGNL interprets objects as booleans like this:
    · If the object is a Boolean, its value is extracted and returned;
    · If the object is a Number, its double-precision floating-point value is compared with zero; non-zero is treated as true, zero as false;
    · If the object is a Character, its boolean value is true if and only if its char value is non-zero;
    Otherwise, its boolean value is true if and only if it is non-null.
    ```
    
> 结论

* 因此在对整数型变量做判断时,只判断NULL即可,不必多做空判断