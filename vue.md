## vue的一些常用配置  
+ 使用axios配置代理  
  在项目根目录下创建vue.config.js  
  在vue.cofnig.js中创建如下  
  ```
    module.exports = {
    devServer: {
        port: 端口号,
        proxy: {
            '/apis': {
                target: 'https://movie.douban.com/',  // target host
                ws: true,  // proxy websockets 
                changeOrigin: true,  // needed for virtual hosted sites
                pathRewrite: {
                    '^/apis': ''  // rewrite path
                }
            },
        }
    }
};
```   
   发送请求如下：  
   ```
   this.axios.get('/apis/ithil_j/activity/movie_annual2017').then(res => {  
	console.log(res.data)  
  }, res => {  
    console.info('调用失败');  
  })  
  ```
文档参考[csdn]("https://blog.csdn.net/m0_37285193/article/details/83176597")  
