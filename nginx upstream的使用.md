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


 
