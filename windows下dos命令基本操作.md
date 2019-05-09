# windows下netstat和netsh的基本用法    
## netstat的基本用法    
+ 查找特定端口的进程pid    
    基本用法：netstat -ano|findstr 要查找的端口号    
+ 结束任务    
    基本用法：taskkill /pid 进程的pid号 /f    
## netsh的用法
+ 配置静态网址    
    基本用法:netsh interface ip set address name="连接名" source=static addr=要配置的ip地址 mask=255.255.255.0 gateway=配置的网关    
+ 配置dns    
    基本用法：netsh interface ip set dns name="要设置的连接名"  source=static  addr=配置的dns地址  
+ 启用有线网卡  
    netsh interface set interface eth0 enabled  
+ 禁用有线网卡  
    netsh interface set interface eth0 disabled
+ 启用无线网卡  
    netsh interface set interface wlan0 disabled
+ 启用有线网卡  
    netsh interface set interface wlan0 enabled
## 打开计算机管理  
+ win+r compmgmt.msc
