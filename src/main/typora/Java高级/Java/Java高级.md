# JDBC



## JDBC介绍

**架构 : 由若干框架与若干技术组成的一个体系。我们称之为架构。**

​           **例如：一个平台项目包含一个架构体系。**

**框架 ： 由若干技术组成，若干技术组成的一个框架。（一个程序的半程平）**

**技术：技术点。 java、js.......**



**JDBC(java database connectivity | connection  ) Java 数据库连接。**

**作用：使用Java语言连接数据库。**





## JDBC快速入门

```html
<dependency>
      <groupId>commons-dbcp</groupId>
      <artifactId>commons-dbcp</artifactId>
      <version>1.4</version>
    </dependency>
 <dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.15</version>
  </dependency>
```



```java
//java语言提供了接口如下:
Connection 接口 : 连接的接口    
Statement | preparedStatement 接口:负责编译发送SQL接口
ResultSet 接口 ： 负责接受结果的接口
DriverManager ：java中负责初始化工作的一个类,驱动管理类,负责初始化工作


创建JDBC类:
1、加载驱动
2、创建连接
3、创建SQL编辑器
4、发送并返回结果
5、释放资源

public class JdbcQuickly {
	
	public static void main(String[] args) throws Exception {
		
		//1、加载驱动  - 让Java能力启动MySQL提供的项目服务
		Class.forName("com.mysql.cj.jdbc.Driver");
		//2、创建连接
		Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/school?serverTimezone=UTC", "root", "root");
		//3、创建SQL编译器
		Statement cs = connection.createStatement();
		//4、发送并返回结果
		int i = cs.executeUpdate("insert into king (name,years) value ('武则天','624-12-16 12:12:12')");
		System.out.println("i:"+i);
		//5、释放资源
		cs.close();
		connection.close();
	}
	
}
```





## JDBC详解



```JAVA
package com.ujiuye.jdbc2;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

/**
 * @说明：JDBC详解
 * @author 中公孙浩楠
 * @2019年11月4日下午3:16:26
 */
public class JDBCDetail {
	
	private static String driver = "com.mysql.cj.jdbc.Driver";
	private static String url = "jdbc:mysql:///school?serverTimezone=UTC";
	private static String username = "root";
	private static String password = "root";
	
	public static void main(String[] args) throws Exception {
		
		/**
		 * 加载驱动
		 * com.mysql.cj.jdbc.Driver
		 * 说明：8.*版本以后可以省略
		 * 5.*  : com.mysql.jdbc.Driver
		 * 8.*  : com.mysql.cj.jdbc.Driver
		 */
		Class.forName(driver);
		
		/**
		 * 创建连接
		 * jdbc:mysql://localhost:3306/数据库名
		 * ?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
		 * jdbc:声明当前连接数据库使用的是JDBC技术。
		 * mysql:表示当前JDBC连接的数据库是MySQL
		 * localhost：本地地址，如果是线上地址，直接写IP 本地地址可以省略
		 * 3306：端口号，如果是本地默认3306 可以省略   （jdbc:mysql:///数据库名）
		 * serverTimezone ： 5.*版本没有时差问题，不需要写。8.*升级后在国内使用必须指定同一时间  UTC
		 * 
		 * URL 问号前是地址。  问号后是参数。 多个参数使用&连接。
		 * useUnicode=true  告知MySQL数据操作时使用的编码格式和Java统一。
		 * characterEconding=utf-8 如果本地操作出现乱码。添加这句话立刻解决。
		 * 
		 */
		Connection connection = DriverManager.getConnection(url, username, password);
		
		/**
		 * 创建SQL编译器
		 * Statement SQL编译器  -- 案例中使用 （SQL注入）
		 * PreparedStatement SQL预编译器  
		 * 说明：SQL发送的载体。
		 */
		Statement cs = connection.createStatement();
		
		/**
		 * 执行SQL并返回结果
		 * execute 执行时数据会正常操作，但是返回结果boolean  true DQL false DML 
		 * 不能够验证执行是否正确，一般不用。
		 * executeUpdate执行DML操作  - 常用
		 * executeQuery 执行DQL操作 - 常用
		 */
		ResultSet rs = cs.executeQuery("select * from king");
		//使用while  next方法移动指针，并且返回bool 根据bool判断是否继续执行
		while(rs.next()){
			
			//取值  - 根据顺序取值字段多的时候容易乱
			/*int kid = rs.getInt(1);
			String name = rs.getString(2);
			String years = rs.getString(3);
			System.out.println("kid:"+kid + " name:" + name + " years:"+years);*/
			
			//使用列名取值 （推荐使用）
			String years = rs.getString("years");
			int kid = rs.getInt("kid");
			String name = rs.getString("name");
			System.out.println("kid:"+kid + " name:" + name + " years:"+years);
			
		}
		
		/**
		 * 释放资源
		 */
		rs.close();
		cs.close();
		connection.close();
		
	}
	
}


作业：使用JDBC完成CRUD操作（要求，能背写）
```





