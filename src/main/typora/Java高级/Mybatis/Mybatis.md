# Mybatis框架



## Mybatis常用Maven包

```xml
<!--测试注解包-->  
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>compile</scope>
</dependency>
 <!-- lombok注解包 -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.4</version>
    <scope>provided</scope>
</dependency>
 <!-- 数据库连接jar包 -->
 <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.15</version>
</dependency>
<!-- mybatis核心jar包  -->
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.4.6</version>
</dependency>
 <!-- log4j日志包 -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
<!-- log4j源码配合log4j使用 -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.11.1</version>
</dependency>
<!-- log4j的接口-->
<dependency>
      <groupId>org.apache.logging.log4j</groupId>
      <artifactId>log4j-api</artifactId>
      <version>2.11.1</version>
</dependency>
 <!-- slf4j用于用户设置日志打印内容 -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-log4j12</artifactId>
    <version>1.7.25</version>
    <scope>test</scope>
</dependency>
<!--slf4j的接口--->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
```



##  逆向工程☆☆☆☆☆

​          这个逆向工程主要通过连接数据库生成对应的实体类,映射文件,mapper接口,可以生成表中的所有的单标操作,但是无法实现多变的查询,所以涉及到多表的操作需要自己进行手动添加映射关系



使用的java包与maven

```xml
<!-- 逆向工程的核心jar包 -->
    <dependency>
        <groupId>org.mybatis.generator</groupId>
        <artifactId>mybatis-generator-core</artifactId>
        <version>1.3.7</version>
    </dependency>
    <!--通过jdbc连接数据库的jar包 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.15</version>
    </dependency>
```

逆向工程配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
	<context id="testTables" targetRuntime="MyBatis3">
		<commentGenerator>
			<!-- 是否去除自动生成的注释 true：是 ： false:否 -->
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		<!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
		<jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
			connectionURL="jdbc:mysql://localhost:3306/dept?serverTimezone=UTC"
			userId="root"
			password="xiaohui">
		</jdbcConnection>
		<!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
			connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg" 
			userId="yycg"
			password="yycg">
		</jdbcConnection> -->

		<!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和 
			NUMERIC 类型解析为java.math.BigDecimal -->
		<javaTypeResolver>
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- targetProject:生成实体类的位置 -->
		<javaModelGenerator targetPackage="com.jia.ssm.day3.bean"
			targetProject=".\src\main\java">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
			<!-- 从数据库返回的值被清理前后的空格 -->
			<property name="trimStrings" value="true" />
		</javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
		<sqlMapGenerator targetPackage="mybtis.mapper.day3"
			targetProject=".\src\main\resource">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</sqlMapGenerator>
		<!-- targetPackage：mapper接口生成的位置 -->
		<javaClientGenerator type="XMLMAPPER"
			targetPackage="com.jia.ssm.day3.dao"
			targetProject=".\src\main\java">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</javaClientGenerator>
		<!-- 指定数据库表 -->
		
        <table tableName="employee"></table>
        <table tableName="dept"></table>
		<table tableName="level"></table>
		<table tableName="position"></table>
		
	</context>
</generatorConfiguration>
```

逆向工程执行的main方法

```java
package com.jia.ssm.day3.utils;

import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class GeneratorSqlmap {

	public void generator() throws Exception{

		List<String> warnings = new ArrayList<String>();
		boolean overwrite = true;
		File configFile = new File("E:\\ideaproject\\ssm\\src\\main\\java\\com\\jia\\ssm\\day3\\utils\\generatorConfig.xml");
		ConfigurationParser cp = new ConfigurationParser(warnings);
		Configuration config = cp.parseConfiguration(configFile);
		DefaultShellCallback callback = new DefaultShellCallback(overwrite);
		MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
				callback, warnings);
		myBatisGenerator.generate(null);

	} 
	public static void main(String[] args) throws Exception {
		try {
			GeneratorSqlmap generatorSqlmap = new GeneratorSqlmap();
			generatorSqlmap.generator();
		} catch (Exception e) {
			e.printStackTrace();
		}
		
	}

}
```



## Mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    
    <!-- 引入外部的properties文件 -->
    <properties resource="db.properties">
       <!-- <property name="db_driver" value="com.mysql.jdbc.Driver"/>
       <property name="db_url" value="jdbc:mysql://localhost:3306/mybatistest"/>
       <property name="db_uname" value="root"/>
       <property name="db_pwd" value="root"/> -->
    </properties>
    
    <settings>
       <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
    
    <typeAliases>
       <!-- 单个起别名，type：自定义类型，alias：别名 -->
      <!-- <typeAlias type="com.offcn.bean.User" alias="u"/> -->
      <!-- 为包里的类批量起别名，默认别名是类名或者把类名的首字母小写 -->
      <package name="com.offcn.bean"/>
    </typeAliases>
    
    <!-- 数据源 -->
	<environments default="development">
		<environment id="development">
			<transactionManager type="JDBC" />
			<dataSource type="POOLED">
				<property name="driver" value="${db_driver}" />
				<property name="url" value="${db_url}" />
				<property name="username" value="${db_uname}" />
				<property name="password" value="${db_pwd}" />
			</dataSource>
		</environment>
	</environments>
	
	<!-- 映射文件 -->
	<mappers>
		<mapper resource="com/offcn/bean/UserMapper.xml" />
	</mappers>
</configuration>
```

