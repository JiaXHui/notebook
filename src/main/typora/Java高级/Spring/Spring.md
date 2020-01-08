# Spring框架

spring介绍

1.开源的一站式开发框架

   spring:完成三成架构中单例模式对象的实例化,例如dao层以及service层的对象

2.特点

​    可以达到解耦的效果,降低三层框架的耦合度

​    支持AOP编程,实现事物的声明式管理

​    可以兼容其他优秀的框架

​    支持springboot,springcloud等微服务开发



## Spring常用maven包

```xml
 <!-- spring上下文依赖注入 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- spring核心工具 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- spring依赖注入 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- spring表达式 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-expression</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
 <!-- spring里面集合的jdbc架构 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- srpingAPI管理器 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- srping面向切面 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
```





## Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/tx
                      http://www.springframework.org/schema/tx/spring-tx.xsd
                      http://www.springframework.org/schema/aop
                      http://www.springframework.org/schema/aop/spring-aop.xsd
                      http://www.springframework.org/schema/mvc
                      http://www.springframework.org/schema/mvc/spring-mvc.xsd
                      http://www.springframework.org/schema/beans
                      http://www.springframework.org/schema/beans/spring-beans.xsd
                      http://www.springframework.org/schema/context
                      http://www.springframework.org/schema/context/spring-context.xsd">


	<bean id="ud" class="com.offcn.dao.UserDaoImpl"></bean>
	
	
</beans>
```



解析配置文件:

```java
public class App {
   public static void main(String[] args) {
	   //配置文件的解析
	  ApplicationContext ap=new ClassPathXmlApplicationContext("beans.xml");
	  UserDaoImpl udao=(UserDaoImpl) ap.getBean("ud");
	  udao.save();
   }
}
```



## bean节点属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/tx
                        http://www.springframework.org/schema/tx/spring-tx.xsd
                        http://www.springframework.org/schema/aop
                        http://www.springframework.org/schema/aop/spring-aop.xsd
                        http://www.springframework.org/schema/mvc
                        http://www.springframework.org/schema/mvc/spring-mvc.xsd
                        http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/context
                        http://www.springframework.org/schema/context/springcontext.xsd">
                        


    <!--id:唯一
        name:作用和id一样的，但是可以为一个节点命名多个名字
        class：要实例化的对象的类型
        lazy-init="true":设置延时加载实例化对象,针对某一个bean节点
        default-lazy-init="true"：针对全部bean节点 
        scope="prototype":以多例（原型）模式实例化对象
        init-method:初始化bean节点后执行的方法
        destroy-method：销毁bean节点时执行的方法-->
	<bean id="ud" 
          name="ud1,ud2,ud3" 
          class="com.offcn.dao.UserDaoImpl" 
          init-method="birth" 
          destroy-method="died">
    </bean>
</beans>
```



### 解析bean.xml的三种方式

1、配置文件相对src的路径

ApplicationContext ap=new ClassPathXmlApplicationContext("beans.xml");

2.配置文件在硬盘中的绝对路径

ApplicationContext ap=new FileSystemXmlApplicationContext("D:\\myworksp\\spring001\\src\\beans.xml");

3.解析配置文件通过绝对路径，默认实例化对象是延时的

BeanFactory ap=new XmlBeanFactory(new FileSystemResource("D:\\myworksp\\spring001\\src\\beans.xml"));



## Spring的核心思想IOC,DI



### IOC控制反转

IOC（Inversion of Control）控制反转，应用程序本身不负责对象的创建（实例化），对象的实例化由Spring 容器（配置文件）完成。

Spring实例化对象常用的三种方式：

#### 1.接口实现类方式

定义接口和接口对应的实现类

配置：

```xml
<!--1.接口，实现类方式，class:实现类 -->
<bean id="pd" class="com.offcn.dao.PerDaoImpl"></bean>
```



#### 2.静态工厂模式

即通过工厂类里的静态方法得到需要的对象

创建一个工厂类，在工厂类创建一个静态方法得到需要的对象

配置：

```XML
<!--2.静态工厂模式 -->
<bean id="pd1" class="com.offcn.utils.PerDaoFactory" factory-method="getPserDao"></bean>
```



#### 3.工厂模式

即通过工厂类里的非静态方法得到需要的对象

```xml
<!-- 3.工厂模式 -->
<bean id="sf" class="com.offcn.utils.PerDaoFactory"></bean>
<bean id="pd2" factory-bean="sf" factory-method="getPserDao1"></bean>
```





### DI依赖注入

