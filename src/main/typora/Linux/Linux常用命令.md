# linux



虚拟机：是系统的载体，可以模拟多个系统（VMware，virtualBox）。

系统分类：

单机系统：window,xp..

移动端系统：android,ios....

服务端系统：unix,Linux.....

Linux系统的作用：

1.通过linux操作服务器，一般在系统运维时使用；

2.Linux开发以及日常办公；

3.系统开发，基于linux原生系统完成发行版系统的研发

特点：免费的，开源的多用户的操作系统，shell脚本。

发行版linux系统：CentOS,Redhat(红帽)，ubuntu





## linux系统目录结构



bin是Binary的缩写, 这个目录存放着最经常使用的命令。

/boot：

这里存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件。

/dev ：

dev是Device(设备)的缩写, 该目录下存放的是Linux的外部设备，在Linux中访问设备的方式和访问文件的方式是相同的。

/etc：

这个目录用来存放所有的系统管理所需要的配置文件和子目录。

/home：

用户的主目录，在Linux中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。 

/lib：

这个目录里存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。

/lost+found：

这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

/media：

linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

/mnt：

系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了。

/opt：

 这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

/proc：

这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。



## linux基本操作

Ps aux|grep java查看服务器上有几个java进程

Kill -9 端口号:结束进程

 

ll:查询当前目录下的所有文件和文件夹  列

ls:查询当前目录下的所有文件和文件夹  排

Pwd:查看自己在哪个目录下面

Mkdir 名字:创建文件夹

\> 名字:创建文件

Mv 文件路径 文件夹路径:经文件移动到指定文件夹下面

Mv  文件名 文件名:修改文件名字

Clear:清楚当前页面

Echo “信息” >文件名:在文件中添加一段数据

Echo “信息”>文件路径:在指定路径下的文件添加一段数据

Cat 文件名:查询文件中的信息

Cd .. :返回上一级目录

Cd 目录路径:到指定目录下面

Rm -rf 文件名或*:删除文件夹(*所有)  强制删除

Rm -r  文件名或*:删除文件(*所有)  确定后删除

Cp 文件路径 文件夹路径:复制文件到指定文件夹中

Find 文件大致路径 -name 文件名:指定文件的路径

Find 文件大致路径 -name 文件*:查询文件的路径(模糊查询)

Ln 文件路径 名字:创建连接

Top:系统进程的使用情况

Df:硬盘的使用情况

Du:当前目录下文件占用内存的大小

Netstat -anu:协议状态

Netstat -ant:协议状态

