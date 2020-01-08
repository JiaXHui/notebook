# SpringMvc框架



## Spring框架搭建及配置说明



### xml配置Controller

1.创建项目依赖的jar包

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
 <!--web框架 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>
<!--web框架 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.1.5.RELEASE</version>
</dependency>
```



2.在web.xml里配置前端页面

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
         xmlns="http://xmlns.jcp.org/xml/ns/javaee" 
		 xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee 
		 http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd" 
		 id="WebApp_ID" version="4.0">
  <display-name>springmvc001</display-name>
  
  <!--前端控制器 -->
  <servlet>
     <servlet-name>springmvc</servlet-name>
     <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
     <!-- 配置Springmvc的配置文件，默认路径在WEB-INF/servlet-name+"-servlet".xml -->
     <!-- <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:springmvc.xml</param-value>
     </init-param> -->
  </servlet>
  <servlet-mapping>
     <servlet-name>springmvc</servlet-name>
     <!-- /拦截所有后台请求 -->
     <url-pattern>/</url-pattern>
  </servlet-mapping>
  
  
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
</web-app>
```

3.编写SpringMVC配置文件

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
                        >
       
     <!-- 开启注解扫描 -->                   
     <context:component-scan base-package="com.offcn.controller">
    </context:component-scan>
     <!-- 开启springmvc的注解驱动 -->
     <mvc:annotation-driven></mvc:annotation-driven>
     
     <!-- 实例化非默认的映射处理器 -->
     <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping">      </bean>
     <!-- 实例化非默认的处理器适配器 -->
     <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
     <!-- 配置视图解析器 -->
     <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 前缀：/:WebContent路径 -->
        <property name="prefix" value="/"></property>
        <property name="suffix" value=".jsp"></property>
     </bean>
     <!--id:请求名，class：处理请求的处理器类  -->                   
     <bean id="/login" class="com.offcn.controller.LoginController"></bean>               
</beans>
```

4.自定义处理器类

```java
public class LoginController implements Controller{

	@Override
	public ModelAndView handleRequest(HttpServletRequest arg0, HttpServletResponse arg1)     throws Exception {
		// TODO Auto-generated method stub
		ModelAndView mv=new ModelAndView();
		mv.setViewName("index.jsp");
		mv.addObject("msg", "第一个Springmvc");
		return mv;
	}

}
```

测试  

[在浏览器访问](http://localhost:8080/springmvc001/login)



### 注释配置Controller

1.开启注解扫描

```xml
 <!-- 开启注解扫描 -->                   
<context:component-scan base-package="com.offcn.controller"></context:component-scan>
```

2.开启springMVC开启驱动

```xml
 <!-- 开启springmvc的注解驱动 -->
<mvc:annotation-driven></mvc:annotation-driven>
```

3.以注解的方式定义控制器类

```java
@Controller
@RequestMapping
public class WelController {
	@RequestMapping("reg") //匹配映射请求  u/reg
	public String hi() {
		System.out.println("通过注解实现控制器类。。。。");
		return "index";//前缀+返回字符串+后缀 
	}
}
```

注意：

1.使用注解时需要把spring-aop依赖包加进来。

2.@RequestMapping()//匹配映射请求，该注解可以写类上或者方法上。

3.所有@RequestMapping中value都省略，则该处理器称为默认处理器。当发送http://localhost:8080/springmvc001/请求后，先去查找静态页面，存在静态页面就使用，不存在就会访问默认处理器。



### SpringMVC实现传参

后台----->视图页面（处理器中类类型的参数会自动实例化）

request传值：Model,ModelAndView,Map,ModelMap

```java
@RequestMapping("d1")
	public String demo1(Model m) {
		m.addAttribute("msg", "通过model传值。。。。");
		return "index";//前缀+返回字符串+后缀 
	}
	
	@RequestMapping("d2")
	public ModelAndView demo2(ModelAndView mv) {
		mv.addObject("msg", "通过modelandview传值。。。。");
		mv.setViewName("index");
		return mv;
	}
	
	@RequestMapping("d3")
	public String demo3(Map m) {
		m.put("msg", "通过map传值。。。。");
		return "index";//前缀+返回字符串+后缀 
	}
	
	@RequestMapping("d4")
	public String demo4(ModelMap m) {
		m.addAttribute("msg", "通过modelmap传值");
		return "index";//前缀+返回字符串+后缀 
	}
```

session实现传参

```java
@RequestMapping("d5")
	public String demo5() {
		
		return "index";//前缀+返回字符串+后缀 
	}
	
	@RequestMapping("d6")
	public String demo6(HttpSession session) {
		session.setAttribute("msg", "seesion传值");
		return "index";//前缀+返回字符串+后缀 
	}
```

