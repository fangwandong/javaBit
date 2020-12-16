# 简介
> okHttp使用

- GET请求
```
//请求方法：Get，请求url
        Request request = new Request.Builder()
                .url(seckillUpUrl)
                .build();
        try {
            //配置client，设置超时时间
            OkHttpClient client = new OkHttpClient().newBuilder()
                    .connectTimeout(2000, TimeUnit.MILLISECONDS)
                    .readTimeout(90L, TimeUnit.SECONDS)
                    .build();
            //执行请求
            Response response = client.newCall(request).execute();
            IndexSeckillDTO indexSeckillDTO = JSONObject.parseObject(response.body().string(), IndexSeckillDTO.class);
            System.out.println(indexSeckillDTO);

        } catch (IOException e) {
            log.error(e.getMessage(), e);
        }

```

## okHttp的坑

1. okHttp没有返回JSON数据
- 原因：response.body().toString() 替换成 response.body().string()
- 参考：  https://blog.csdn.net/fool2009/article/details/52959363

2. okhttp异常： java.lang.IllegalStateException: closed
- 原因：这个错误是由于response.body().string()调用了多次导致的，string()仅可调用一次。
- 在调用了response.body().string()方法之后，response中的流会被关闭，我们需要创建出一个新的response给应用层处理。
- 参考：https://blog.csdn.net/ucxiii/article/details/52447945

