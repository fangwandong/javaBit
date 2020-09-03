

# redis队列

```
- rpush  写入队列  rpush key value1 value2 ===  rpush xiaofei 111 222 333
- lpop   消费队列  lpop key   ===  lpop  xiaofei 

```

# redis哈希

```
- hset 写入   hset key field value ===  hset haxi field1 1111
- hget 读取   hget key field  === hget haxi field1
```

# 自增

```
- incr key  === incr aaa
```

# 随机返回一个key

```
- randomkey  
```

# rpush lpush 区别

- 2个命令都是属于无序集合的范畴，并不是有序集合zset等，顾知晓！

```

1.lpush

从左往右添加元素

在key 对应 list的头部添加字符串元素

```


```

2.rpush

从右到左添加元素

在key 对应 list 的尾部添加字符串元素

```


https://www.cnblogs.com/sdgf/p/6244937.html