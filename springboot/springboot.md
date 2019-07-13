# springboot应用  

## 注解

@Value和@ConfigurationProperties获取值的比较

|                | @ConfigurationProperties  | @Value     |
| -------------- | ------------------------- | ---------- |
| 功能           | 批量注入配置文件中 的属性 | 一个个指定 |
| 松散绑定       | 支持                      | 不支持     |
| SpEL           | 不支持                    | 支持       |
| JSR303数据校验 | 支持                      | 不支持     |

