
- [前言](#前言)
  * [SpringJDBC](#SpringJDBC)
  * [SpringJDBCTemplate](#SpringJDBCTemplate)
  * [Spring和JDBC整合](#Spring和JDBC整合)
  * [Spring操作数据库(JDBCTemplate)](#Spring操作数据库(JDBCTemplate))

# 前言
> 主要记录Spring框架相关 Spring JDBC和 Spring JDBCTemplate，以及相关的实操代码。

## SpringJDBC
- JDBC: Java Database Connectivity: java数据库连接

## SpringJDBCTemplate

- JDBCTemplate： JDBC提供操作数据库的工具类


## Spring和JDBC整合
**① 引入Mysql驱动**

```xml
    <!--Mysql驱动-->
    <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.38</version>
    </dependency>

```


**② 引入Spring和JDBC的整合包**

```xml

    <!--Spring整合JDBC包-->
    <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>4.3.18.RELEASE</version>
    </dependency>

```


**③ Spring配置JDBC**
- 3-1：mysql数据源配置
- 3-2：spring操作数据库工具类 JDBCTemplate配置

```xml

    <!-- Spring整合JDBC的固定配置 -->
    <!--
    1. 配置数据源DriverManagerDataSource
    2. driverClassName: Mysql驱动包路径
    3. url: 数据库地址
    4. username: 数据库用户名
    5. password: 数据库密码
    -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://192.168.229.100:3306/springjdbc"/>
        <property name="username" value="root"/>
        <property name="password" value="xxxxxx"/>
    </bean>
    <!-- 配置Spring操作数据库工具类，JDBCTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--数据操作层注入jdbcTemplate-->
    <bean id="userDao" class="com.jd.springjdbc.UserDao">
        <property name="jdbcTemplate" ref="jdbcTemplate"/>
    </bean>

```

## Spring操作数据库(JDBCTemplate)
**④ 创建UserDao类，通过JDBCTemplate进行CURD**
```java

public class UserDao {

    private JdbcTemplate jdbcTemplate;

    public JdbcTemplate getJdbcTemplate() {
        return jdbcTemplate;
    }

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void insert(UserDomain userDomain){
        String sql = "insert into springuser values(null,?,?,?)";
        int result = jdbcTemplate.update(sql,new Object[]{userDomain.getAccount(),
                userDomain.getName(), userDomain.getPwd()});
        if (result > 0){
            System.out.println("插入成功");
        }else {
            System.out.println("插入失败");
        }
    }

    public void delete(int userid){
        String sql = "delete from springuser where id = ?";
        int result = jdbcTemplate.update(sql, new Object[]{userid});
        if (result > 0){
            System.out.println("删除成功");
        }else {
            System.out.println("删除失败");
        }
    }

    public List<UserDomain> getAllUser(){
        String sql = "select * from springuser";
        List<UserDomain> userDomainList = jdbcTemplate.query(sql,new BeanPropertyRowMapper<UserDomain>(UserDomain.class));
        return userDomainList;
    }

    public void update(String name, int id){
        String sql = "update springuser set name = ? where id = ? ";
        int result = jdbcTemplate.update(sql, name, id);
        if (result > 0){
            System.out.println("更新成功");
        }else {
            System.out.println("更新失败");
        }
    }
}

```

**⑤ 创建UserDomain实体类**

```java
public class UserDomain {

    private String account;
    private String pwd;
    private String name;

    public String getAccount() {
        return account;
    }

    public void setAccount(String account) {
        this.account = account;
    }

    public String getPwd() {
        return pwd;
    }

    public void setPwd(String pwd) {
        this.pwd = pwd;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public UserDomain() {
    }

    @Override
    public String toString() {
        return "UserDomain{" +
                "account='" + account + '\'' +
                ", pwd='" + pwd + '\'' +
                ", name='" + name + '\'' +
                '}';
    }
}

```

**⑥ 运行类测试类**

```java
public class SpringJdbcRun {

    public static void main(String[] args) {
        ApplicationContext app = new ClassPathXmlApplicationContext("applicationContext-jdbc.xml");
        UserDao userDao = (UserDao) app.getBean("userDao");

        // 新增
        // UserDomain userDomain = new UserDomain();
        // userDomain.setAccount("7777");
        // userDomain.setName("565");
        // userDomain.setPwd("123");
        // userDao.insert(userDomain);

        // 删除
        // userDao.delete(3);

        // 查询
        // System.out.println(userDao.getAllUser());

        // 更新
        // userDao.update("6666", 5);
    }
}

```

**⑦ 数据库建表语句**

```Mysql

CREATE TABLE `springuser` (
  `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
  `account` varchar(255) DEFAULT NULL,
  `pwd` varchar(255) DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8

```