DI(Dependency Injection)依赖注入,为Spring容器实例化对象的属性赋值的过程称为依赖注入的过程。通俗的说就是在spring实例化某个类时，为这个类依赖的属性赋值的过程称为依赖注入。



#### set注入

```JAVA
@Accessors
public class PerServiceImpl implements PerService {

	PerDao pd;
	int pid;
	String name;
	List<Object> list;
	Set<Object> set;
	Map<String,Object> map;
	Object[] array;

	@Override
	public void save() {
		// TODO Auto-generated method stub
        pd.save();
        System.out.println("pid:"+pid+"----"+name);
        for(Object o:list) {
        	System.out.println("list:"+o);
        }
        
        for(Object o:set) {
        	System.out.println("set:"+o);
        }
        
        for(Object o:array) {
        	System.out.println("array:"+o);
        }
        
        Set<String> keySet = map.keySet();
        for(Object k:keySet) {
        	System.out.println("map key:"+k+"----"+map.get(k));
        }
	}
}
```

在配置文件beans.xml中完成注入：

```xml
<bean id="ps" class="com.offcn.service.PerServiceImpl">
	  <property name="pd" ref="pd"></property>
	  <property name="pid" value="12"></property>
	  <property name="name" value="弟弟"></property>
	  <property name="list">
	    <list>
	       <value>14</value>
	       <value>14</value>
	       <value>kl</value>
	       <!-- <ref bean="pd"/> -->
	       <bean class="com.offcn.dao.PerDaoImpl"></bean>
	    </list>
	  </property>
	  <property name="map">
	     <map>
	        <entry key="map001">
	           <value>jlkjlk</value>
	        </entry>
	        <entry key="map002">
	           <ref bean="ud"/>
	        </entry>
	     </map>
	  </property>
	  <property name="set">
	    <set>
	       <value>14</value>
	       <value>14</value>
	       <value>kl</value>
	       <!-- <ref bean="pd"/> -->
	       <bean class="com.offcn.dao.PerDaoImpl"></bean>
	    </set>
	  </property>
	  <property name="array">
	    <array>
	       <value>14</value>
	       <value>14</value>
	       <value>kl</value>
	       <!-- <ref bean="pd"/> -->
	       <bean class="com.offcn.dao.PerDaoImpl"></bean>
	    </array>
	  </property>
	</bean>
```





#### 构造注入

通过有参构造方法，为类中所依赖的属性赋值。

按照参数的索引注入(index)

```xml
<bean id="ss" class="com.offcn.service.StuServiceImpl">
	    <constructor-arg index="0">
	     <value>12</value>
	   </constructor-arg>
	   <constructor-arg index="1">
	     <value>小牧</value>
	   </constructor-arg>
</bean>
```

按照参数的类型注入(type)

```xml
<bean id="ss" class="com.offcn.service.StuServiceImpl">
	  <constructor-arg type="String">
	     <value>12</value>
	   </constructor-arg>
	   <constructor-arg type="String">
	     <value>小牧</value>
	   </constructor-arg> 
</bean>
```

按照参数的名称注入(name)

```xml
<bean id="ss" class="com.offcn.service.StuServiceImpl"> 
<constructor-arg name="sid" index="0">
	     <value>12</value>
	   </constructor-arg>
	   <constructor-arg name="uname" index="1">
	     <value>小牧</value>
	   </constructor-arg>
</bean>
```

注意：三种方式可以合起来一起使用，唯一确定一个有参构造方法。

**练习**

**Spring**方式完成数据源的实例化

**以c3po**的连接池为例。

```xml
<bean id="ds" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	   <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
	   <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/hibdata"></property>
	   <property name="user" value="root"></property>
	   <property name="password" value="root"></property>
</bean>
```

**以dbcp**连接池为例。

```xml
<bean id="ds1" class="org.apache.commons.dbcp.BasicDataSource">
	   <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
	   <property name="url" value="jdbc:mysql://localhost:3306/hibdata"></property>
	   <property name="username" value="root"></property>
	   <property name="password" value="root"></property>
</bean>
```





## JdbcTemplate插件



加入jar包

```xml
 <!-- spring里面集合的jdbc架构 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-jdbc</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
    <!-- srpingAPI管理器 -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-tx</artifactId>
      <version>5.1.5.RELEASE</version>
    </dependency>
```

在bean包下面定义实体类

dao层中通过JdbcTemplate完成CURD操作接口NewsDao



接口NewsDao:

```java
import com.offcn.bean.News;

public interface NewsDao {
   public List<News> getAll();
   public News getByNid(int nid);
   public int save(News news);
   public int update(News news);
   public int del(int nid);
}
```

接口实现类NewsDaoImpl:

