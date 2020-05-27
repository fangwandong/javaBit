- [前言](#前言)
  * [Swagger旧版本配置](#swagger旧版本配置)
      - [旧版最新版本配置](#旧版最新版本配置)
      - [非接口分组配置](#非接口分组配置)
      - [接口分组类配置](#接口分组类配置)
      - [Controller层使用swagger注解](#Controller层使用swagger注解)
      - [WebApplication启动SpringBoot类](#WebApplication启动SpringBoot类)
      - [接口访问](#接口访问)
  * [Swagger新版本knife4j配置](#Swagger新版本knife4j配置)

# 前言

> Swagger gitee地址： https://gitee.com/xiaoym/knife4j

> swagger-bootstrap-ui是基于swagger接口api实现的一套UI,因swagger原生ui是上下结构的，在浏览接口时不是很清晰,
> 所以，swagger-bootstrap-ui是基于左右菜单风格的方式,适用与我们在开发后台系统左右结构这种风格类似,方便与接口浏览
 
> GitHub:https://github.com/xiaoymin/Swagger-Bootstrap-UI

>用户手册：https://doc.xiaominfo.com/


## Swagger旧版本配置

#### 旧版最新版本配置
- 1.9.6 是最新版

```xml

<!-- swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- swagger2 UI -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- swagger UI 增强版 -->
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>swagger-bootstrap-ui</artifactId>
    <version>1.9.6</version>
</dependency>

```

#### 非接口分组配置
- 新增SwaggerConfiguration类,@EnableSwagger2主要注解
- 注意 .apis(RequestHandlerSelectors.basePackage("com.jd.water.web.controller")) 地址配置，如果配置错误。
- 打开 http://localhost:8043/doc.html 报404错误

```java

@Configuration
@EnableSwagger2
public class SwaggerConfiguration {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.xxx.water.web.controller"))  
                .paths(PathSelectors.any())
                .build();
    }

    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("swagger-bootstrap-ui RESTful APIs")
                .description("swagger-bootstrap-ui")
                // .termsOfServiceUrl("http://localhost:8043/")
                .version("1.0")
                .build();
    }
}

```

#### 接口分组类配置

```java

@Configuration
@EnableSwagger2
public class SwaggerConfiguration {
​
    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("资源管理")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.lishiots.dc.baseinfo.ctl"))
                .paths(PathSelectors.any())
                .build();
    }
    @Bean
    public Docket createMonitorRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("实时监测")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.lishiots.dc.monitor.ctl"))
                .paths(PathSelectors.any())
                .build();
    }
    @Bean
    public Docket createActivitiRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("工作流引擎")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.lishiots.dc.activiti.ctl"))
                .paths(PathSelectors.any())
                .build();
    }
​
    @Bean
    public Docket createBaseRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("kernel模块")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.lishiots.dc.kernel.ctl"))
                .paths(PathSelectors.any())
                .build();
    }
​
    @Bean
    public Docket createComplaintRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .groupName("投诉管理")
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.lishiots.dc.complaint.ctl"))
                .paths(PathSelectors.any())
                .build();
    }
​
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("swagger RESTful APIs")
                .description("swagger RESTful APIs")
                .termsOfServiceUrl("http://www.test.com/")
                .contact("xiaoymin@foxmail.com")
                .version("1.0")
                .build();
    }
​
}

```


#### Controller层使用swagger注解

```java

@Api(tags = "banner管理")
@RestController
@RequestMapping("/api/bannerInfo")
public class BannerCtl {
    @Autowired
    private BannerInfoService service;
​
    @PostMapping("/query")
    @ApiOperation(value = "查询banner",notes = "查询banner")
    public Pagination<BannerInfo> bannerInfoQuery(){
        Pagination<BannerInfo> pagination = service.bannerInfoQuery();
        return pagination;
    }
}

```

#### WebApplication启动SpringBoot类

```java

@EnableAsync
@SpringBootApplication(scanBasePackages = "com.xxx.water")
@ConditionalOnClass(SpringfoxWebMvcConfiguration.class)
@EnableSwagger2
public class WebApplication extends SpringBootServletInitializer {
    /**
     * Log4j2日志异步配置
     */
    static {
        System.setProperty("Log4jContextSelector",
                "org.apache.logging.log4j.core.async.AsyncLoggerContextSelector");
    }
    /**
     * run
     *
     * @param args args
     */
    public static void main(String[] args) {
        SpringApplication.run(WebApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        // 注意这里要指向原先用main方法执行的Application启动类
        return builder.sources(WebApplication.class);
    }
}

```

#### 接口访问
- 在浏览器输入：http://${host}:${port}/doc.html


## Swagger新版本knife4j配置
- knife4j是为Java MVC框架集成Swagger生成Api文档的工具,前身是swagger-bootstrap-ui
- 文档： http://doc.xiaominfo.com/

- 待更新