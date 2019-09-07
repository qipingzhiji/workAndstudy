# Jdk 生成证书过程

## 生成密钥

执行命令：**keytool -genkey -alias zhanght -keyalg RSA -keystore D:\cert\test**

## 生成证书

执行命令：**keytool -export -trustcacerts -alias zhanght -file D:\cert\test.cer -keystore D:\cert\test**

