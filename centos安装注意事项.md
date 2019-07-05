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