Gzip  ./*:压缩文件(分开压缩并删除原文件)

Gzip -d ./*:解压文件(解压并删除压缩包)

tar -czvf 压缩包名字.tar ./*:压缩当前目录下所有文件

tar -xzvf 文件名字 -C(大写) 解压路径:解压所有文件  

 

## linux常用命令操作



# 文件和目录

### pwd ：显示当前所在的目录

/  linux中根目录

~ 当前的用户目录

 

### cd  切换目录 (. , ..)

cd /home 进入 '/ home' 目录' 

cd .. 返回上一级目录 

cd ../.. 返回上两级目录 

cd 进入当前登录用户的主目录 

cd ~ user1 进入个人的主目录 

cd - 返回上次所在的目录 

 

### ls 查看目录下的文件

ls -F 查看目录中的文件 

ls -l 显示文件和目录的详细资料(常用的查看指令) =ll

ls -a 显示隐藏文件

 

### cat查看文件内容：

Cat 指令 查看文件内容的指令

Cat 文件名

Cat 有效的文件路径/文件 查看文件内容

 

### 目录指令

mkdir dir1 创建一个叫做 'dir1' 的目录' 

mkdir dir1 dir2 同时创建两个目录 

mkdir -p /tmp/dir1/dir2 创建一个目录树(多个文件夹的嵌套) 

rmdir dir1 删除一个叫做 'dir1' 的目录'(文件夹中没有内容的) 

 

rm -rf dir1 删除一个叫做 'dir1' 的目录并同时删除其内容 

rm -rf dir1 dir2 同时删除两个目录及它们的内容

m*v dir1* new_dir 重命名

 

### 创建文件的指令

 Touch file1 创建一个叫’file1’的文件

 Vim 文件名 编辑文件内容后就会产生一个文件

 Echo ‘内容’ > 文件名 将制定内容添加到文件中，这样会覆盖文件原有内容，如果不想覆盖请使用 >> 符号

 

###  删除文件

rm -f 文件名(或者是一个有效的文件目录,最终指向一个文件)  删除一个文件

 mv 文件名 新文件名   \\ 给文件重命名

 mv 文件名 一个有效的文件目录。 \\将某个文件移动到指定目录中

 mv 文件的路径  文件的路径  将当前目录中制定的文件移动到目标文件中 

 

### Cp 复制指令

cp dir/* . 复制一个目录下的所有文件到当前工作目录 



cp -a /tmp/dir1 路径 复制一个目录到当前工作目录 



cp -a file1 file2 #连同文件的所有特性把文件file1复制成文件file2



cp file1 file2 file3 dir #把文件file1、file2、file3复制到目录dir中



## vi编辑

:wq保存退出 :q!强制退出不保存 :wq!强制保存退出 :q不保存退出

0:上面插入一行 A:行末插入 I:前一行插入

/关键字查询 n从上往下查  shift  n从下往上查

?关键字查询

:set nu添加行号 :set nonu取消行号

Gg:到第一行

G:文本最后一行

:n 到底n行

n删除光标虽在的字符   nx删除光标所咋位置的第n个字符

Dd删除一行   d+shift+g光标后的字符删除(行)

Shift+d 光标后所有的字符

U 取消上一步操作

Ctrl+r 恢复下一步操作



vim:编辑文本

vim 文件名/文件的有效路径

拷贝一行代码：yyp(一般模式)

命令模式：

:wq

:w

:w！

:q

:q!

:set nu 		显示行数

:set nonu	不显示行数



## 系统命令

Halt:关机

Reboot:重启

\#useradd: 用户名:创建用户

\#more  /ect/passwd: 查看当前系统的用户

\#passwd  用户名: 密码给用户设置密码

\#usermod -l l 新用户名  旧用户名:修改用户名

 

Chmod g+权限 文件名:添加一个权限 

g组 u用户

### 关机

shutdown -h 0 #<==0秒后关机

shutdown -h now #<==现在关机

shutdown -h 10 #<==10分钟后关机

shutdown -h 23:20 #<==23：20分关机

shutdown -c #<==取消shutdown关机命令

init 0 #<==立马关机（切换运行级别为0，推荐使用）

halt #<==立马关机

poweroff #<==立马关机

### 重启

shutdown -r now #<==现在重启

shutdown -r 23:20 & #<==23：20分重启，加&符号代表把该命令转到后台处理

reboot #<==立马重启（推荐使用）

init 6 #<==立马重启（切换运行级别为6，推荐使用）





## 配置jdk

Vi  /etc/profile

JAVA_HOME=jdk安装路径

Export PATH=$JAVA_HOME/bin:$PATH

:wq!

Source /etc/profile

Java -version

如果配置后报错..-bash: /usr/local/java/jdk1.7.0_55/bin/java: /lib/ld-linux.so.2: bad ELF interpreter

运行命令:yum install glibc.i686

根据提示确认安装就行了

 

Nohup ./startup.sh  &(打印到日志):运行脚本

Tail -f ../logs/catalina.out:查看日志



## 查看防火墙

如果上面两部都没有问题，查看防火墙状态：# service iptables status

关闭防火墙：

\#service iptables stop|start|restart 关闭|开启|重启当前防火墙

\#chkconfig iptables off|on 永久关闭|开启防火墙

 

如果iptables提示不能用：centos从7开始默认用的是firewalld，这个是基于iptables的，虽然有iptables的核心，但是iptables的服务是没安装的。所以你只要停止firewalld服务即可：

 

\#sudo systemctl stop firewalld.service && sudo systemctl disable firewalld.service

 

如果你要改用iptables的话，需要安装iptables服务：

sudo yum install iptables-services

sudo systemctl enable iptables && sudo systemctl enable ip6tables

sudo systemctl start iptables && sudo systemctl start ip6tables。



## 安装redis

yum install -y gcc-c++ :安装C语言

进入安装目录make编译一下

redismake install PREFIX=redis-3.0.0

前端启动:

./Bin/redis-server: 启动redis

./redis-cli:进入

后端启动:

Cp redis.config bin/:复制redis.config

./bin/redis-server redis.conf

./redis-cli:进入

 

1.安装依赖插件

yum -y install gcc

yum install tcl

rpm -ivh  jemalloc-3.6.0-1.el6.x86_64.rpm

2.解压redis压缩包

tar -xvf 压缩包名

3.进入解压包的根目录，执行make

4.Redis/src-------> ./redis-server

5.将redis服务端服务在后台运行

在redis.conf中修改 

 1.daemonize yes  136

 2.释放redis的受保护模式 88

 3.释放redis绑定的ip地址 69 

 4.为redis设置密码 507

退出并保存

 

客户端登录命令： ./redis-cli -a 密码 -p 6379 --raw

 

6379:端口号



## 安装activemq

./bin/activemq start 启动

./bin/activemq status 当前状态

./bin/activemq stop  停止

 

8161:端口号



## 安装zookeeper

cp zoo_sample.cfg zoo.cfg:复制

Vi zoo.cfg:编辑修改

dataDir=数据文件夹地址

./zookeeper/zookeeper-3.4.6/bin/zkServer.sh start:启动zooleeper



## 部署dubbo部署中心

1.将war放在tomcat/webapp中

2.启动zookeepr

3.启动tomcat 查看日志

4.查看地址:8080/dubbo

用户名:root

密码:root



## 安装Tomcat

JDK的安装以及配置

查看当前系统是否安装jdk

rpm -qa|grep java

rpm -e  --nodeps  jdk 名称

1.解压jdk安装包

tar -xvf  jdk-8u171-linux-x64.tar.gz  -C 解压文件夹

2.访问：bin---->./java -version

3.配置环境变量

Vim  /etc/profile

JAVA_HOME=/opt/mysoft/jdk1.8.0_171

PATH=$JAVA_HOME/bin:$PATH

export  JAVA_HOME PATH

4.退出编辑以后执行source /etc/profile；使修改后的文件生效。

5.测试，在任意目录输入java -version

Tomcat的安装

1.解压安装包

2.Cd  tomcat里bin ./startup.sh-------started(成功启动)

3.开放linux系统对外8080端口

  Vim /etc/sysconfig/iptables

  

4.关闭tomcat，然后重启防火墙 service iptables restart ------关闭：service iptables stop 

5.启动tomcat ./startup.sh

访问：http://192.168.107.129:8080/



## Mysql安装

1、查看是否已经安装了mysql

rpm -qa | grep mysql

若系统本身安装了mysql，将自带的mysql卸载，反之，跳过此步即可。卸载命令如下：

rpm -e --nodeps 软件名

2、安装mysql的依赖库（分别执行以下两行命令）

yum install numactl  #numactl依赖库

yum install perl    #perl依赖库

 

3、上传linux版本的Mysql到服务器，并解压到/usr/local/下的mysql目录。

cd /usr/local  #切换目录

mkdir mysql  #创建mysql目录

tar -xvf mysql-5.7.18-1.el6.x86_64.rpm.tar -C /usr/local/mysql  #解压

 

4、在/usr/local/mysql下安装mysql

(1)安装mysql公共类库包：

rpm -ivh mysql-community-common-5.7.18-1.el6.x86_64.rpm 

(2)安装mysql依赖库：   

rpm -ivh mysql-community-libs-5.7.18-1.el6.x86_64.rpm

(3)安装客户端：

rpm -ivh mysql-community-client-5.7.18-1.el6.x86_64.rpm 

(4)安装服务器端：

rpm -ivhmysql-community-server-5.7.18-1.el6.x86_64.rpm

 

5、初始化数据库，执行命令后会在/var/log/mysqld.log生成随机密码。

mysqld --initialize

 

 

6、更改mysql数据库目录的所属用户及其所属组

chown mysql:mysql /var/lib/mysql -R

 

7、启动mysql服务

service mysqld start

 

8、将mysql服务添加至系统服务中并设置开机自启动

chkconfig --add mysqld-------chkconfig --add 服务名

chkconfig mysqld on----------chkconfig 服务名 on

 

9、登录mysql

mysql–uroot -p  #回车，输入第7步生成的随机密码(直接复制过来)

 

10、修改mysql的密码（注意：登陆后必须先修改密码，否则执行不了任何指令）

set password = password('123');

 

11、开启mysql的远程登录权限

grant all privileges on *.* to 'root' @'%' identified by '123'; #123是第上一步的新密码

flush privileges;  #刷新权限

exit;   #退出mysql

 

12、开放Linux的对外访问的端口3306

注意：这两组命令操作是在linux命令行下，所以上一步需退出mysql，命令是exit。

/sbin/iptables -I INPUT -p tcp --dport 3306 -j ACCEPT  #开放3306端口

service iptables restart  #将修改永久保存到防火墙中

service iptables stop      #关闭防火墙





## Nginx安装与配置

(1)：使用yum安装nginx，安装nginx库

rpm -af|grep nginx： 检索当前环境是否安装了nginx
rpm -e --nodeps  卸载内容
rpm -Uvh http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm//更新当前linux系统中安装nginx的环境
 (2)：使用下面命令安装nginx

yum -y install nginx（在线安装）
rpm -ivh nginx文件名（离线安装）
 (3)：启动nginx
service nginx start   #centos6
 (4)：防火墙允许通过80端口
复制代码
vim /etc/sysconfig/iptables

Generated by iptables-save v1.4.7 on Thu Dec 28 19:47:19 2017

*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [21691:949300]
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p icmp -j ACCEPT
-A INPUT -i lo -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A FORWARD -j REJECT --reject-with icmp-host-prohibited
COMMIT
复制代码
 (5)：重启防火墙
service iptables stop

8、访问测试
http://IP



### 修改配置文件

/etc/nginx/conf.d/

upstream www {
        server 10.10.22.161:8081; 
        server 10.10.22.161:8082;
}

location /() {
     proxy_pass http://www;

 }
location ~ \.(html|css|js|gif|jpg|jpeg|png|bmp|swf)$ {
          root /usr/share/nginx/html;

 }

location ~ \.(jsp|do|action)$ {
        proxy_pass http://www;
}