```java
public class NewsDaoImpl implements NewsDao {
    
	JdbcTemplate jt;
	
	public void setJt(JdbcTemplate jt) {
		this.jt = jt;
	}

	@Override
	public List<News> getAll() {
		// TODO Auto-generated method stub
		String sql="select * from news";
		RowMapper<News> rm=new BeanPropertyRowMapper<News>(News.class);
		List<News> nlist = jt.query(sql, rm);
		return nlist;
	}

	@Override
	public News getByNid(int nid) {
		// TODO Auto-generated method stub
		String sql="select * from news where nid="+nid;
		RowMapper<News> rm=new BeanPropertyRowMapper<News>(News.class);
		News news = jt.queryForObject(sql, rm);
		return news;
	}

	@Override
	public int save(News news) {
		// TODO Auto-generated method stub
		String sql="insert into news values(null,?,?,?)";
		return jt.update(sql, news.getTitle(),news.getContent(),news.getPhoto());
	}

	@Override
	public int update(News news) {
		// TODO Auto-generated method stub
		String sql="update news set title=?,content=? where nid=?";
		return jt.update(sql, news.getTitle(),news.getContent(),news.getNid());
	}

	@Override
	public int del(int nid) {
		// TODO Auto-generated method stub
		return jt.update("delete from news where nid="+nid);
	}

}
```



配置文件链接数据库连接池

```xml
<!-- 实例化dbcp连接池 -->
	<bean id="ds1" class="org.apache.commons.dbcp.BasicDataSource">
	   <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
	   <property name="url" value="jdbc:mysql://localhost:3306/hibdata"></property>
	   <property name="username" value="root"></property>
	   <property name="password" value="root"></property>
	</bean>
	
	<!-- 实例化JdbcTemplate-->
	<bean id="jdtm" class="org.springframework.jdbc.core.JdbcTemplate">
	   <!-- 依赖连接池 -->
	   <property name="dataSource" ref="ds1"></property>
	</bean>
	
	<!-- 实现dao层实现类NewsDaoImpl -->
	<bean id="nd" class="com.offcn.dao.NewsDaoImpl">
	  <!--依赖Jdbctemplate  -->
	  <property name="jt" ref="jdtm"></property>
	</bean>
```



测试类:

```java
public class App2 {
   public static void main(String[] args) throws IllegalAccessException, IllegalArgumentException, InvocationTargetException, NoSuchMethodException, SecurityException {
	   ApplicationContext ap=new ClassPathXmlApplicationContext("beans.xml");
	   NewsDao nd=(NewsDao) ap.getBean("nd");
       
	   //查询所有数据
//	   List<News> nlist = nd.getAll();
//	   System.out.println(nlist);
	   
	   //根据逐渐查询
//	   News news = nd.getByNid(4);
//	   System.out.println(news);
	   
	   //保存
//	   News news=new News();
//	   news.setTitle("第三方");
//	   news.setContent("取消时序");
//	   news.setPhoto("no.gif");
//	   int row = nd.save(news);
//	   System.out.println(row);
	   
	   //更新
//	   News news=new News();
//	   news.setNid(17);
//	   news.setTitle("第三方wdd");
//	   news.setContent("取消时序ddd");
//	   news.setPhoto("no.gif");
//	   int row = nd.update(news);
//	   System.out.println(row);
	   
	   
	   //删除
	   int row = nd.del(17);
	   System.out.println(row);
	   
   }
}
```

## 注解方式一站开发



### @Repository

@Repository:注解在dao层上面用来实例化dao层对象



### @Service

@Service:注解在service层上面用来实例化service层对象



### @AutoWired/@Resource

@AutoWired/@Resource:作用在依赖的属性上面,用来给依赖的属性复制



### @Controller/@Action

@Controller(spring的注解)/@Action(status2的注解):作用在控制层上面用来实例化控制层对象



### @Component

@Component:总用在三层架构以外的对象上面,用来实例化这些对象



### 扫描

1.开启扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:mvc="http://www.springframework.org/schema/mvc"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/tx
                        http://www.springframework.org/schema/tx/spring-tx.xsd
                        http://www.springframework.org/schema/aop
                        http://www.springframework.org/schema/aop/spring-aop.xsd
                        http://www.springframework.org/schema/mvc
                        http://www.springframework.org/schema/mvc/spring-mvc.xsd
                        http://www.springframework.org/schema/beans
                        http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/context
                        http://www.springframework.org/schema/context/springcontext.xsd">

    <!--开启注解扫描-->
   <context:component-scan base-package="com.offcn"></context:component-scan>
