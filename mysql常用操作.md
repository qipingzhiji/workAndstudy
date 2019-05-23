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
## 同时更新两张表     
  基本语法：update emp a,dept b set a.sal=a.sal=a.sal*b.deptno,b.deptname=a.ename where a.deptno = b.deptno;  

## 同时删除两张表中的内容  
  基本语法：delete a,b from emp a,dept b where a.deptno=b.deptno and a.deptno = 3;
## mysql优秀的第三方引擎  
  tokudb存储引擎:适用于存储日志数据；历史数据。  
## mysql索引操作  
+ 查看表的索引有哪些  
  show index from table_name  
+ 删除表的索引  
  drop index index_name on table_name  
+ 查看索引的使用情况  
  show status like '%handler_read%';  其中handler_read_key的值越高越好，说明索引使用情况多;handler_read_rnd_next越高说明索引有问题。  
## mysql基本优化  
+ 导入大量数据时可以先设置  
alter table table_name disable keys   
load data   
alter table table_name enable keys   
+ 关闭唯一性校验  
set unique_checks=0;  
load data  
set unique_checks=1;  
+ 关闭自动提交  
set autocommit=0;  
load data  
set autocommit=1;  
+ mysql优化order by  
优化filesort算法  适当增加max_length_for_sort_data 来使用一次扫描算法  
## mysql优化数据库列  
+ procedure analyse()  
例如： select * from table_name procedure annlyse();  
## myisam存储引擎锁问题  
+ 不适用于更新插入频繁场景的原因  
  写锁优先于读锁，即使读进程优先获得锁，写进程也会优先获得锁，这样就容易造成读进程出现延时。  
+ 更改写锁优先级方法  
  1.启动时指定参数  low-priority-updates;  
  2.set low_priority_updates=1;  
  3.通过指定insert,update,delete的低优先级,low_priority;  
  4.设置max_write_lock_count=100，当读进程到达100后，会自动降低写进程获得锁的优先级。  
