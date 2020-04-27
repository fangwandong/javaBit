# 前言

[[toc]]

>log4j2是log4j的升级版，并提供了许多logback改进的地方。
## 日志格式汇总

| 日志类型 | 描述  | 
| :---- | :---- |
|sync|同步打印日志，日志输出与业务逻辑在同一线程内，当日志输出完毕，才能进行后续业务逻辑操作|
|Async Appender | 异步打印日志，内部采用ArrayBlockingQueue, 对每个AsyncAppender创建一个线程用于处理日志输出。|
|Async Logger| 异步打印日志，采用了高性能并发框架Disruptor，创建一个线程用于处理日志输出。| 

## 1、排除Logback依赖
- Spring Boot 2.x默认使用Logback日志框架，要使用 Log4j2必须先排除 Logback。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
         <!--排除logback-->
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

```

## 2、引入Log4j2依赖

```xml
<!--配置异步日志提高性能 依赖-->
<dependency>
    <groupId>com.lmax</groupId>
    <artifactId>disruptor</artifactId>
    <version>3.3.7</version>
</dependency>

<!--log4j2 依赖-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

## 3、配置Log4j2
- 创建log4j2.xml文件，放在工程resources目录里。
- 下面是一份比较详细的 log4j2 配置文件, 只需要修改 LOG_HOME、 LOG_FILE_NAME 成自己的路径即可。

```xml

<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status，这个用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，你会看到log4j2内部各种详细输出-->
<!--monitorInterval：Log4j能够自动检测修改配置 文件和重新配置本身，设置间隔秒数-->

<!-- Don't forget to set system property
-Dlog4j2.contextSelector=org.apache.logging.log4j.core.async.AsyncLoggerContextSelector
     to make all loggers asynchronous. -->
<configuration status="WARN" monitorInterval="30">

    <properties>
        <property name="LOG_HOME">/export/Logs/water.xxx.com</property>
        <property name="LOG_FILE_NAME">water-web</property>
    </properties>

    <!--先定义所有的appender-->
    <appenders>
        <!--这个输出控制台的配置-->
        <console name="Console" target="SYSTEM_OUT">
            <!--输出日志的格式-->
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
        </console>
        <!--文件会打印出所有信息，这个log每次运行程序会自动清空，由append属性决定，这个也挺有用的，适合临时测试用-->
        <File name="log" fileName="log/test.log" append="false">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} %-5level %class{36} %L %M - %msg%xEx%n"/>
        </File>
        <!-- 这个会打印出所有的info及以下级别的信息，每次大小超过size，则这size大小的日志会自动存入按年份-月份建立的文件夹下面并进行压缩，作为存档-->
        <RollingFile name="RollingFileInfo" fileName="${LOG_HOME}/${LOG_FILE_NAME}-info.log"
                     filePattern="${LOG_HOME}/${LOG_FILE_NAME}/$${date:yyyy-MM}/info-%d{yyyy-MM-dd}-%i.log">
            <!--控制台只输出level及以上级别的信息（onMatch），其他的直接拒绝（onMismatch）-->
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
        </RollingFile>
        <RollingFile name="RollingFileWarn" fileName="${LOG_HOME}/${LOG_FILE_NAME}-warn.log"
                     filePattern="${LOG_HOME}/${LOG_FILE_NAME}/$${date:yyyy-MM}/warn-%d{yyyy-MM-dd}-%i.log">
            <ThresholdFilter level="warn" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
            <!-- DefaultRolloverStrategy属性如不设置，则默认为最多同一文件夹下7个文件，这里设置了20 -->
            <DefaultRolloverStrategy max="20"/>
        </RollingFile>
        <RollingFile name="RollingFileError" fileName="${LOG_HOME}/${LOG_FILE_NAME}-error.log"
                     filePattern="${LOG_HOME}/${LOG_FILE_NAME}/$${date:yyyy-MM}/error-%d{yyyy-MM-dd}-%i.log">
            <ThresholdFilter level="error" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
        </RollingFile>
    </appenders>
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <loggers>
        <!--过滤掉spring和mybatis的一些无用的DEBUG信息-->
        <logger name="org.springframework" level="INFO"/>
        <logger name="org.mybatis" level="INFO"/>
        <root level="all">
            <appender-ref ref="Console"/>
            <appender-ref ref="RollingFileInfo"/>
            <appender-ref ref="RollingFileWarn"/>
            <appender-ref ref="RollingFileError"/>
        </root>
    </loggers>
</configuration>

```

