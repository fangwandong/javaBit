### 定时任务
- https://moonbingbing.gitbooks.io/openresty-best-practices/content/ngx_lua/timer.html
- local new_timer = ngx.timer.at 


```nginx
lua_shared_dict td_cache 128m;
lua_shared_dict ducc 64m;

```

### lua 重启
```nginx
/export/servers/openresty/nginx/sbin/nginx -c /export/servers/openresty/nginx/conf/nginx.conf -s stop

```

### curl 验证

```nginx 

curl -H 'application:lapi-service' -H 'token:a4cb039519e7477c835ad4a1ac805971' "http://xxx.jd.con/v1/namespace/lapi_service/config/admin/profile/test/items"

```
General.lua:17