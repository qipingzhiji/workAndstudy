# mysql常用基本操作指令   
## 数据库备份操作   
+ 方法1   
mysqldump -hlocalhost -uroot -proot 数据库名 表名>存储位置+文件名   
## 表锁定操作  
+ 锁定表  
  lock tables table_name read或write  
  解释： lock tables table_name read 当前线程执行插入，更新表操作时直接报错,其它线程会进行等待  
        lock tables table_name write 当前线程能够执行表更新操作，其它线程执行会进行等待    
+ 解除表锁定状态  
  unlock tables  
   
+ 查看当前哪些表被锁定  
  show open tables where in_use >0;  
  
## 表复制操作  
+ insert select  
  基本语法: Insert into Table2(field1,field2,…) select value1,value2,… from Table1 where condition;  