### settings属性与值

|             设置参数             |      | 描述                                                         | 有效值                                                       | 默认值                                                |
| :------------------------------: | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------------------------------------------------- |
|           cacheEnabled           |      | 全局地开启或关闭配置文件中的所有映射器已经配置的任何缓存。   | true \| false                                                | true                                                  |
|        lazyLoadingEnabled        |      | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态。 | true \| false                                                | false                                                 |
|      aggressiveLazyLoading       |      | 当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载（参考lazyLoadTriggerMethods). | true \| false                                                | false (true in ≤3.4.1)                                |
|    multipleResultSetsEnabled     |      | 是否允许单一语句返回多结果集（需要兼容驱动）。               | true \| false                                                | true                                                  |
|          useColumnLabel          |      | 使用列标签代替列名。不同的驱动在这方面会有不同的表现， 具体可参考相关驱动文档或通过测试这两种不同的模式来观察所用驱动的结果。 | true \| false                                                | true                                                  |
|         useGeneratedKeys         |      | 允许 JDBC 支持自动生成主键，需要驱动兼容。 如果设置为 true 则这个设置强制使用自动生成主键，尽管一些驱动不能兼容但仍可正常工作（比如 Derby）。 | true \| false                                                | False                                                 |
|       autoMappingBehavior        |      | 指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示取消自动映射；PARTIAL 只会自动映射没有定义嵌套结果集映射的结果集。 FULL 会自动映射任意复杂的结果集（无论是否嵌套）。 | NONE, PARTIAL, FULL                                          | PARTIAL                                               |
| autoMappingUnknownColumnBehavior |      | 指定发现自动映射目标未知列（或者未知属性类型）的行为。<br/>NONE: 不做任何反应<br/>WARNING: 输出提醒日志 ('org.apache.ibatis.session.AutoMappingUnknownColumnBehavior'的日志等级必须设置为 WARN)<br/>FAILING: 映射失败 (抛出 SqlSessionException) | NONE, WARNING, FAILING                                       | NONE                                                  |
|       defaultExecutorType        |      | 配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（prepared statements）； BATCH 执行器将重用语句并执行批量更新。 | SIMPLE REUSE BATCH                                           | SIMPLE                                                |
|     defaultStatementTimeout      |      | 设置超时时间，它决定驱动等待数据库响应的秒数。               | 任意正整数                                                   | Not Set (null)                                        |
|         defaultFetchSize         |      | 为驱动的结果集获取数量（fetchSize）设置一个提示值。此参数只可以在查询设置中被覆盖。 | 任意正整数                                                   | Not Set (null)                                        |
|       safeRowBoundsEnabled       |      | 允许在嵌套语句中使用分页（RowBounds）。如果允许使用则设置为false。 | true \| false                                                | False                                                 |
|     safeResultHandlerEnabled     |      | 允许在嵌套语句中使用分页（ResultHandler）。如果允许使用则设置为false。 | true \| false                                                | True                                                  |
|     mapUnderscoreToCamelCase     |      | 是否开启自动驼峰命名规则（camel case）映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。 | true \| false                                                | False                                                 |
|         localCacheScope          |      | MyBatis 利用本地缓存机制（Local Cache）防止循环引用（circular references）和加速重复嵌套查询。 默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlSession 的不同调用将不会共享数据。 | SESSION \| STATEMENT                                         | SESSION                                               |
|         jdbcTypeForNull          |      | 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。 某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。 | JdbcType 常量. 大多都为: NULL, VARCHAR and OTHER             | OTHER                                                 |
|      lazyLoadTriggerMethods      |      | 指定哪个对象的方法触发一次延迟加载。                         | 用逗号分隔的方法列表。                                       | equals,clone,hashCode,toString                        |
|     defaultScriptingLanguage     |      | 指定动态 SQL 生成的默认语言。                                | 一个类型别名或完全限定类名。                                 | org.apache.ibatis.scripting.xmltags.XMLLanguageDriver |
|      defaultEnumTypeHandler      |      | 指定 Enum 使用的默认 TypeHandler 。 (从3.4.5开始)            | 一个类型别名或完全限定类名。                                 | org.apache.ibatis.type.EnumTypeHandler                |
|        callSettersOnNulls        |      | 指定当结果集中值为 null 的时候是否调用映射对象的 setter（map 对象时为 put）方法，这对于有 Map.keySet() 依赖或 null 值初始化的时候是有用的。注意基本类型（int、boolean等）是不能设置成 null 的。 | true \| false                                                | false                                                 |
|    returnInstanceForEmptyRow     |      | 当返回行的所有列都是空时，MyBatis默认返回null。 当开启这个设置时，MyBatis会返回一个空实例。 请注意，它也适用于嵌套的结果集 (i.e. collectioin and association)。（从3.4.2开始） | true \| false                                                | false                                                 |
|            logPrefix             |      | 指定 MyBatis 增加到日志名称的前缀。                          | 任何字符串                                                   | Not set                                               |
|             logImpl              |      | 指定 MyBatis 所用日志的具体实现，未指定时将自动查找。        | SLF4J \| LOG4J \| LOG4J2 \| JDK_LOGGING \| COMMONS_LOGGING \| STDOUT_LOGGING \| NO_LOGGING | Not set                                               |
|           proxyFactory           |      | 指定 Mybatis 创建具有延迟加载能力的对象所用到的代理工具。    | CGLIB \| JAVASSIST                                           | JAVASSIST (MyBatis 3.3 or above)                      |
|             vfsImpl              |      | 指定VFS的实现                                                | 自定义VFS的实现的类全限定名，以逗号分隔。                    | Not set                                               |
|        useActualParamName        |      | 允许使用方法签名中的名称作为语句参数名称。 为了使用该特性，你的工程必须采用Java 8编译，并且加上-parameters选项。（从3.4.1开始） | true \| false                                                | true                                                  |
|       configurationFactory       |      | 指定一个提供Configuration实例的类。 这个被返回的Configuration实例用来加载被反序列化对象的懒加载属性值。 这个类必须包含一个签名方法static Configuration getConfiguration(). (从 3.2.3 版本开始) | 类型别名或者全类名.                                          | Not set                                               |



