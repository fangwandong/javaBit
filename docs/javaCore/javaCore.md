# 前言
>  JavaCore读书笔记


## Java程序设计概述

1. 面向对象： Java数据（面向对象）和接口
2. 分布式： Java处理http，FTP之类的ICP/IP协议
3. 安全性： Java沙箱机制
4. 体系结构中立：Java编译器， 将执行最频繁的字节码翻译成机器码，这一过程称为编译。
5. 可移植性：
- 数据类型大小明确 
- 处理文件
- 正则表达式
- XMl
- 时间和日期
- 数据库 
- 网络连接
- 线程
6. 解释型： 对于开发就能看的效果的语言（Python）我认为是一个缺点。
7. 高性能： 动态翻译字节码
8. 多线程： 多线程可以带来更好的交互响应和实时行为。
9. 动态性： 库中可以自由添加新方法和实例变量。


## Java基本程序设计结构
- 变量
使用 Character.isJavaIdentifierStart 和 isJavaIdentifierPart方法检测是否属于java中的字母
int box=1；
- 常量
final 关键字指示常量, 一旦被赋值之后，就不能在更改了。

```java
final double CM_PER_INCH = 3.23;
```
- 类常量, 在main方法的外部
```java

public static final double CM_PER_INCH = 3.23;
public static void main(){

}

```

- 字符串
1. 字符串拼接
```java

    public static void main(String[] args) {
        String hello = "Hello";
        String world = "world";
        String groupString = hello + world;
        System.out.printf(groupString);
        https://

out Helloworld
    }

```

2. 字符串截取

```java

    public static void main(String[] args) {
        String hello = "Hello";
        String sub = hello.substring(0, 4);
        System.out.printf(sub);
        https://

out Hell
    }

```

3. 字符串相等

```java

    public static void main(String[] args) {
        String hello = "Hello";
        String world = "Hello";
        boolean test = hello.equals(world);
        System.out.printf("ddd" + test);
    }

```

4. 空串和Null串
- 空串 "" 是长度为0的字符串， 内容为空
- 验证既不是空也不是null

```java

    public static void main(String[] args) {
        String str = "";
        String strNull = null;
        if (strNull != null && str.length() != 0) {
            System.out.printf("非空串");
        } else {
            System.out.printf("空串");
        }
    }


```
5. 构建字符串 StringBuilder
```java

    public static void main(String[] args) {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.append("Hello");
        stringBuilder.append("world");
        String builder = stringBuilder.toString();
        System.out.printf(builder);

    }

```







Unicode编码： http:https://

www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html
=======================================================================================






