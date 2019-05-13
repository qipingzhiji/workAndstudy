# nodeJs常用配置  
+ 配置国内镜像  
  执行命令: npm config set registry " https://registry.npm.taobao.org "  
+ node包管理工具nvm配置  
  在nvm安装目录下的settings文件下增加如下两行配置：
  node_mirror: https://npm.taobao.org/mirrors/node/  
  npm_mirror: https://npm.taobao.org/mirrors/npm/  

+ 配置vue-ci  
1. 配置淘宝镜像   
  npm install -g cnpm --registry=http://registry.npm.taobao.org  
2.安装    
  cnpm install -g vue-cli  
3.查看是否安装成功  
  vue -V
