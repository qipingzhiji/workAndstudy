# mybatis框架的使用

## 传参问题

1. 如里是一个参数，那么任意的参数名都可以
2. 如果是多个参数的话
   1. 第 一种方式，可以使用param1、param2的形式来指定传入sql的参数
   2. 第二种方式，也是推荐的方式，使用@Param注解来绑定参数 

3. 如果多个参数正好是业务逻辑处理的模型，可以直接传入pojo类
4. 如果多个参数不是业务模型中的数据，也没有对应的pojo，可以直接传入map,其中#{key}代表map中的key值
5. 如果多个参数不是业务模型中的数据，但是要经常使用，推荐编写一个TO类作为数据传输对象

## 取值#{ }和${}的区别

**区别：**

	1. #{}是以预编译的方式将参数设置到sql语句中去，防止sql注入
 	2. ${}是取出的值直接拼接在sql语句中，会有安全问题

## 查询时需要返回集合类型时

1. 返回list时，需要在<select id="###" resultType="设定集合类中的包装类"></select>

   ```xml
   <select id="allEmployees" resultType="employee">
           SELECT  * from employee
   </select>
   ```

2. 返回map集合时，需要在<select id="###" resultType="设定集合类中的包装类"></select>，同时要在接口方法名上设定map的key值取值

   ```java
    	@MapKey(value = "id")//这是设定map的key值为查询到的employee对象的id属性值
       Map<Integer, Employee> allEmpMap();
   ```

   ```xml
   <select id="allEmpMap" resultType="employee">
           SELECT * from employee
   </select>
   ```