## 单元测试Junit

```mysql
是测试人员必会的工作，开发人员必须是使用的工具。
作用:解决一个类中只有一个main方法的问题。
关键词：@Test  要求类名不能出现test关键词。 
使用要求：导包
```





# 连接池



**数据库连接池  database pool**

**创建Connection极其消耗内存,底层为IO**



**手动实现一个连接池**

```java
package com.ujiuye.database.mypool;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;


/**
 * @说明：手动实现一个连接池
 * @author 中公孙浩楠
 * @2019年11月5日下午3:39:58
 */
public class MyDataBasePool {
	
	//创建一个存放连接的容器(池子)
	private static List<Connection> list = new ArrayList<>();
	//定义初始化连接池数量
	private static int init_number = 5;
	//定义最大连接
	private static int max_conn = 200;
	//定义每次创建连接数量
	private static int create_number = 20;
	
	/**
	 * 初始化连接池
	 */
	static {
		for (int i = 0; i < init_number; i++) {
			Connection connection = createConnectionSHN();
			//将获取的连接存放到池子中
			list.add(connection);
			System.out.println("connection init : "+ connection + "连接池数量："+list.size());
		}
	}
	
	/**
	 * 创建连接的方法
	 */
	private static Connection createConnectionSHN(){
		Connection connection = null;
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/db0909?serverTimezone=UTC", "root", "root");
		} catch (Exception e) {
			e.printStackTrace();
		}
		return connection;
	}
	
	
	/**
	 * 从连接池中获取数据
	 */
	public static Connection getConnectionSHN(){
		
		//从池子中获取连接
		Connection connection = list.get(0);
		//从池子中移除连接
		list.remove(connection);
		System.out.println("从连接池中获取连接："+connection + " 剩余连接："+list.size());
		
		return connection;
	}
	
	/**
	 * 使用完成 - 归还连接
	 * 将创建出的对象关闭，将纯净的连接归还给连接池
	 */
	public static void backConnectionSHN(Connection connection,PreparedStatement ps,ResultSet rs){
		
		if(rs != null){
			try {
				rs.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		if(ps != null){
			try {
				ps.close();
			} catch (SQLException e) {
				e.printStackTrace();
			}
		}
		//归还连接
		list.add(connection);
		System.out.println("归还连接："+connection + " 池子剩余："+ list.size());
		
	}
	
	
	
	
}

===测试代码
package com.ujiuye.database.mypool;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import org.junit.Test;

/**
 * @说明：测试连接池的使用
 * @author 中公孙浩楠
 * @2019年11月5日下午4:00:55
 */
public class TestMyDateBasePool {

	/**
	 * 测试连接增加
	 * @throws Exception
	 */
	@Test
	public void save() throws Exception{
		
		String sql = "insert into user (name,username,pwd) value (?,?,?)";
		Connection connection = MyDataBasePool.getConnectionSHN();
		PreparedStatement ps = connection.prepareStatement(sql);
		ps.setString(1, "山鸡");
		ps.setString(2, "shanji");
		ps.setString(3, "jiji123");
		int i = ps.executeUpdate();
		System.out.println(i);
		
		MyDataBasePool.backConnectionSHN(connection, ps, null);
		
	}
	
	/**
	 * 测试连接池重复使用
	 * @throws Exception 
	 */
	@Test
	public void repeatUse() throws Exception{
		Connection c1 = MyDataBasePool.getConnectionSHN();
		Connection c2 = MyDataBasePool.getConnectionSHN();
		Connection c3 = MyDataBasePool.getConnectionSHN();
		Connection c4 = MyDataBasePool.getConnectionSHN();
		Connection c5 = MyDataBasePool.getConnectionSHN();
		
		//归还一个
		MyDataBasePool.backConnectionSHN(c5, null, null);
		
		
		Connection c55 = MyDataBasePool.getConnectionSHN();
		
		String sql = "insert into user (name,username,pwd) value (?,?,?)";
		PreparedStatement ps = c55.prepareStatement(sql);
		ps.setString(1, "江小白");
		ps.setString(2, "xiaobai");
		ps.setString(3, "baibai123");
		int i = ps.executeUpdate();
		System.out.println(i);
		
		MyDataBasePool.backConnectionSHN(c55, ps, null);
		
	}
	
}
```





## DBCP连接池

**Commons   Apache软件基金会**

**功能:继承数据库连接+数据库**

**特点:速度快,高并发**

**缺点:不安全**



**使用连接池找数据源(连接数据的信息)**

**导包:**

**commonse-dbcp     commonse-pool**



