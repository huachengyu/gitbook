### Spring分布式Session管理
@Date 2017.05.07

> 利用spring-session & MySql实现持久化session共享

* WEB应用中很多情况需要对session有统一的管理,现在分布式的情况下,多台机器的访问session更需要一个统一管理的地方,如果自己实现的话工作量会很大.
* 网上有很多spring-session的介绍,大多数都是和redis结合使用的,本文主要介绍和MySql结合的使用方式.
* GitHub地址：https://github.com/spring-projects/spring-session
* 本文讲解利用XML配置方式,因为配置MySql一定会配置一个DataSource,而spring-session就算利用DataSource在mysql里存储session.

```
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session</artifactId>
    <version>1.2.1.RELEASE</version>
</dependency>
```

```
<!-- Spring Session 配置   start-->
<bean class="org.springframework.session.jdbc.config.annotation.web.http.JdbcHttpSessionConfiguration"/>

<bean class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <constructor-arg ref="dataSource"/>
</bean>
<!-- Spring Session 配置   end-->
```

* 如上,在bean中引入项目配置的DataSource即可,spring-session会自动创建管理session的表以及操作.