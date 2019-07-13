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



## springboot  日志框架配置

```pro
#表示在当前磁盘的根目录下创建spring文件夹和里面的log文件夹，使用spring.log作为默认的日志文件
logging.path=/spring/log
#在控制台输出的格式
logging.pattern.console=%d{yyyy-MM-dd} [%thread] %-5level %logger{50} - %msg%n
#指定文件中日志输出的格式
logging.pattern.file=%d{yyyy-MM-dd} [%thread] %-5level - %logger{50} %msg%n
```

**如何自定义自己的日志配置： 只需要在类路径下放上日志框架的配置文件即可；springboot应用就不会在使用默认的配置了**

| Logging system         | Customization                                                |
| ---------------------- | ------------------------------------------------------------ |
| Logback                | l**ogback-spring.xml**,**logback-spring.groovy**, or **logback.groovy** |
| Log4j2                 | **log4j2-spring.xml** or **log4j2.xml**                      |
| JDK(Java Util Logging) | logging.properties                                           |

**logback.xml:** 直接就被日志框架识别了

**logback-spring.xml:** 日志框架就不直接加载日志的配置项，由springboot

```xml
<springProfile name="staging">
    <!-- configuration to be enabled when the "staging" profile is active -->
</springProfile>
```

## spring boot 网站应用开发

### 静态资源处理

**在WebMvcAutoConfiguration类中有如下方法是对静态资源处理的**

```java
@Override
		public void addResourceHandlers(ResourceHandlerRegistry registry) {
			if (!this.resourceProperties.isAddMappings()) {
				logger.debug("Default resource handling disabled");
				return;
			}
			Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
			CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
			if (!registry.hasMappingForPattern("/webjars/**")) {
				customizeResourceHandlerRegistration(registry.addResourceHandler("/webjars/**")
						.addResourceLocations("classpath:/META-INF/resources/webjars/")
						.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
			}
			String staticPathPattern = this.mvcProperties.getStaticPathPattern();
			if (!registry.hasMappingForPattern(staticPathPattern)) {
				customizeResourceHandlerRegistration(registry.addResourceHandler(staticPathPattern)
						.addResourceLocations(getResourceLocations(this.resourceProperties.getStaticLocations()))
						.setCachePeriod(getSeconds(cachePeriod)).setCacheControl(cacheControl));
			}
		}
```

```java
private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
			"classpath:/resources/", "classpath:/static/", "classpath:/public/" };
```

对静态资源的处理有如下两种方式：

1. 对引入webjars静态资源的文件进行处理
2. 对以下路径的资源进行处理
   - classpath:/META-INF/resources/
   - classpath:/resources/
   - classpath:/static/
   - classpath:/public/
   - /:表示当前项目的根路径

3. 欢迎页：在静态资源文件下所有inex.html页面 

4. 所有的**/favicon.ico在上述的静态资源目录下面进行查找

5. 自己进行指定静态资源目录

   ​	配置spring.resources.static-locations属性