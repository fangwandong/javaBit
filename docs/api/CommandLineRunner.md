# spring boot 启动加载数据 CommandLineRunner 接口
- 实际应用中，我们会有在项目服务启动的时候去加载一些数据或者一些事情这样的需求。
- 为了解决这个问题，spring-boot为我们提供了一个方法，通过实现接口 CommandLineRunner来实现。


## 创建一个实现类
1. @Order 注解的执行优先级是按value值从小到大顺序。


```java
@Slf4j
@Component
@Order(value = 1)
public class ApplicationStartupRunner implements CommandLineRunner {

    @Resource
    private CommonProperties commonProperties;

    @Override
    public void run(String... args) throws Exception {
        log.info(">>>>>>>>>>>>>  http://localhost:{}/doc.html  <<<<<<<<<<<<<", commonProperties.getPortVal());
    }
}

```

## 读取yml配置数据CommonProperties
1. ConfigurationProperties 配置文件前缀

```java

@Component
@ConfigurationProperties("common")
@Getter
@Setter
public class CommonProperties {

    @Value("${server.port}")
    private String portVal;
}

```


## yml 配置如下

```xml

server:
  port: 8043
  ##servlet:
    ##context-path: /demo

```

