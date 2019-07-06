# CentOs安装注意事项

# 更换yum镜像源

​	Centos自带的国外的yum源下载速度很慢，为此，需要将yum源更换为国内的阿里的数据源。

### 备份yum数据源原文件

​	执行如下命令:

```linux
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
```

### 替换yum数据源文件

​	可以从下面的网址下载阿里的数据源文件

​    http://mirrors.aliyun.com/repo/Centos-7.repo

### 生成缓存

   在替换完成yum 镜像文件后运行如下命令生成缓存

```linux
yum makecache
```

### 安装gcc gcc-c++编译软件

执行如下命令安装gcc和gcc-c++插件

```linux
yum -y install gcc  
yum -y install gcc-c++
```



## 常见问题解决

### make失败的时候，需要再次make编译软件的时候，先执行下make distclean清空make失败的文件。

### 编写shell脚本时，提示“没有此文件或目录”

​	此原因为编写shell脚本在编写时的系统跟你的执行脚本的系统不一致，比如你在window系统编写的脚本，当拷贝到Linux系统时就会遇见此问题。

**解决办法**

​	执行如下命令:

```linux
#执行set ff查看shell脚本的文件格式
set ff
如果文件格式为fileformat=doc，则继续执行以下命令
##修改文件format为unix
set ff=unix
##或者是也可以执行如下：
set fileformat=unix
```

