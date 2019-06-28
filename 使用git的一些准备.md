# 使用git的一些准备  
## 前期准备  
+ 初始化设置  
1. 设置姓名和邮箱  
```git
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"  
git config --global color.ui auto  
```
2. 设置sshkey  
```git
ssh-keygen -t rsa -C "你配置的邮箱"  
```
3. github中添加公钥
```git  
cat ~/.ssh/id_rsa_pub
```
4.认证  
```git 
ssh -T git@github.com
```
