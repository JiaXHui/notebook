# Maven常用jar包



## lombok(get ,set方法注释)

```html
  <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.10</version>
  </dependency>
```

```java
@AllArgsConstructor
@NoArgsConstructor
@Data
@Accessors
//常见注释(实体类必加)
☆@AllArgsConstructor(生成有参构造方法)
☆@NoArgsConstructor(生成无参构造方法)
☆@Data（相当于 3、4、5和6中的@RequiredArgsConstructor）
☆@Accessors(注解用来配置lombok如何产生和显示getters和setters的方法)
 
//酌情用于常见类中
@SneakyThrows(将所有方法用 try catch 语句包裹起来并捕捉相应的异常)
@log4j(注解在类上；为类提供一个属性名为log的log4j日志对象，提供默认构造方法。)
----------------------------------------------------------------------------------------- 
        
1、@NonNull
2、@Cleanup
3、@Getter/@Setter
4、@ToString
5、@EqualsAndHashCode
6、@NoArgsConstructor/@RequiredArgsConstructor /@AllArgsConstructor
7、@Data
8、@Value
9、@SneakyThrows
10、@Synchronized
11、@Log
    
----------------------------------------------------------------------------------------- 
@NonNull   
//这个注解可以用在成员方法或者构造方法的参数前面，会自动产生一个关于此参数的非空检查，参数为空，则抛出一个空指   针异常，举个例子来看看：
//成员方法参数加上@NonNull注解
    public String getName(@NonNull Person p){
   			 return p.getName();
    }
//实际效果相当于：
    public String getName(@NonNull Person p){
        if(p==null){
            throw new NullPointerException("person");
        }
        return p.getName();
    }
-----------------------------------------------------------------------------------------@Cleanup
//这个注解用在变量前面，可以保证此变量代表的资源会被自动关闭，默认是调用资源的close()方法，如果该资源有其它   关闭方法，可使用@Cleanup(“methodName”)来指定要调用的方法
//用输入输出流来举个例子:
    public static void main(String[] args) throws IOException {
         @Cleanup InputStream in = new FileInputStream(args[0]);
         @Cleanup OutputStream out = new FileOutputStream(args[1]);
         byte[] b = new byte[1024];
         while (true) {
           int r = in.read(b);
           if (r == -1) break;
           out.write(b, 0, r);
         }
     }
//实际效果相当于：
    public static void main(String[] args) throws IOException {
         InputStream in = new FileInputStream(args[0]);
         try {
           OutputStream out = new FileOutputStream(args[1]);
           try {
             byte[] b = new byte[10000];
             while (true) {
               int r = in.read(b);
               if (r == -1) break;
               out.write(b, 0, r);
             }
           } finally {
             if (out != null) {
               out.close();
             }
           }
         } finally {
           if (in != null) {
             in.close();
           }
        }
    }
-----------------------------------------------------------------------------------------
@Getter/@Setter(知道就好已经有了替代)
//这一对注解从名字上就很好理解，用在成员变量前面，相当于为成员变量生成对应的get和set方法，同时还可以为生成的   方法指定访问修饰符，当然，默认为public 
//例子:
    public class Programmer{
        @Getter
        @Setter
        private String name;

        @Setter(AccessLevel.PROTECTED)
        private int age;

        @Getter(AccessLevel.PUBLIC)
        private String language;
    }
//实际效果相当于：
    public class Programmer{
        private String name;
        private int age;
        private String language;

        public void setName(String name){
            this.name = name;
        }

        public String getName(){
            return name;
        }

        protected void setAge(int age){
            this.age = age;
        }

        public String getLanguage(){
            return language;
        }
    }
-----------------------------------------------------------------------------------------
@ToString/@EqualsAndHashCode
    
/*这两个注解也比较好理解，就是生成toString，equals和hashcode方法，同时后者还会生成一个canEqual方法，用**于判断某个对象是否是当前类的实例，生成方法时只会使用类中的非静态和非transient成员变量，这些都比较好理解，**就不举例子了。
**当然，这两个注解也可以添加限制条件，例如用@ToString(exclude={“param1”，“param2”})来排除param1和**param2两个成员变量，或者用@ToString(of={“param1”，“param2”})来指定使用param1和param2两个成员变***量，@EqualsAndHashCode注解也有同样的用法。
*/
    
-----------------------------------------------------------------------------------------
@NoArgsConstructor/@AllArgsConstructor/@RequiredArgsConstructor
    
/*这三个注解都是用在类上的，第一个和第三个都很好理解，就是为该类产生无参的构造方法和包含所有参数的构造方法，第*二个注解则使用类中所有带有@NonNull注解的或者带有final修饰的成员变量生成对应的构造方法，当然，和前面几个注**解一样，成员变量都是非静态的，另外，如果类中含有final修饰的成员变量，是无法使用@NoArgsConstructor注解**的。
**三个注解都可以指定生成的构造方法的访问权限，同时，第二个注解还可以用*@RequiredArgsConstructor(staticName=”methodName”)的形式生成一个指定名称的静态方法，返回一个调用相*应的构造方法产生的对象，下面来看一个生动鲜活的例子：
*/

//下面来看一个生动鲜活的例子：
    @RequiredArgsConstructor(staticName = "sunsfan")
    @AllArgsConstructor(access = AccessLevel.PROTECTED)
    @NoArgsConstructor
    public class Shape {
        private int x;
        @NonNull
        private double y;
        @NonNull
        private String name;
    }
//实际效果相当于：
    public class Shape {
        private int x;
        private double y;
        private String name;

        public Shape(){
        }

        protected Shape(int x,double y,String name){
            this.x = x;
            this.y = y;
            this.name = name;
        }

        public Shape(double y,String name){
            this.y = y;
            this.name = name;
        }

        public static Shape sunsfan(double y,String name){
            return new Shape(y,name);
        }
    }
-----------------------------------------------------------------------------------------

@Data/@Value
    
/*@Data注解综合了3,4,5和6里面的@RequiredArgsConstructor注解，其中@RequiredArgsConstructor使用了**类中的带有@NonNull注解的或者final修饰的成员变量，它可以使@Data(staticConstructor=”methodName”)来*生成一个静态方法，返回一个调用相应的构造方法产生的对象。这个例子就也省略了吧…
*@Value注解和@Data类似，区别在于它会把所有成员变量默认定义为private final修饰，并且不会生成set方法。
*/
    
-----------------------------------------------------------------------------------------

@SneakyThrows
/*这个注解用在方法上，可以将方法中的代码用try-catch语句包裹起来，捕获异常并在catch中用*Lombok.sneakyThrow(e)把异常抛出，可以使用@SneakyThrows(Exception.class)的形式指定抛出哪种异常，很*简单的注解
*/
    
//直接看个例子：
    public class SneakyThrows implements Runnable {
        @SneakyThrows(UnsupportedEncodingException.class)
        public String utf8ToString(byte[] bytes) {
            return new String(bytes, "UTF-8");
        }

        @SneakyThrows
        public void run() {
            throw new Throwable();
        }
    }
//实际效果相当于：
    public class SneakyThrows implements Runnable {
        @SneakyThrows(UnsupportedEncodingException.class)
        public String utf8ToString(byte[] bytes) {
            try{
                return new String(bytes, "UTF-8");
            }catch(UnsupportedEncodingException uee){
                throw Lombok.sneakyThrow(uee);
            }
        }

        @SneakyThrows
        public void run() {
            try{
                throw new Throwable();
            }catch(Throwable t){
                throw Lombok.sneakyThrow(t);
            }
        }
    }
-----------------------------------------------------------------------------------------
@Synchronized
    
/*
*这个注解用在类方法或者实例方法上，效果和synchronized关键字相同，区别在于锁对象不同，对于类方法和实例方    *法，synchronized关键字的锁对象分别是类的class对象和this对象，而@Synchronized得锁对象分别是私有静态*final对象LOCK和私有final对象LOCK和私有final对象lock，当然，也可以自己指定锁对象
*/

//例子也很简单，往下看：
    public class Synchronized {
        private final Object readLock = new Object();

        @Synchronized
        public static void hello() {
            System.out.println("world");
        }

        @Synchronized
        public int answerToLife() {
            return 42;
        }

        @Synchronized("readLock")
        public void foo() {
            System.out.println("bar");
        }
    }
//实际效果相当于：
    public class Synchronized {
       private static final Object $LOCK = new Object[0];
       private final Object $lock = new Object[0];
       private final Object readLock = new Object();

       public static void hello() {
         synchronized($LOCK) {
           System.out.println("world");
         }
       }

       public int answerToLife() {
         synchronized($lock) {
           return 42;
         }
       }

       public void foo() {
         synchronized(readLock) {
           System.out.println("bar");
         }
       }
     }

-----------------------------------------------------------------------------------------    
@Log
    
/*
*这个注解用在类上，可以省去从日志工厂生成日志对象这一步，直接进行日志记录，具体注解根据日志工具的不同而不同， *同时，可以在注解中使用topic来指定生成log对象时的类名。不同的日志注解总结如下(上面是注解，下面是实际作用)：
*/
@CommonsLog
private static final org.apache.commons.logging.Log log = org.apache.commons.logging.LogFactory.getLog(LogExample.class);
@JBossLog
private static final org.jboss.logging.Logger log = org.jboss.logging.Logger.getLogger(LogExample.class);
@Log
private static final java.util.logging.Logger log = java.util.logging.Logger.getLogger(LogExample.class.getName());
@Log4j
private static final org.apache.log4j.Logger log = org.apache.log4j.Logger.getLogger(LogExample.class);
@Log4j2
private static final org.apache.logging.log4j.Logger log = org.apache.logging.log4j.LogManager.getLogger(LogExample.class);
@Slf4j
private static final org.slf4j.Logger log = org.slf4j.LoggerFactory.getLogger(LogExample.class);
@XSlf4j
private static final org.slf4j.ext.XLogger log = org.slf4j.ext.XLoggerFactory.getXLogger(LogExample.class);

```





## junit工具类(@Test)

```html
<!--推荐使用-->
<dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>compile</scope>
</dependency>
-----------------------------------------------------------------------------------------<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>RELEASE</version>
    <scope>compile</scope>
  </dependency>
```



## Tomcat 7

```html
<build>
    <plugins>
      <!-- 配置Tomcat插件 -->
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.2</version>
        <configuration>
          <path>/</path>
          <port>8080</port>
        </configuration>
      </plugin>
    </plugins>
  </build>
```