```

2.加入依赖的jar包

```xml
 <!-- srping面向切面 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>
```

3.在dao层,service层实现类上面,和所依赖的属性上面分别加上注解

**dao层:**

```java
@Repository  //<bean id="ndao" class="当前类">
public class NewsDaoImpl implements NewsDao {
	/* 指定名称：先按照当前属性的名称name找到对应的bean节点，赋值给当前属性;
	 *若没有找到对应的节点会按照属性的类型查找对应的bean节点，赋值给当前属性
	 *没有指定名称：先按照当前属性的默认名称（属性类型的首字母小写）找到对应的bean节点，赋值给当前属性;
	 *若没有找到对应的节点会按照属性的类型查找对应的bean节点，赋值给当前属性
	 */
	//@Resource /* (name="jdbcTemplate") */
	@Autowired //按照当前属性（jt）的类型（JdbcTemplate）查找到系统中该类型的bean节点，赋值给当前属性。
	JdbcTemplate jt;
	
	
//	public void setJt(JdbcTemplate jt) {
//		this.jt = jt;
//	}
	@Override
	public List<News> getAll() {
		// TODO Auto-generated method stub
		RowMapper<News> rm=new BeanPropertyRowMapper<News>(News.class);
		return jt.query("select * from news", rm);
	}
}
```



**service层:**

```java
@Service  //<bean id="ns" class>
public class NewsServiceImpl implements NewsService {
	//@Resource//<property name="nd" ref="ndao">
	@Autowired
	NewsDao nd;
//	public void setNd(NewsDao nd) {
//		this.nd = nd;
//	}
	@Override
	public List<News> getNews() {
		// TODO Auto-generated method stub
		return nd.getAll();
	}
}

```



**测试:**

```java
public class NewsController {
   public static void main(String[] args) {
	  ApplicationContext ap=new ClassPathXmlApplicationContext("beans.xml");
	  NewsService ns=(NewsService) ap.getBean("newsServiceImpl");
	  List<News> nlist = ns.getNews();
	  System.out.println(nlist);
   }
}
```



### **细节**

@Resource注解:

指定名称:先按照当前name指定的名称去找对应的bean节点,赋值给当前属性;

​                 若没有找到相应的属性会去按照属性的类型查找到相应的bean节点,然后赋值给属性

没有指定名称:先按照当前属性默认的属性名称(属性名称首字母小写)去找相应的bean节点,若果没有找到

​                         会按照当前属性的类型去找到相应的bean节点

@AutoWired

会按照当前属性的类型找到相应的bean节点,然后赋值给当前的属性



## AOP的介绍以及动态代理



**Aop介绍**

AOP：（Aspect Oriented Programming）面向切面编程。通过[预编译](https://baike.baidu.com/item/预编译/3191547)方式和运行期动态代理实现程序功能的统一维护的一种技术。AOP是[OOP](https://baike.baidu.com/item/OOP)的延续，是软件开发中的一个热点，也是[Spring](https://baike.baidu.com/item/Spring)框架中的一个重要内容，是[函数式编程](https://baike.baidu.com/item/函数式编程/4035031)的一种衍生范型。利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的[耦合度](https://baike.baidu.com/item/耦合度/2603938)降低，提高程序的可重用性，同时提高了开发的效率。



1.依赖动态代理实现功能的统一维护

2.Aop是面向对象的一种补充,将系统中和主要业务无关的代码提出来统一封装到一个类的方法里面,将封装的类和                   代码块形成切面

3.纵向重复,横向抽取

4.降低业务和辅助业务的耦合度,同时提高代码的重要性



### 代理

一个机构或一个人,代理另外一个结构或另外一个人完成一部分工作.一个方法或一个类代理另外一个方法或另外一类完成一部分服务的其他业务,降低主要业务和辅助业务的耦合度.同时通过辅助业务对主要业务进行增强



#### 静态代理

静态代理:为每一个目标类生成一个代理类,静态代理中在编译期间就已经确定.静态代理相对于动态代理来说代理的效率要高一些,但是静态代理代码亢余量大



#### 动态代理

动态代理:对于每一个目标类(需要代理的类)通过反射在内存中生成一个代理类.动态代理是由jvm运行中生成,可以最大限度的降低代码亢余



常用的动态代理有两种jdk和CGLib代理



##### JDK代理

代理的目标类实现了接口(代理类和目标类实现了相同的接口)

```java
public class JDKProxyUtil {
	/*
	 *  为目标类生产代理类的方法
	 */
	public Object getProxy(Object tarObj) {
		/*
		 * 参数一：目标类的类加载器
		 * 参数二：目标类实现的接口
		 * 参数三：InvocationHandler: 代理类对象执行方法时内存中执行的内容
		 */
		return Proxy.newProxyInstance(tarObj.getClass().getClassLoader(),        							tarObj.getClass().getInterfaces(), new InvocationHandler() {
			
			/*
			 * 参数一：代理类对象
			 * 参数二：目标类的方法Method对象
			 * 参数三：目标类的方法参数
			 */
	@Override
	public Object invoke(Object proxy, Method method, Object[] args) throws Throwable{
				// TODO Auto-generated method stub
				//执行目标类的主要业务
				Object res = method.invoke(tarObj, args);
				//辅助业务
				System.out.println("提交事务");
				return res;
			}
		});
	}
	
}
```



##### CGLib代理

对代理目标类可以实现接口(可以使代理类和目标类可以实现相同的接口)

也可以是没有实现接口的目标类(代理类继承目标类)

加入依赖包

```xml
<!-- https://mvnrepository.com/artifact/org.ow2.asm/asm -->
<dependency>
    <groupId>org.ow2.asm</groupId>
    <artifactId>asm</artifactId>
    <version>5.0.4</version>
