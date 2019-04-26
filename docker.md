# docker基本操作  
## docker 挂载本地磁盘  
    1.docker run --name mysql-master -e MYSQL_ROOT_PASSWORD=root --mount type-bind,source=E:/test,destination=/opt/test -d mysql/mysql-server:5.7  
    2.docker run -p --name mysql-master -e MYSQL_ROOT_PASSWORD=root -v E:/test:/opt/test -d mysql/mysql-server:5.7.21  
