# centos添加环境变量  

## 方法1  

1. 运行shell命令  
  直接在当前**会话窗口**中运行:  
  ```
    export JAVA_HOME=/usr/java/jdk1.6.0_14  
    export PATH=$JAVA_HOME/bin:$PATH  
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
  ```
  **只会对当前会话生效**  
 ## 方法2  
 1. 修改**/etc/.bash_profile**文件  
  ```
    export JAVA_HOME=/usr/java/jdk1.6.0_14  
    export PATH=$JAVA_HOME/bin:$PATH  
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
  ```  
  **只会对特定用户生效**  
  
  ## 方法3  
  1. 修改**/etc/profile**文件  
  ```
    export JAVA_HOME=/usr/java/jdk1.6.0_14  
    export PATH=$JAVA_HOME/bin:$PATH  
    export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar  
  ```  
  **对所有用户的shell生效**  
