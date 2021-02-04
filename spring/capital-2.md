# 第二章 spring5.x与日志框架的整合

spring与日志框架的整合，日志框架就可以在控制台中，输出spring框架运行
过程中一些重要信息，
好处：便于了解spring框架的运行过程，利于程序的调试。

- spring 如何整合日志框架，
    * spring 1,2,3 早期都是整合的common-logging.jar
    * spring5.x默认整合的日志框架logback,log4j2
- 如果不想使用spring的默认的日志框架，比如像整合的是log4j,需要引入要适应的依赖
```
* pom 
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.25</version>、
    </dependency>
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>

* log4j.properties

    #resource  根目录下
    ## 配置日志根目录
    log4j.rootLogger = debug.console
    
    ### 设置日志输出到控制台显示

    log4j.appender.console=org.apache.log4j.ConsoleAppender
    log4j.appender.console.Target=System.out
    log4j.appender.console.layout=org.apache.log4j.PatternLayoutlog4j.appender.console.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss}%-5p %c{1}:%L - %m%n

```