## Mappe配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.offcn.dao.UserDao">
     
     <select id="getUserById"  parameterType="int" resultType="user">
        select * from user where id=#{uid}  
     </select>
     
     <!--输入参数是自定义类型，#{自定义类型的属性名}  -->
     <insert id="saveUser" parameterType="com.offcn.bean.User">
        insert into user values(null,#{username},#{birthday},#{sex},#{address})
        <!--keyColumn:主键列名，keyProperty：主键属性名，order：主键在插入之前或之后生成，resultType：主键的类型  -->
        <selectKey keyColumn="id" keyProperty="id" order="AFTER" resultType="int">
           select last_insert_id()
        </selectKey>
     </insert>
     
     <update id="updUser" parameterType="com.offcn.bean.User">
        update user set username=#{username},sex=#{sex} where id=#{id}
     </update>
     
     <delete id="delUser" parameterType="int">
        delete from user where id=#{uid}
     </delete>
     
     <!-- 查询姓名包含某个字以及性别为男或者女的所有用户 -->
     <!-- ${}和#{}：${}是拼接的方式，#{}是占位符的方式 -->
     <select id="getByNameAndSex" parameterType="User" resultType="user">
        select * from user where username like '%${username}%' and sex=#{sex}
     </select>
     
     <select id="getByNameAndSex1" parameterType="com.offcn.bean.User" resultType="user">
        select * from user where username like "%"#{username}"%" and sex=#{sex}
     </select>
     
     <select id="getByNameAndSex2" parameterType="map" resultType="com.offcn.bean.User">
        select * from user where username like "%"#{uname}"%" and sex=#{sex}
     </select>
     
     <select id="getByNameAndSex3" resultType="com.offcn.bean.User">
        select * from user where username like "%"#{un}"%" and sex=#{usex}
     </select>
     
     <select id="getByNameAndSex4" resultType="com.offcn.bean.User">
        select * from user where username like "%"#{0}"%" and sex=#{1}
     </select>
     
</mapper>
```



## log4j日志配置文件

```yaml
# 全球日志配置
log4j.rootLogger=DEBUG, stdout
# MyBatis 日志配置
log4j.logger.org.mybatis.example.BlogMapper=TRACE
# 控制台打印
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```





## Mapper配置文件



### 动态sql



#### resultMap☆☆☆☆☆

完整成功时手动映射,一般结果集的列名和实体的属性名不一致时使用.映射成功时使用.

```xml
<!--type:返回结果的类型，id：任意定义在当前命名空间唯一  -->
    <resultMap type="user" id="rs">
       <!--完成主键映射，column：列名 ，property：属性名 -->
       <id column="uid" property="id"/>
       <!--完成非主键映射 -->
       <result column="uname" property="username"/>
    </resultMap>
    <select id="getUserById" parameterType="int" resultMap="rs">
       select id uid,username uname from user where id=#{uid}
    </select>
```



#### sql片段☆☆☆☆☆

提供sql语句的公共部分,同意维护.

代码:

```xml
<sql id="任意定义，要在当前命名空间唯一的">
   select * from user
</sql>
```

引用:

```xml
<include refid="sql片段的id"></include>
```



#### forEach☆☆☆☆☆

遍历数组和集合

代码:

```xml
<!-- foreach -->
    <delete id="delMulti" parameterType="java.util.List">
       delete from user where id in
         <!--collection:输入参数为List集合时，必须写list,
             item:为集合里的每一项起名，可以任意定义
             separator：每一项中间的分割符
<!-- foreach -->
    <delete id="delMulti" parameterType="java.util.List">
       delete from user where id in
         <!--collection:输入参数为List集合时，必须写list,
             item:为集合里的每一项起名，可以任意定义
             separator：每一项中间的分割符
             open:在执行循环体之前拼接的内容；
             close：在执行循环体之后拼接的内容；-->
         <foreach collection="list" item="uid" separator="," open="(" close=")">
            #{uid}
         </foreach>
    </delete>
    
    <delete id="delMultiByArray" parameterType="java.lang.reflect.Array">
       delete from user where id in
         <!--collection:输入参数为List集合时，必须写array,
             item:为集合里的每一项起名，可以任意定义
             separator：每一项中间的分割符
             open:在执行循环体之前拼接的内容；
             close：在执行循环体之后拼接的内容；-->
         <foreach collection="array" item="uid" separator="," open="(" close=")">
            #{uid}
         </foreach>
    </delete>
    
    <delete id="delMultiByObj" parameterType="UserVo">
       delete from user where id in
         <!--collection:输入参数为自定义类型时，必须写实体类的属性名,
             item:为集合里的每一项起名，可以任意定义
             separator：每一项中间的分割符
             open:在执行循环体之前拼接的内容；
             close：在执行循环体之后拼接的内容；-->
         <foreach collection="ids" item="uid" separator="," open="(" close=")">
            #{uid}
         </foreach>
    </delete>
    <!-- 批量插入-->
    <insert id="saveMulti" parameterType="java.util.List">
       insert into user(username,sex) values
       <foreach collection="list" item="u" separator=",">
          (#{u.username},#{u.sex})
       </foreach>
    </insert>
```



#### where if☆☆☆☆☆

一般用条件检索

只用需要where条件的sql语句中可以省略忽略开头的and

```xml
<!--where:解析成where关键字，会自动去掉第一个符合条件的限定条件中的and -->
    <select id="getByNameSex" parameterType="map" resultType="user">
       select * from user 
       <where>
          <if test="uname!=null and uname!=''">
            and  username like "%"#{uname}"%"
          </if>
          <if test="usex!=null and usex!=''">
             and sex=#{usex}
          </if>
       </where>
    </select>
```



#### choose when orherwise

一般用做条件检索

映射文件

```xml
<!--choose when otherwise:某个判断满足条件后，其他条件就不会再执行  -->
    <select id="getByNameSex1" parameterType="map" resultType="user">
       select * from user where
       <choose>
          <when test="uname!=null and uname!=''">
             username like "%"#{uname}"%"
          </when>
          <when test="usex!=null and usex!=''">
             sex=#{usex}
          </when>
          <otherwise>
              1=1
          </otherwise>
       </choose>
    </select>
```



#### set if☆☆☆☆☆

用于动态更新某些字段

用于update的条件修改

```xml
<!-- set:解析为set关键字，可以自动去掉最后一个更新的字段后面的逗号 -->
<update id="updUser" parameterType="user">
       update  user
       <set>
          <if test="username!=null and username!=''">
              username=#{username}, 
          </if>
          <if test="sex!=null and sex!=''">
              sex=#{sex}
          </if>
       </set>
       where id=#{id}
    </update>
```



#### trim

格式化sql语句,同时设置sql前后缀

```xml
<update id="updUser1" parameterType="user">
      <!--prefix:前缀，在trim中内容执行之前拼接
          suffix：后缀：在trim中内容执行之后拼接
          suffixOverrides：忽略后缀
          prefixOverrides:忽略前缀-->
      <trim prefix="update  user set" suffix="where id=#{id}" suffixOverrides=",">
          
	          <if test="username!=null and username!=''">
	              username=#{username}, 
	          </if>
	          <if test="sex!=null and sex!=''">
	              sex=#{sex}
	          </if>
          
      </trim>
    </update>
```



#### bind

在输入字符串的同时拼接一些其他的字符组成一个新的字符串

```xml
<select id="getByUname" parameterType="string" resultType="User">
      <!--name:新的字符串的变量名  -->
      <bind name="uname" value="'%'+_parameter+'%'"/>
      select * from user where username like #{uname}
   </select>

```



### 关系映射

处理关系映射时可以采用自动映射和手动映射(关联查询,嵌套查询(支持延迟加载)).



#### resultMap可以用于继承

```xml
<resultMap id="CarResultPulic" type="car">
		<id property="cid" column="cid"></id>
		<result property="cname" column="cname"></result>
		<result property="color" column="color"></result>
		<result property="productDate" column="product_date"></result>
</resultMap>

<resultMap id="CarResult" type="car" extends="CarResultPulic">
		<association property="motorcycle" javaType="Motorcycle">
			<result property="motorcycleType" column="motorcycle_type"></result>
		</association>
	</resultMap>

	<resultMap id="CarResultM" type="car" extends="CarResultPulic">
		<association property="motorcycle" javaType="motorcycle" select="com.jia.ssm.day2.dao.CarDao.findMotorcycleList" column="mid">
		</association>
	</resultMap>
```



#### 一对一

##### 自动映射

```xml
<!-- 1-1:自动映射 -->
    <select id="oneToOne" resultType="UserView">
        select u.*,c.num from user u,card c where u.id=c.per_fk
    </select>

```



##### 手动映射

1、把表与表之间的关系转化为实体和实体之间的关系.

​      在其中一方加入另一方的实体类属性(在一个类中加载另外一个类中的关系)

2、在映射文件中完成手动映射

######    $\textcolor{red}{关联查 询 :通过一个sql语句查到结果,resultMap完成映射}$

关联映射查询数据,所有的列名都要配置映射,如果列名没有映射,则查出的数据显示为空

```xml
<!-- 1-1:手动映射之级联查询  -->
    <resultMap type="user" id="oTors">
       <id column="id" property="id"/>
       <result column="username" property="username"/>
       <result column="sex" property="sex"/>
       <!--association:完成自定义类型的映射
           property:User类中自定义类型（Card）的属性名
           javaType:指定对应属性的类型-->
       <association property="card" javaType="card" resultMap="cardrs">
           <result property="num" column="num"/>
       </association>
    </resultMap>
    <select id="oneToOne1" resultMap="oTors">
        select u.*,c.num from user u,card c where u.id=c.per_fk
    </select>
```

###### **$\textcolor{red}{ 嵌套查询:先查询主信息表的信息,然后在通过其他sql语句查询关联信息}$**

嵌套映射查询中,如果列名与类中的属性名一一对应则不需要配置相应的映射,所有数据都会自动映射

```xml
<!-- 1-1:手动映射之嵌套查询（分步）  -->
    <resultMap type="User" id="oTors2" >
       <id column="id" property="id"/>
       <result column="username" property="username"/>
       <result column="sex" property="sex"/>
       <!-- select:调用另外一条sql语句（命名空间.sql语句id）
            column:在第一个结果集中用来关联查询的列名 -->
       <association property="card" javaType="card" 
       select="com.offcn.dao.UserDao.getCardByUid" column="id"></association>
    </resultMap>
    <select id="oneToOne2" resultMap="oTors2">
       select  * from user  
    </select>
    
    <select id="getCardByUid" parameterType="int" resultType="card">
       select * from card where per_fk=#{uid}
    </select>

```



#### 一对多

手动映射

映射代码:

```xml
<!-- 1-n:关联查询 -->
    <resultMap type="Orders" id="orderrs">
           <id column="oid" property="id"></id>
           <result column="number" property="number"/>
    </resultMap>
    <resultMap type="user" id="oTmrs" extends="commrs">
        <collection property="olist" ofType="Orders" resultMap="orderrs">
            <id column="oid" property="id"></id>
           <result column="number" property="number"/>
        </collection>
    </resultMap>
    <select id="oneToMany" resultMap="oTmrs">
       select u.*,o.id oid,o.number from user u,orders o where u.id=o.userId
    </select>
```

嵌套映射

映射代码:

```xml
<!--1-n:嵌套查询-->
    <resultMap type="user" id="oTmrs2">
        <collection property="olist" ofType="Orders" 
        select="com.offcn.dao.UserDao.getOrdersByUid" column="id"></collection>
    </resultMap>
    <select id="oneToMany2" resultMap="oTmrs2">
       select * from user
    </select>
    
    <select id="getOrdersByUid" parameterType="int" resultType="Orders">
       select * from orders where userId=#{uid}
    </select>

```



#### 多对多

映射代码:

```xml
<!-- 查询某个用户购买所有的商品（商品购买的订单号，下单人的信息，以及商品购买的数目） -->
    <resultMap type="user" id="mTmrs">
        <result column="username" property="username"/>
        <collection property="olist" ofType="Orders">
           <result property="number" column="number"/>
           <collection property="dlist" ofType="Orderdetail">
              <result column="itemsNum" property="itemsNum"/>
              <association property="items" javaType="Items">
                  <result column="name" property="name"/>
                  <result column="price" property="price"/>
              </association>
           </collection>
        </collection>
    </resultMap>
    <select id="manyToMany" resultMap="mTmrs">
       select u.username,o.number,i.name,i.price,d.itemsNum  
       from user  u,orders o,orderdetail d,items i 
       where u.id=o.userId and o.id=d.ordersId and d.itemsId=i.id
	</select>
```



## 延迟加载

先加载主信息表的信息,在需要关联信息的时候,在查询对应的关联信息

配置;

在全局配置文件里配置延迟加载

```xml
<setting name="lazyLoadingEnabled" value="true"/>
```

**$\textcolor{red}{注意:延迟加载只用于嵌套查询}$**



## 缓存机制

1.为了避免频繁的从数据库中查询数据,所以会将常用的数据放入缓存中

2.Mybatis提供的缓存机制有两种



### 一级缓存

   一级缓存:Mybatis提供的一种缓存机制,不需要开发人员提供任何配置.Sqlsession级别的缓存,每一个Sqlsession      	只能使用自己的一级缓存区域数据,不能跨sqlsession共享一级缓存.在执行增删改只有一级缓存里面的数据会自	动清除也可以手动清除.使用  clear方法



测试案例:

```java
@Test
   public void oneCache() {
	   User user = udao.getUser();
	   System.out.println("第一次查询："+user);
	   
	    /*udao.delMultiByArray(new int[] {41,45});
	    ss.commit();*/
	   ss.clearCache();//清除一级缓存区域的数据
	   
	   User user2=udao.getUser();
	   System.out.println("第二次查询："+user2);
   }
```



### 二级缓存

​     二级缓存:二级缓存是SqlSessionFactory级别的缓存,或者说是mapper接口处的缓存.不同的Sqlsession操作一个	 mapper接口可以共享数据



手动设置:

1.指定二级缓存内容(Mybatis提供的二级缓存实现类,也可以是用第三方插件提供的Mybatis二级缓存（ehcahce,redis）),在增删改后二级缓存区域的数据也会被清除掉

```xml
<!-- 指定二级缓存的类型,type省略不写默认是自带的-->
<cache type="org.apache.ibatis.cache.impl.PerpetualCache"></cache>
```

2.序列化缓存的mapper接口

```java
public interface DeptMapper extends Serializable
```



代码案例:

```java
@Test
   public void secondCache() {
	   User user = udao.getUser();//第一个SqlSession对象ss来操作UserDao接口
	   System.out.println("第一次查询："+user);
	   
	   
	    udao.delMultiByArray(new int[] {41,45});
	    ss.commit();
	    ss.close();
	   
	   
	   User user2=udao1.getUser();//第二个SqlSession对象ss1来操作UserDao接口
	   System.out.println("第二次查询："+user2);
	   ss1.close();
   }
```

