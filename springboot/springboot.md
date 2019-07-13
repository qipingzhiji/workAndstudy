# springboot应用  

## 注解

@Value和@ConfigurationProperties获取值的比较

|                | @ConfigurationProperties  | @Value     |
| :------------: | ------------------------- | ---------- |
|      功能      | 批量注入配置文件中 的属性 | 一个个指定 |
|    松散绑定    | 支持                      | 不支持     |
|      SpEL      | 不支持                    | 支持       |
| JSR303数据校验 | 支持                      | 不支持     |
| 复杂类型的支持 | 支持                      | 不支持     |

**使用场景：**

​	@Value: 在某个业务逻辑中需要获取一下配置文件中的属性，使用@Value

​	@ConfigurationProperties：专门编写了一个javabean来和配置文件进行映射时使用

如果要使用@Value获取配置文件属性中的值的话，可以参考如下：

配置文件中的值：

```yaml
list: topic1,topic2,topic3
maps: "{k1: 'v1', k2: 'v2'}"
#在进行map值设定的时候，一定要将map中key值对应的value值包起来
```

java类属性上的注解：

```java
@Value("#{'${list}'.split(',')}")
private List<String> list;
@Value("#{${maps}}")
private Map<String,String> maps;
```

## 配置文件加载位置

​	springboot 启动会扫描以下位置的application.properties或者application.yml文件作为spring boot的默认配置文件

	- file：./config/
	- -file: ./
	- -classpath:/config
	- -classpath:/

​     以上是按照优先级从高到低的顺序加载的，所有位置的文件都会被加载，高优先级的配置内容会覆盖低优先级配置的内容

​	**也可以通过spring.config.location来改变配置文件加载的位置** 

​	**项目打包好以后，可以通过使用命令行的方式，启动 项目的时候指定配置文件的新位置；指定配置文件和默认加载的这些配置文件共同起作用形成互补**

## springboot 配置文件

[springboot 配置文件属性一览](https://docs.spring.io/spring-boot/docs/1.5.9.RELEASE/reference/htmlsingle/#common-application-properties)