</dependency>
<!-- https://mvnrepository.com/artifact/cglib/cglib -->
<dependency>
    <groupId>cglib</groupId>
    <artifactId>cglib</artifactId>
    <version>3.3.0</version>
</dependency>

```

代码:

```java
public class CGLibProxy {
	/*
	 *  为目标类生产代理类的方法
	 */
	public Object getProxy(Object tarObj) {
		Enhancer en=new Enhancer();
		en.setClassLoader(tarObj.getClass().getClassLoader());//目标类的类加载器
		//en.setInterfaces(tarObj.getClass().getInterfaces());//目标类实现的接口
		en.setSuperclass(tarObj.getClass());
		en.setCallback(new MethodInterceptor() {
			
			@Override
			public Object intercept(Object proxy, Method method, Object[] args, MethodProxy arg3) throws Throwable {
				// TODO Auto-generated method stub
				//执行目标类的主要业务
				Object res = method.invoke(tarObj, args);
				//辅助业务
				System.out.println("提交事务");
				return res;
			}
		});//代理类对象执行方法时内存中执行的内容
		return en.create();
	}
	
}
```



### SpringAOP配置



1.连接点:需要增强的方法

2.切点:方法被增强以后形成为切点

3.通知:方法增加的内容称之为通知

4.切面:吧通知作用在切点上面称之为切面(切点+通知)

5.织面:吧通知作用在切点上面的过程称之为织面

6.目标类:要被代理(增强)的类

7.代理类:还有增强内容的类



#### XML配置文件完成配置

springAOP配置步骤:

1.依赖包

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/tx
                      http://www.springframework.org/schema/tx/spring-tx.xsd
                      http://www.springframework.org/schema/aop
                      http://www.springframework.org/schema/aop/spring-aop.xsd
                      http://www.springframework.org/schema/mvc
                      http://www.springframework.org/schema/mvc/spring-mvc.xsd
                      http://www.springframework.org/schema/beans
                      http://www.springframework.org/schema/beans/spring-beans.xsd
                      http://www.springframework.org/schema/context
                      http://www.springframework.org/schema/context/spring-context.xsd">
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjtools -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjtools</artifactId>
    <version>1.9.1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/aopalliance/aopalliance -->
<dependency>
    <groupId>aopalliance</groupId>
    <artifactId>aopalliance</artifactId>
    <version>1.0</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>

</beans>
```

2.准备目标类(UserService,UserServiceImpl)

3.准备代理类及增强的内容(辅助业务)

4.织入的完成:把代理的类的辅助业务作用在目标类中的方法上

配置文件:

```xml
 
<!-- 实例化业务层的实现类 -->
  <bean id="us" class="com.offcn.service2.UserServiceImpl"></bean> 
   <!-- 实例化代理类 -->
  <bean id="aspect" class="com.offcn.service2.ProxyObj"></bean> 
   
   <aop:config>
      <!--关联代理类-->
      <aop:aspect ref="aspect">
         <!-- 切点：指定在目标类中哪些方法要被增强  -->
         <aop:pointcut expression="execution( * com.offcn.service2.*.*(..))" id="pid"/>
         <!-- 前置通知：增强的内容在目标方法之前执行
              method：实现增强业务的方法（即代理类中含有辅助业务的方法） -->
         <!-- <aop:before method="toStrong" pointcut-ref="pid"/> -->
         <!-- 后置通知:辅助业务在主要业务之后执行 -->
         <!-- <aop:after-returning method="toStrong" pointcut-ref="pid" returning="res"/> -->
         <!-- <aop:after method="toStrong" pointcut-ref="pid"/> -->
         <!-- <aop:after-throwing method="toStrong" pointcut-ref="pid" throwing="exp"/> -->
         <aop:around method="toStrong1" pointcut-ref="pid"/>
      </aop:aspect>
   </aop:config>
```

 指定切点的语法

