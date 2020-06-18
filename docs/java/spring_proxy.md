# 前言
> AOP的技术是基于代理（proxy），整理下代理相关知识。
 
> proxy作用：扩展已经上线运行代码的功能。


### 静态代理
- 创建 IUserDao接口
```java

public interface IUserDao {
    void save();
}

```
- 创建实现类UserDao

```java

public class UserDao implements IUserDao {
    @Override
    public void save() {
        System.out.println("----------执行保存方法-----------");
    }
}

```

- UserDaoProxy代理类，代理 UserDao，增加UserDao功能

```java

public class UserDaoProxy implements IUserDao {

    private IUserDao target;

    //生产构造方法
    public UserDaoProxy(IUserDao target) {
        this.target = target;
    }

    @Override
    public void save() {

        System.out.println("新添加功能代码1");

        //接口调用方法--接口的实现类调用
        target.save();

        System.out.println("新添加代码功能2");
    }
}

```

- 运行类

```java

public class StaticProxyRun {
    public static void main(String[] args) {

        //父类引用指向子类对象--> 多态
        IUserDao iud = new UserDao();
        iud.save();

        System.out.println("=================");

        IUserDao proxy = new UserDaoProxy(iud);
        proxy.save();
    }
}

```


### 动态代理

- 创建动态代理工厂

```java

public class DynamicFactory {
    private Object target;

    public DynamicFactory(Object target) {
        this.target = target;
    }

    //生产代理对象
    public Object getProxyInstance(){
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("动态代理执行之前功能代码1");
                Object returnValue = method.invoke(target,args);
                System.out.println("动态代理执行功能代码之后2");
                return returnValue;
            }
        });
    }
}

```

- 运行类

```java
public class DynamicProxyRun {

    public static void main(String[] args) {
        IUserDao target = new UserDao();
        System.out.println(target.getClass());

        IUserDao proxy = (IUserDao) new DynamicFactory(target).getProxyInstance();
        System.out.println(proxy.getClass());
        proxy.save();
    }
}

```
