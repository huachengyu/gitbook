### spring动态自定义logback日志目录
@Date 2018.10.18

#### 问题场景

> 在业务开发中, 遇到一个场景. 日志目录需要根据不同的一个业务id存储. 故需要动态存储logback的日志


#### 解决办法

> 在springboot中, 或者spring相关框架中, 可以通过实现logback的PropertyDefinerBase方法来动态决定日志目录.

```java
// 通过实现logback的PropertyDefinerBase方法,动态定义logback配置中的变量
@Component
public class DefineDir extends PropertyDefinerBase {

    @Override
    public String getPropertyValue() {
        return "动态参数";
    }
}
```

```xml
<configuration>

    // 通过DefineDir类映射自定义变量, 实现动态修改logback的日志目录
    <define  name="dirXxx" class="com.xxx.DefineDir" />

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>../logs/${dirXxx}/info.log</file>
        <encoder>
            <charset>UTF-8</charset>
            <pattern>%d{yyyy/MM/dd HH:mm:ss.SSS} [%thread] [%X{requestId}] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <root level="INFO">
        <appender-ref ref="FILE" />
    </root>
</configuration>

```