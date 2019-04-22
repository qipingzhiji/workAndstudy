# mysql开启双机热备  
## 主服务器  
+ 创建用户  
    ```  
        create user 'replicate'@'%' identified by 'replicate';
        grant replication slave on *.* to 'replicate'@'%';
    ```  
+ mysql开启binlog    
     修改mysql的配置文件，添加如下：  
     server-id = 服务器地址后一位（唯一标识，可不进行修改）
     log-bin=mysql-bin(开启logbin)
     binlog-do-db=test(要备份的数据库)
     binlog-ignore-db= mysql(忽略的数据库）
+ 重启mysql服务  
  windows下可直接dos命令操作如下：
  ```
      net stop mysql(服务名）
      net start mysql(服务名）
  ```

+ 查看服务器状态  
  ```
    show master status;
  ```
  
## 从服务器  

+  从服务器开启binlog  
         修改mysql的配置文件，添加如下：  
         server-id = 服务器地址后一位（唯一标识，可不进行修改）  
         log-bin=mysql-bin(开启logbin)    
         binlog-do-db=test(要备份的数据库)    
         binlog-ignore-db= mysql(忽略的数据库）   
         
+ 重启mysql服务  

+ 开启slave线程
  1.stop slave  
  2.reset slave  
  3.root用户执行如下命令  
    ```
     change master to
       master_host='主服务器ip',
       master_user='主服务器创建的用于主从复制的用户',
       master_password='密码',
       master_log_file='主服务器的mysql-bin-log文件名称,如要在主服务器下执行show master status来获取',
       master_log_pos=如上，注意没有引号,结束加逗号
## 查看从服务器状态  
    ```
    show  slave status\G;
    ```
    
    
##  其它的一些配置  
+ **主服务器**  
    binlog-do-db = 要备份的数据库,如果有多个数据库,另起一行,每个备份的数据库都要起一行.
    binlog-ignore-db =忽略备份的数据库
+ **从服务器**  
    replicate-do-db=要备份的数据库，如果有多个，则起多行
    replicate-ignore-db=忽略备份的数据库
    