[配置文件信息](https://blog.csdn.net/qq_41063182/article/details/82498441)

```mysql
package com.ujiuye.dbcp;

import java.io.IOException;
import java.io.InputStream;
import java.sql.Connection;
import java.util.Properties;

import javax.sql.DataSource;

import org.apache.commons.dbcp.BasicDataSourceFactory;

/**
 * @说明：测试DBCP
 * @author 中公孙浩楠
 * @2019年11月5日下午4:47:49
 */
public class MyDbcpPool() {
	
	private static Properties prop = new Properties();
	
	static{
		InputStream is = MyDbcpPool.class.getClassLoader().getResourceAsStream("dbcp.properties");
		try {
			prop.load(is);
		} catch (IOException e) {
			e.printStackTrace();
		}
	}
	
	/**
	 * 获取连接的方法
	 * @return
	 */
	public static Connection getConnectionSHN(){
		Connection connection = null;
		//寻找数据源
		try {
			DataSource ds = BasicDataSourceFactory.createDataSource(prop);
			connection = ds.getConnection();
		} catch (Exception e) {
			e.printStackTrace();
		}
		return connection;
	}
	
}

测试代码
package com.ujiuye.dbcp;

import java.sql.Connection;
import java.sql.PreparedStatement;

import org.junit.Test;

/**
 * @说明：
 * @author 中公孙浩楠
 * @2019年11月5日下午5:01:58
 */
public class TestDBCP {
	
	@Test
	public void tt() throws Exception{
		String hn = "insert into user (name) value (?)";
		//连接池中获取连接
		Connection connection = MyDbcpPool.getConnectionSHN();
		PreparedStatement ps = connection.prepareStatement(hn);
		ps.setString(1, "大飞哥");
		
		int i = ps.executeUpdate();
		System.out.println(i);
		//归还连接
		connection.close();
		
	}
	
}
```

**配置文件**

```properties
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql:///db0909?serverTimezone=UTC
username=root
password=root

#设置是否自动提交,默认为true
defaultAutoCommit=true
#设置数据库的事务隔离级别默认为1，READ_UNCOMMITTED，推荐设置为3
#defaultTransactionIsolation=3
#初始化数据池拥有的连接数量
initialSize=10
#池中最多可容纳的活着的连接数量，当达到这个数量不在创建连接
maxActive=20
#最大等待时间，超过这个时间等待队列中的连接就会失效
maxWait=2000
```



## C3P0连接池

**C3P0:效率偏低绝对安全,可以自动释放空间连接**

**导包**

**c3p0         mcchenge-commons-java**

[**C3P0属性信息**](**https://blog.csdn.net/futudeniaodan/article/details/62037826**)

```java
package com.ujiuye.c3p0;

import java.beans.PropertyVetoException;
import java.sql.Connection;
import java.sql.SQLException;

import com.mchange.v2.c3p0.ComboPooledDataSource;

/**
 * @说明：我的连接池
 * @author 中公孙浩楠
 * @2019年11月6日上午9:07:00
 */
public class MyC3p0Easy {
	
	private static ComboPooledDataSource ds = new ComboPooledDataSource();
	
	/**
	 * 获取连接
	 * @return
	 * @throws SQLException 
	 */
	public static Connection getConnectionSHN(){
		Connection connection = null;
		try {
			connection = ds.getConnection();
		} catch (SQLException e) {
			e.printStackTrace();
		}
		return connection;
	}
	
	
	public static void main(String[] args) {
		System.out.println(getConnectionSHN());
	}
	
}
```

```html
<!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.5.2</version>
</dependency>
 <!-- https://mvnrepository.com/artifact/com.mchange/mchange-commons-java -->
      <dependency>
          <groupId>com.mchange</groupId>
          <artifactId>mchange-commons-java</artifactId>
          <version>0.2.15</version>
      </dependency>

```



## DBUtils工具

**连接数据库的发展历史**

**JDBC：注册驱动、加载连接、创建编译器、发送返回结果、释放资源**

**JDBC封装：注册驱动、加载连接公共使用。**

**属性文件使用：解耦合**

**连接池：数据源（注册驱动、加载连接）**

**但是...**

**创建Statement或者preparedStatement**

**赋值，执行，返回结果后循环挨个获取数据。手动封装对象。**

**我们获取连接之后的操作还是过于麻烦。例如：查询返回结果。**

**可以使用持久化工具进行简化操作。**

**持久化工具就是和数据直接打交道的（CRUD）。**

**DBUtils 持久化工具**

**hibernate持久化框架**

**MyBatis 持久化框架**

​        **DBUtils是Apache基金会提供的一个对JDBC进行简单封装的开源工具类库,使用它能够简化JDBC应用程序的开发,同时也不会影响程序的性能.**

**使用DBUtils导包:**

   **commons-dbutils**

**核心类**

**QueryRunner**

```java
//定义一个C3p0数据源
private ComboPooledDataSource  ds = new ComboPooledDataSource();
//定义QueryRunner对象
private QueryRunner qr = new QueryRunner(ds);
```

**DBUtils可以自动帮我们释放资源**

```html
  <!-- https://mvnrepository.com/artifact/commons-dbutils/commons-dbutils -->
    <dependency>
      <groupId>commons-dbutils</groupId>
      <artifactId>commons-dbutils</artifactId>
      <version>1.6</version>
    </dependency>
```