## 4、调用Logger输出日志
- 下面的示例代码使用了神器lombok中的@Slf4j 注解可以很方便的使用 org.slf4j.Logger 对象。日常开发尽量使用Slf4j门面来处理日志，尽量避免使用具体的日志框架。

```java

package com.xxx.water.web.controller;

import com.xxx.water.common.result.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.time.LocalDate;

/**
 * 包名：com.xxx.water.web.controller
 * 类名：StringBuilderController
 * 描述：stringBuilder
 *
 * @author fangwandong
 * @date 2020-05-01 15:43
 **/

@Slf4j
@RestController
@RequestMapping("/string")
public class StringBuilderController {


    /**
     * test log
     * @return null
     */
    @GetMapping(value = "/test")
    public Result testString() {
        StringBuilder sb = new StringBuilder();
        log.info("log4j2 test date: {}  info: {}", LocalDate.now(), "日志test");
        sb.append("abc").append(true).append(123);//直接添加内容
        System.out.println(sb.toString());
        log.warn("warn test: {}", LocalDate.now());
        log.error("error log test:{}", sb);
        return Result.of(sb);
    }
}

```

## 全量异步日志配置 
- 在第二步新增了 disruptor 依赖之后，新增异步日志提高性能。
- 在webApplication配置Log4j2异步日志，提高性能

```java

    /**
     * Log4j2日志异步配置
     */
    static {
        System.setProperty("Log4jContextSelector",
                "org.apache.logging.log4j.core.async.AsyncLoggerContextSelector");
    }

```

## 异步log4j2中location信息打印问题
- 全量异步日志，不支持 位置信息
- 位置信息包括： 或者表达式%C或%class、%F或%file、%l或%location、%L或%line、%M或%method
- 解决办法： 新增 includeLocation="true"
- 其他解决办法： 手动写本地日志
- 缺点： log4j2将会获取堆栈的快照（snapshot），并遍历堆栈跟踪以查找位置信息，因此会消耗较多的时间。
  比同步logger慢1.3到5倍，同步日志记录器在获取堆栈快照之前会等待尽可能长的时间，如果不需要位置,那么快照将永远不会被捕获。

```xml

<Root level="info" includeLocation="true">
  <AppenderRef ref="RandomAccessFile"/>
</Root>

```

## 审计日志
- 如果必须要本地审计日志，可以采取同步异步混合
- 官方给出了一个混合异步的配置例子：
```xml

<?xml version="1.0" encoding="UTF-8"?>

<!-- No need to set system property "log4j2.contextSelector" to any value
    when using <asyncLogger> or <asyncRoot>. -->

<Configuration status="WARN">
 <Appenders>
   <!-- Async Loggers will auto-flush in batches, so switch off immediateFlush. -->
   <RandomAccessFile name="RandomAccessFile" fileName="asyncWithLocation.log"
             immediateFlush="false" append="false">
     <PatternLayout>
       <Pattern>%d %p %class{1.} [%t] %location %m %ex%n</Pattern>
     </PatternLayout>
   </RandomAccessFile>
 </Appenders>
 <Loggers>
   <!-- pattern layout actually uses location, so we need to include it -->
   <AsyncLogger name="com.foo.Bar" level="trace" includeLocation="true">
     <AppenderRef ref="RandomAccessFile"/>
   </AsyncLogger>
   <Root level="info" includeLocation="true">
     <AppenderRef ref="RandomAccessFile"/>
   </Root>
 </Loggers>
</Configuration>

```
