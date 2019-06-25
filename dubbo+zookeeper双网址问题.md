# dubbo+zookeeper双网关问题不一致问题解决方案  
**参考网址csdn**
[解决dubbo注册zookepper服务IP乱入问题的三种方式](https://blog.csdn.net/fullbug/article/details/52739580)  
+ 解决方案  
1.方案  去掉服务器上的dnf配置  
```
找到服务器上的/etc/resolv.conf 将DNS配置去掉或配置成8.8.8.8或配成192.168.23.180这样这台服务器的DNS不可用。
参考配置如下
#nameserver 222.246.129.80
#nameserver 59.51.78.210
#nameserver 8.8.8.8
nameserver 192.168.23.180
#search localdomain
```
2.方案   在工程duboo注册服务配置文件里指定IP  
```
把管理控制台中dubbo/webapps/ROOT/WEB-INF/dubbo.properties文件中加入dubbo.protocol.host=192.168.23.180，然后在Dubbo服务的dubbo配置文件<dubbo:protocol name="dubbo" port="20883"  />中加入 host="192.168.23.180"，在Dubbo消费者端加入<dubbo:protocol host="192.168.0.123" />的配置。然后重启Dubbo管理员控制台、停止消费者端，停止服务提供端，启动服务提供端，再启动消费者端。
参考配置如下：
服务提供者provider.xml
<!-- 用dubbo协议在20880端口暴露服务 -->
<dubbo:protocol name="dubbo" host="192.168.23.180" port="20883" />
消费者consumer.xml
<dubbo:protocol host="192.168.23.180" />
配置完了后在dubbo-admin控制台可以看到服务提供者注册到zookepper上的dobbo服务已经是正常的192.168.23.180。消费者显示的还是consumer://124.232.132.94/***** 但不影响调用。
```  
3.方案  在服务器上/etc/hosts，上配置主机名和注册服务的IP。如：192.168.23.180 host2  
```
没有配置之前ping 主机名host2 返回的是124.232.132.94
ping host2
PING host2 (124.232.132.94) 56(84) bytes of data.
64 bytes from 124.232.132.94: icmp_seq=1 ttl=55 time=5.67 ms
在/etc/hosts里配置IP和主机名192.168.23.180 host2 后ping主机名host2返回 192.168.23.180
ping host2
PING host2 (192.168.23.180) 56(84) bytes of data.
64 bytes from host2 (192.168.23.180): icmp_seq=1 ttl=64 time=0.024 ms
``` 
