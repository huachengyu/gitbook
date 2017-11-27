### Caused by: org.apache.ibatis.binding.BindingException: Parameter ‘__frch_callRecord_0’ not found.
@Date 2016.11.13

> 异常现象

* 使用mbatis批量插入时、出现如下异常：

```
Caused by: org.apache.ibatis.binding.BindingException: Parameter '__frch_callRecord_0' not found.
```

> 解决思路

* Mbatis对此类问题的异常描述不是很清晰,如出现上诉异常，主要检查以下几个方面
    * 批量插入的List对象中字段是否和数据库表中字段名一致并且都存在
    * 批量插入的List对象中是否有NULL的对象(此原因很重要)
    * 在Mbatis的XML或者注解的Sql语句中,是否传入的表字段和list字段一致