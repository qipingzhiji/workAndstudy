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

## 获取自增主键的值 

**在使用insert语句时需要获取自增主键的值，使用useGeneratedKeys="true" keyProperty="id"**

```xml
<insert id="insert" flushCache="false" useGeneratedKeys="true" keyProperty="id">
      insert into employee(name,age,birth) VALUES (#{name},#{age},#{birth})
</insert>
```

**对于不支持数据库自增属性的，可以使用<selectKey></selectKey>**

```xml
 <insert id="insert" flushCache="false" >
      <selectKey order="BEFORE" keyProperty="id" >
        select employee_seq.nextval from dual
      </selectKey>
      insert into employee(id, name,age,birth) VALUES (#{id},#{name},#{age},#			  {birth})
  </insert>
```

## 级联(join)查询时自定义关联对象封闭规则

**举例如下**

```xml
<resultMap id="teacherAndDeptResult" type="teacher" autoMapping="true">
        <id column="id" property="id"></id>
        <result column="first_name" property="name"></result>
        <result column="subject" property="subject"></result>
        <result column="b_id" property="dept.id"></result>
        <result column="dept_name" property="dept.deptName"></result>
    </resultMap>

    <resultMap id="teacherAndDeptResult2" type="teacher" autoMapping="true">
        <id column="id" property="id"></id>
        <result column="first_name" property="name"></result>
        <result column="subject" property="subject"></result>
        <association property="dept" javaType="dept">
            <id column="b_id" property="id"></id>
            <result column="dept_name" property="deptName"></result>
        </association>
    </resultMap>
<select id="selectTeacherAndDept" resultMap="teacherAndDeptResult">
      select a.*,b.id b_id,b.dept_name dept_name from teacher a,dept b where        a.deptNum=b.id and b.id= #{id};
</select>
```

**其中在teacherAndDeptResult2配置的时候，使用了association标签，其中的property指的是javabean中的属性，而javaType指的是javabean的全限定名，由于使用了@Alias(dept)，所以这里定义的是dept**

## mybatis association 分步查询

```xml
<resultMap id="teacherAndDeptResultStep" type="teacher">
        <id column="id" property="id"></id>
        <result column="first_name" property="name"></result>
        <result column="subject" property="subject"></result>
        <result column="deptNum" property="deptNum"></result>
        <association  property="dept" select="com.mybatis.demo.mapper.DeptMapper.selectDeptById"
                      column="deptNum">
        </association>
    </resultMap>

    <select id="selectTeacherAndDeptStep" resultMap="teacherAndDeptResultStep">
        SELECT  * from teacher where id = #{id}
    </select>
```

**property:指的是teacher这个bean的dept属性**

**select:指的是调用com.mybatis.demo.mapper.DeptMapper.selectDeptById这个方法**

**column:指的是在SELECT  * from teacher where id = #{id}查询出来的deptNum这个属性值，相当于方法的入参**

## association开启懒加载

### 使用全局配置

```yml
aggressive-lazy-loading: false  #如果开启的话，在加载数据的时候会一并加载暂时用不到的数据
lazy-loading-enabled: true #开启懒加载，只有在使用的时候才会加载数据
```

### 在mapper文件中使用fetchType=lazy属性

```xml
<collection property="teachers" select="com.mybatis.demo.mapper.TeacherMapper.selectTeacherByDeptNum" 
                    column="{deptNum=id}" fetchType="lazy">
```



## 关联查询，使用collection定义集合类型的处理

```xml
<resultMap id="deptAndTeacherMap" type="dept" >
        <id column="id" property="id"></id>
        <result column="dept_name" property="deptName"></result>
        <collection property="teachers" ofType="teacher">
            <id column="t_id" property="id"></id>
            <result column="first_name" property="name"></result>
            <result column="subject" property="subject"></result>
            <result column="deptNum" property="deptNum"></result>
        </collection>
    </resultMap>

    <select id="selectDeptAndTeachers" resultMap="deptAndTeacherMap">
        select d.id,d.dept_name,t.id t_id,t.first_name,t.subject,t.deptNum from dept d
        left join teacher t on d.id = t.deptNum WHERE d.id = #{id,javaType=Integer,jdbcType=TINYINT}
```