expression=execution(访问修饰 返回值类型 方法名（参数）)

访问修饰:（非必须）,可以省略不写支持任何访问修饰符

返回值类型：*，任意返回值类型

方法名：包名(*任意一级包,..包中所有内容).类名(*).方法名（*）

参数：（..任意参数） 

案例：

execution( * com.offcn.service2.*.*(..))：com.offcn.service2包中的任意类任意方法

execution( * com.offcn.service2..*(..))：com.offcn.service2包中的任意类或者任意包中的任意方法。

通知：

前置通知：辅助业务在主要业务之前执行。

后置通知：辅助业务在主要业务之后执行，当主要业务中出现异常，辅助业务不执行,在辅助业务中可以得到主要业务执行后的返回值。

最终通知：辅助业务在主要业务之后执行，不管主要业务中是否出现异常，辅助业务都会执行

异常通知：辅助业务在主要业务之后执行，只有在主要业务出现异常时，辅助业务才会执行。

环绕通知：辅助业务可以在主要业务之前执行，辅助业务也可以在主要业务之厚执行，辅助业务也可以在主要业务之前，之后都执行。



#### 注解完成配置

1.spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/tx
                      http://www.springframework.org/schema/tx/spring-tx.xsd
                      http://www.springframework.org/schema/aop
                      http://www.springframework.org/schema/aop/spring-aop.xsd
                      http://www.springframework.org/schema/mvc
                      http://www.springframework.org/schema/mvc/spring-mvc.xsd
                      http://www.springframework.org/schema/beans
                      http://www.springframework.org/schema/beans/spring-beans.xsd
                      http://www.springframework.org/schema/context
                      http://www.springframework.org/schema/context/spring-context.xsd">
  <context:component-scan base-package="com.offcn.service2"></context:component-scan>
   <!--启用aop自动代理  -->
   <aop:aspectj-autoproxy></aop:aspectj-autoproxy>  
</beans>
```

2.目标类只用注解实例化

```java
@Service
public class UserServiceImpl implements UserService {
```

3.代理类

 使用注解实例化代理类

  ```java
//代理类
@Component
@Aspect
public class ProxyObj {
  ```

同时使用注解形成切面

```java
@Component
//切面
@Aspect
public class ProxyObj {
```

在辅助的方法业务上面指定注解的范围

```java
//辅助业务,JoinPoint:封装目标方法的属性（例如目标方法名称，参数）
	//@Before("execution( * com.offcn.service2.*.*(..))")
	@AfterThrowing(value = "execution( * com.offcn.service2.*.*(..))",throwing = "exp")
	public void toStrong(JoinPoint jp/* ,Object res */ ,Throwable exp ) {
		Object[] args = jp.getArgs();
		for(Object o:args) {
			System.out.println("目标方法的参数："+o);
		}
		//System.out.println("目标方法(即主要业务方法)的返回值:"+res);
		System.out.println("主要业务中发生的异常："+exp);
		System.out.println("辅助业务，提交事务。。。。");
	}
```



### Spring事务

springAOP的思想完成声明式事务配置



1.账户表

2.记录表

给8888账户转10000

账户表：1008 余额 +10000-----修改

记录表：1008 10000-------插入



#### 事务的的传播行为

| 传播行为      | 意义                                                         |
| ------------- | ------------------------------------------------------------ |
| REQUIRED      | 业务方法需要在一个事务中运行.如果事务运行时,已经处在一个事务中,name加入到该事物,否则为自己创建一个新事物 |
| NOT_SUPPORTED | 声明方法不需要事物,如果方法没有关联到一个事务,容器不会为他开启一个事务.如果方法在一个事务中被调用,该事物会被挂起,在方法调用结束后,原先的事务会被回复执行 |
| REQUIRES_NEW  | 属性表用不管是否存在事务,业务方法总会为自己发起一个新的事务.如果方法已经运行在一个事务中,则原有事务会被挂起.新的事务会被创建,直到方法执行结束,新的是错误才算结束,原先的事务才会恢复执行 |
| MANDATORY     | 该属性指定该业务方法只能在一个已经存在的事务中,业务方法不能发起自己的事务.如果业务方法在事务范围外调用,容器会被抛出 |
| SUPPORTS      | 这一事务属性表名,如果业务方法在事务范围内被调用,则方法成为该事物的一部分.如果业务方法在事务范围外被调用,则方法在没有事务环境下进行 |
| NEVER         | 指定业务方法绝对不能再事务范围内执行.若页业务方法在某一个事务中执行,容器会抛出例外,只有业务方法没有关联到任何事务,才能正常执行 |
| NESTED        | 如果一个活动的事务存在,则运行在一个嵌套事务中.如果没有活动事务,则按REQUIRED属性执行.它使用了单独的事务,这个事务拥有多个可以回滚的保存点,内部事务的回滚不会对外部事物产生影响他只对DataSourceTransactionManager事务管理器起效 |





#### xml配置事务

##### 实体类

###### Account(账户表)

```java
public class Account {
	private String accountid;
	private Double balance;

