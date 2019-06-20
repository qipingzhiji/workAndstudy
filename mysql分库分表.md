# mysql数据库分库分表操作  
[参考至微信公众号java基基](https://mp.weixin.qq.com/s/wFEdLuri0xAUCedIY30YRQ)  

[mysql replace into 和 insert into 区别](https://www.cnblogs.com/honeybaobao/p/5692332.html)  

## mysql replace into 和 insert into 的区别  
```
      MySQL对replace into的操作是首先是insert操作，如果insert失败，则对insert失败的这条记录进行update，如果update还是失败，则会进行delete操作之后再
  update。
```  

## mysql维护主键唯一   
**实例**  
1.建立sequence表  
  ``` mysql
      create table sequenee(
         id bigint(20) unsigned not null auto_increment,
         stub char(1) not null default '',
         primary key(id),
         unique key stub(stub)
         )engine=myisam default charset=utf8;
  ```
  
 2.当需要全局唯一id时，执行如下  
   ``` mysql
      replace into  sequence(stub) values('a');
      select last_insert_id();
   
   ```  
3. 为什么要使用myisam?  
使用 MyISAM 存储引擎而不是 InnoDB，以获取更高的性能。MyISAM使用的是表级别的锁，对表的读写是串行的，所以不用担心在并发时两次读取同一个ID值。  