## 关联查询，使用collection分步查询

```xml
<resultMap id="deptAndTeacherMapStep" type="dept">
        <id column="id" property="id"></id>
        <result column="dept_name" property="deptName"></result>
        <collection property="teachers" select="com.mybatis.demo.mapper.TeacherMapper.selectTeacherByDeptNum" column="id" >

        </collection>
    </resultMap>

    <select id="selectDeptAndTeachersStep" resultMap="deptAndTeacherMapStep">
        select * from dept where id = #{id}
    </select>
```

**如何传递在collection标签的select属性所指的方法中传递多个参数，可以使用如下的形式**

{key1=value1,key2=value2}的形式

```xml
 <collection property="teachers" select="com.mybatis.demo.mapper.TeacherMapper.selectTeacherByDeptNum" column="{deptNum=id}" >
```

## mybatis 缓存

### 一级缓存失效问题

​	一级缓存默认是开启的，对应一个sqlSession

	1. sqlSession不同
 	2. sqlSession相同，查询条件不同（当前一级缓存中还没有这个数据）
 	3. sqlSession相同，两次查询之间执行了增删改操作
 	4. sqlSession相同，手动清除了一级缓存

### 二级缓存

​	**二级缓存是一个全局缓存，是基于namespace的缓存，一个namespace对应一个二级缓存**

### 如何开启二级缓存

```yaml
mybatis:
  mapper-locations: classpath:mapping/*Mapper.xml
  type-aliases-package: com.mybatis.demo.entity
  configuration:
    auto-mapping-behavior: full
    map-underscore-to-camel-case: true
    aggressive-lazy-loading: false
    lazy-loading-enabled: true #开启懒加载，只有在使用的时候才会加载数据
    cache-enabled: true #开启全局二级缓存配置
```

```xml
<cache type="" blocking="" eviction="" flushInterval="" readOnly="" size="">
        <!--
         eviction:缓存回收策略：
			LRU-最近最少使用，这是默认的回收策略
			FIFO-先进先出
			SOFT-软引用，移除基于垃圾回收器状态软引用规则的数据
			WEAK-弱引用,移除基于垃圾回收器状态和弱引用规则的数据
		flushInterval:缓存刷新时间间隔
		readOnly:是否只读
			-true：只读，直接将缓存中的数据交给用户，不安全
			-false:默认是非只读的，会利用序列化和反序列化技术克隆一份新的数据交给用户
		size：缓存中可以放多少元素
		type=""：指定缓存的全类名，需要实现Cache接口
        -->
    </cache>
```

**只有在sqlSession提交的时候一级缓存中的数据才会放入到二级缓存中去**

## mybatis运行原理解析 

	1. 根据配置文件（全局，sql映射）初始化Configuration对象
 	2. 创建一个DefaultSqlSession对象，里面包含Configuration及Executor（根据配置文件中的defaultExccutorType创建出对应的Executor,默认是simple）
 	3. DefaultSqlSession.getMapper();获取Mapper接口对应的MapperProxy对象
 	4. MapperProxy里面有DefaultSqlSession
 	5. 执行增删改查方法
     1. 调用DefaultSqlSession的增删改查；
     2. 会创建StatementHandler对象，同时创建ParameterHandler和ResultsetHandler对象
     3. 调用StatementHandler预编译参数及设置参数值 
     4. 调用StatementHandler的增删改查方法
     5. 使用ResultHandler对象环境结果

## mybatis 插件的编写

对四大对象进行拦截

1. 实现Interceptor
2. 使用@Intercepts注解完成插件签名
3. 将写好的插件注册到全局配置文件中

## mybatis对分页插件的支持

**使用pageHelper插件**

