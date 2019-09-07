# JDK生成证书过程

## 生成密钥

执行命令：

```
keytool -genkey -alias zhanght -keyalg RSA -keystore D:\cert\test
```



## 生成证书

执行命令：

```dos
keytool -export -trustcacerts -alias zhanght -file D:\cert\test.cer -keystore D:\cert\test
```



## 将证书导入到JDK中

执行命令

```dos
keytool -import -trustcacerts -alias zhanght -file D:\cert\test.cer -keystore "C:\Program Files\Java\jdk1.8.0_181\jre\lib\security\cacerts"
```

**要求输入JDK密钥库的口令**

密钥口令为：changeit

