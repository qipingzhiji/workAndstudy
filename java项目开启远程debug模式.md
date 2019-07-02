# java项目部署到linux服务器开启远程debug的几种方式  
+ tomcat服务器下的远程debug设置
+ springboo项目以jar包的形式远程debug的配置 
+ 开发工具上进行配置  
## tomcat服务器下的远程debug设置  
进入tomcat安装目录，找到bin目录下的catalina.sh文件，增加如下配置：
```xml
declare -x CATALINA_OPTS="-server -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8001"
```
远程debug地址为8001,保存，启动tomcat，即可。
## springboo项目以jar包的形式远程debug的配置 
springboot内嵌了tomcat，项目默认打成jar包，只需要在启动的时候加上如下参数，设置远程debug端口为8001。执行如下启命令：
``` java
java -Dspring.profiles.active=uc -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=8001 -jar Alogin-1.0.0.jar
```
## 开发工具上进行配置 
1. 打开idea中的run/debug configurations, 选择remote类型，地址配置为服务器地址，端口配置为上述配置参数中的address;
2. 打开Eclipse，点击左上角的Debug Configutations按钮，找到Remote Java Application项，双击。

**注意**
1.首先本地开发工具上要有和部署在远程服务器的项目代码保持一致，否则debug的时候会出现代码行错位，难以达到debug的效果。
2.服务器上远程debug的端口要对外开放，如果是阿里云服务器的话，需要配置安全组策略，自己的服务器则需要防火墙开启相应的端口

