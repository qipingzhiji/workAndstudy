### 查看防火墙状态  
  systemctl status firewalld.service
### 启用禁用防火墙  
  + 启用  
  systemctl start firewalld.service
  + 禁用  
  systemctl stop firewalld.service
### 通过防火墙开启特定端口  
  1. 增加端口  
     firewall-cmd --zone=public --add-port=80/tcp --permanent  
     firewall-cmd --zone=public --add-port=1000-2000/tcp --permanent
  2.  重新载入  
     firewall-cmd  --reload
### 查看防火墙状态  
  firewall-cmd --zone=public --query-port=80/tcp  
### 删除  
  firewall-cmd --zone-public --remove-port=80/tcp --permanet 
  
[参考博客](https://blog.csdn.net/weiyangdong/article/details/79540217,"firewall") 
 
