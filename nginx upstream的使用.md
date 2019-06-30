# nginx    
## 配置集群    
+ 默认配置   
1. 在server前配置ip和端口号    
如:upstream linuxidx {
    server 127.0.0.1:8080;
    server 127.0.0.1:8082;
}
2. 将server节点下的location节点中的proxy_pass配置为http://+upstream名称,即    
location/{
    root html;
    index index.html index.htm;
    proxy_pass http://linuxidc;
}
3. 这种方式配置的是采用默认轮询的方式来进行负载,适用于图片服务器集群和纯静态页面集群   
+ 配置权重(weight)   
1.指定轮询几率,weight和访问比率成正比,用于后端服务器性能不均的情况.    
例如:
 upstream linuxidc{
     server 127.0.0.1 weight=5
     server 127.0.01 weight=10
 }    
+ ip_hash(访问ip)   
1.每一个请求按访问ip的hash结果分配.这样每一个访客固定访问一个后端服务器,能够解决session的问题    
例如:
upstream faversin{
    ip_hash;
    server 10.0.0.10:8080;
    server 10.0.0.11:8080;    
}    
+ fair(第三方)    
1.按后端服务器的响应时间来分配请求,响应时间短的优先分配    
例如:
upstream favresin{
    server 10.0.0.10:8080;
    server 10.0.0.11:8080;
    fair;
}      
+ url_hash(第三方)     
按访问url的hash结果来分配请求,使每一个url定时到同一个后端服务器.后端服务器为缓存是比较有效.
注意:在upstream中加入hash语句.server语句中不能同时写入weight等其它参数，hash_method是使用的hash算法
例如：    
upstream resinserver{
    server 10.0.0.10:7777;
    server 10.0.0.11:8888;
    hash $request_uri;
    hash_method crc32;
}

 **特别注意:**

​		**nginx在修改完配置文件后，建议使用 nginx.exe -t  命令来检查一下配置文件是否存在语法错误**

## nginx expires 缓存功能

### 效果简介

​	通过在nginx配置文件设置，告诉浏览器返回数据的有效时间，浏览器根据这个时间，确定是否继续向服务器发送数据请求。

### 如何使用

```nginx
#这个设置是设置的对图片文件的处理
location ~\.(jpeg|jpg|png)${
    #缓存时间为一天
    expires 1d;
}
```

## nginx gzip 压缩功能

### 效果简介

​	压缩网络请求中的资源文件，占用带宽小，能够快速的访问到资源。

### 如何进行配置

```ng
gzip on; #开启gzip
gzip_http_version 1.0; #gzip支持的http协议版本
gzip_disable 'MSIE[1-6]'; #不支持微软的ie1-6版本
gzip_types image/jpeg; #对哪个类型的文件进行压缩
```

## nginx 负载均衡实现  

### 效果简介

### 参数说明

1. weight:权重
2. max_fails=*  :分发的最大的失败次数
3. fail_timeout=20s  :失败的超时时间

### session共享问题的解决

存入数据库，使用memcache,mysql,redis

硬盘共享方式，磁盘共享方式

ip_hash hash一致性 让同一个用户访问同一台服务器，具体实现如下所示：

```ngin
upstream nginx{
	#通过ip_hash可以实现同一个用户访问的是同一个web服务器，从而解决了session丢失问题
	ip_hash;
	server 192.168.73.130 weight=1 max_fails=3 fail_timeout=20s;
	server 192.168.73.131 weight=2 max_fails=3 fail_timeout=20s;
}
```