	public String getAccountid() {
		return accountid;
	}

	public void setAccountid(String accountid) {
		this.accountid = accountid;
	}

	public Double getBalance() {
		return balance;
	}

	public void setBalance(Double balance) {
		this.balance = balance;
	}

}
```

###### IcAccount(记录表)

```java
public class InAccount {
	private String accountid;
	private Double inbalance;

	public String getAccountid() {
		return accountid;
	}

	public void setAccountid(String accountid) {
		this.accountid = accountid;
	}

	public Double getInbalance() {
		return inbalance;
	}

	public void setInbalance(Double inbalance) {
		this.inbalance = inbalance;
	}

}
```

##### dao层

###### AccDao接口实现类

```java
public class AccDaoImpl implements AccDao {

	JdbcTemplate jt;
	
	public void setJt(JdbcTemplate jt) {
		this.jt = jt;
	}
	/*
	 * 更新账户
	 */
	@Override
	public void updaBalance(Account acc) {
		// TODO Auto-generated method stub
		String  sql="update account set balance=? where accountid=?lkkkkk";
        jt.update(sql, acc.getBalance(),acc.getAccountid());
	}
    /*
     * 根据账户id查询账户
     */
	@Override
	public Account getBtAccId(String accid) {
		// TODO Auto-generated method stub
		String sql="select * from account where accountid='"+accid+"'";
		RowMapper<Account> rm=new BeanPropertyRowMapper<Account>(Account.class);
		return jt.queryForObject(sql, rm);
	}

}

```

###### IcAccDao接口实现类

```java
public class InAccDaoImpl implements InAccDao {

	JdbcTemplate jt;
	
	public void setJt(JdbcTemplate jt) {
		this.jt = jt;
	}
    
