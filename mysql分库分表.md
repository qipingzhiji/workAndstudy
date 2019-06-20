# mysql数据库分库分表操作  
[参考至微信公众号java基基](https://mp.weixin.qq.com/s/wFEdLuri0xAUCedIY30YRQ)  

[mysql replace into 和 insert into 区别](https://www.cnblogs.com/honeybaobao/p/5692332.html)  

## mysql replace into 和 insert into 的区别  
```
      MySQL对replace into的操作是首先是insert操作，如果insert失败，则对insert失败的这条记录进行update，如果update还是失败，则会进行delete操作之后再
  update。
```  
