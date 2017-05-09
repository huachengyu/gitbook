### logback自定义Appender和Layout
@Date 2017.05.09

* Appender是logback中最重要的组件之一,它委托encoder组件来完成LoggingEvent的格式化和记录,具体源码分析网上有很多, 本文主要是应用实践.
* Layout组件来将LoggingEvent进行格式化，返回一个String，然后通过OutputStream.write()方法，把格式化之后的日志信息写到目的地.

> 继承RollingFileAppender,重写输出文件格式

1. 场景

工程中期望输出到日志中的是JSON格式, 但是在业务中log.info(变量),此变量会有带引号的情况下, 会影响最终输出结构,因为重写file appender.

2. logback.xml

```
<encoder>-->
    <charset>UTF-8</charset>
    <pattern>{"time":"${bySecond}","level":"%-5level","msg":"%msg"} %n</pattern>-->
</encoder>
```

%msg是logback输出的日志内容,此字符串如果本身带引号,则造成输出到日志文件里的JSON格式无法解析.

3. 解决办法

* 集成logback默认的RollingFileAppender,获取msg内容并做格式化替换
* 在logback.xml中<appender class="替换自定义的Appender"/>

```
public class UserFileAppender extends RollingFileAppender<ILoggingEvent> {
    @Override
    protected void subAppend(ILoggingEvent event){
        // 获取event中的message内容
        event.getFormattedMessage().replace("\"","\\\"");
        start();
        super.subAppend(event);
    }
}
```

> 进阶:异步处理log.xxxx()日志

1. 场景

当在程序中输出了一些日志,并期望对这些日志内容做特定处理(存储DB/解析发送消息等)

2. 集成UnsynchronizedAppenderBase

```
public class TransportDBLoggerAppender extends UnsynchronizedAppenderBase<ILoggingEvent> {
    
    @Override
    public void append(ILoggingEvent event) {
        try {
            String content = event.getFormattedMessage();
            Map<String, String> map = new HashMap<>();
            map.put("LOG_LEVEL", event.getLevel().levelStr);
            map.put("CONTENT", content.replace("'", "''"));
            // 处理逻辑
        } catch (Exception e) {
            System.err.println(e);
        }
    }
}
```

* 上述代码继承了UnsynchronizedAppenderBase,重写了append方法,里面可以根据event对象获取log中的内容和默认的系统提供的参数(level等),可以在此做各种业务逻辑, 此内容是异步操作,节省我们在工程上自己构建异步日志收集等工作量,适合小应用的场景.

> Layout:自定义日志输出格式

1. 场景

在上述场景中发现只输出msg还不够满足业务的需求,业务里有需要把MDC存储的上下文也加到输出的JSON日志中,因此使用了layout自定义输出.

2. logback.xml

```
<encoder class="ch.qos.logback.core.encoder.LayoutWrappingEncoder">
    <layout class="自定义layout类" />
</encoder>
```

3. Java

```
public class LoggingConsoleLayout extends LayoutBase<ILoggingEvent> {

    @Override
    public String doLayout(ILoggingEvent event) {
        StringBuilder sbuf = new StringBuilder();
        if (null != event && null != event.getMDCPropertyMap()) {
            sbuf.append("{");

            sbuf.append("\"time\":\"");
            sbuf.append(System.currentTimeMillis());
            sbuf.append("\",");

            sbuf.append("\"level\":\"");
            sbuf.append(event.getLevel());
            sbuf.append("\",");

            sbuf.append("\"tag\":\"");
            sbuf.append(event.getMDCPropertyMap().get("tag"));
            sbuf.append("\",");

            sbuf.append("\"msg\":\"");
            sbuf.append(event.getFormattedMessage().replace("\"", "\\\""));
            sbuf.append("\",");

            sbuf.append("\"source\":\"dialog\"} \n");
        }

        return sbuf.toString();
    }
}
```

* 利用自定义的encoder layout,输出程序中存在的各种变量,输出不同需求自定义的日志格式