	/*
	 * 保存转账记录
	 */
	@Override
	public void save(InAccount inacc) {
		// TODO Auto-generated method stub
		String sql="insert into inaccount values(?,?)";
        jt.update(sql, inacc.getAccountid(),inacc.getInbalance());
	}

}
```

##### Service层

###### AccService实现类

```java
public class AccServiceImpl implements AccService {
	InAccDao inaccDao;
	AccDao accDao;
	public void setInaccDao(InAccDao inaccDao) {
		this.inaccDao = inaccDao;
	}
	public void setAccDao(AccDao accDao) {
		this.accDao = accDao;
	}
	@Override
	public void transMoney(InAccount inacc) {
		// TODO Auto-generated method stub
		//插入转账记录
		inaccDao.save(inacc);
		//更新余额
		Account acc = accDao.getBtAccId(inacc.getAccountid());//获取账户
		acc.setBalance(acc.getBalance()+inacc.getInbalance());//设置账户的余额=原来的金额+转过来的金额
		accDao.updaBalance(acc);//更新账户信息
	}
}

```

##### beans.xml配置文件

```xml
<!-- 实例化数据源连接池 -->
   <bean id="ds" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	   <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
	   <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/springdata">   </property>
	   <property name="user" value="root"></property>
	   <property name="password" value="root"></property>
	</bean>
   <!-- 实例化JdbcTemplate -->
   <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate">
      <property name="dataSource" ref="ds"></property>
   </bean>  
   <!-- 实例化Dao层AccDaoImpl -->
   <bean id="accDao" class="com.offcn.dao.AccDaoImpl">
     <property name="jt" ref="jt"></property>
   </bean>  
   <!-- 实例化Dao层InAccDaoImpl -->
    <bean id="inaccDao" class="com.offcn.dao.InAccDaoImpl">
     <property name="jt" ref="jt"></property>
   </bean>
    <!-- 实例化Service层AccServiceImpl -->
    <bean id="accs" class="com.offcn.service.AccServiceImpl">
     <property name="inaccDao" ref="inaccDao"></property>
     <property name="accDao" ref="accDao"></property>
   </bean>
<!--事务的通知：事务的特性（隔离级别，传播行为，是否只读）  -->
<tx:advice id="myadvice" transaction-manager="tx">
      <tx:attributes>
     <!--isolation:隔离级别（是否可以防止脏读，不可重复读，幻读），不同的数据库隔离级别不一样 ，
                   DEFAULT  代表隔离级别和数据库保持一致
         propagation：传播行为（指定事务的运行环境），REQUIRED有事务环境使用当前事务没有就新开
                     启一个事务 
         read-only:只读性-->
         <tx:method name="*" isolation="DEFAULT" 
                    propagation="REQUIRED" read-only="true"/>
         <tx:method name="trans*" isolation="DEFAULT" 
                    propagation="REQUIRED" read-only="false"/>
         <tx:method name="save*" isolation="DEFAULT" 
                    propagation="REQUIRED" read-only="false"/>
         <tx:method name="upda*" isolation="DEFAULT" 
                    propagation="REQUIRED" read-only="false"/>
         <tx:method name="del*" isolation="DEFAULT" 
                    propagation="REQUIRED" read-only="false"/>
      </tx:attributes>
   </tx:advice>
   <aop:config>
      <aop:pointcut expression="execution( * com.offcn.service.*.*(..))" id="pid"/>
      <aop:advisor advice-ref="myadvice" pointcut-ref="pid"/>
   </aop:config>
```

##### Controller控制层

测试类

```java
public class AccController {
   public static void main(String[] args) {
	  ApplicationContext ap=new ClassPathXmlApplicationContext("com/offcn/beans.xml");
	  AccService acc=(AccService) ap.getBean("accs");
	  InAccount inacc=new InAccount();
	  inacc.setAccountid("8888");
	  inacc.setInbalance(10000d);
	  acc.transMoney(inacc);
   }
}
```



#### 注解方式完成事务



##### dao层

###### AccDao接口实现类

```java
@Repository
public class AccDaoImpl implements AccDao {
	@Autowired
	JdbcTemplate jt;
```

###### IcAccDao接口实现类

```java
@Repository
public class InAccDaoImpl implements InAccDao {

	@Autowired
	JdbcTemplate jt;
```



##### service层

```java
@Service
@Transactional(isolation = Isolation.DEFAULT,
               propagation =Propagation.REQUIRED,readOnly = true )
public class AccServiceImpl implements AccService {

	@Autowired
	InAccDao inaccDao;
	@Autowired
	AccDao accDao;
```

事务通知的特性写到事务中

```java
	@Override
	@Transactional(isolation = Isolation.DEFAULT,
                   propagation =Propagation.REQUIRED,readOnly = false )
	public void transMoney(InAccount inacc) {
		// TODO Auto-generated method stub
		//插入转账记录
		inaccDao.save(inacc);
		//更新余额
		Account acc = accDao.getBtAccId(inacc.getAccountid());//获取账户
		acc.setBalance(acc.getBalance()+inacc.getInbalance());//设置账户的余额=原来的金额+转过来的金额
		accDao.updaBalance(acc);//更新账户信息
	}
```



##### xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xsi:schemaLocation="http://www.springframework.org/schema/tx
                      http://www.springframework.org/schema/tx/spring-tx.xsd
                      http://www.springframework.org/schema/aop
                      http://www.springframework.org/schema/aop/spring-aop.xsd
                      http://www.springframework.org/schema/mvc
                      http://www.springframework.org/schema/mvc/spring-mvc.xsd
                      http://www.springframework.org/schema/beans
                      http://www.springframework.org/schema/beans/spring-beans.xsd
                      http://www.springframework.org/schema/context
                      http://www.springframework.org/schema/context/spring-context.xsd">
<!--开启注解扫描-->
 <context:component-scan base-package="com.offcn"></context:component-scan>

   <!-- 实例化数据源连接池 -->
   <bean id="ds" class="com.mchange.v2.c3p0.ComboPooledDataSource">
	   <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
	   <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/springdata"></property>
	   <property name="user" value="root"></property>
	   <property name="password" value="root"></property>
	</bean>

   <!-- 实例化JdbcTemplate -->
   <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate">
      <property name="dataSource" ref="ds"></property>
   </bean>

<!--开启注解事务-->
<bean id="tx" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
      <property name="dataSource" ref="ds"></property>
   </bean>
   <tx:annotation-driven transaction-manager="tx"/>
</beans>    
```



