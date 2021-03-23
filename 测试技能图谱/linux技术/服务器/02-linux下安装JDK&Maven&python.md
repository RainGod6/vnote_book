# 02-linux下安装JDK&Maven


## 安装JDK


```
1、进入java官网下载压缩包至linux服务器
wget https://download.oracle.com/otn/java/jdk/8u271-b09/61ae65e088624f5aaa0b1d2d801acb16/jdk-8u271-linux-x64.tar.gz?AuthParam=1609835869_84f6621ae4b27c6eaa43619659089844

2、拷贝至/usr/local文件夹进行解压
 cp jdk-8u271-linux-x64.tar.gz /usr/local/

3、解压
tar -zxf jdk-8u271-linux-x64.tar.gz 

4、配置环境变量
export JAVA_HOME=/usr/local/jdk1.8.0_271/
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

5、刷新配置生效
 source /etc/profile

测试java命令是否正常
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# java -version
java version "1.8.0_271"
Java(TM) SE Runtime Environment (build 1.8.0_271-b09)
Java HotSpot(TM) 64-Bit Server VM (build 25.271-b09, mixed mode)
```

到此就安装成功了！


## 安装Maven

```
1、进入官网下载
wget https://mirror.bit.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz

2、拷贝至/usr/local文件夹进行解压
cp apache-maven-3.6.3-bin.tar.gz /usr/local/

3、解压
tar -zxf apache-maven-3.6.3-bin.tar.gz

4、配置环境变量
export MAVEN_HOME=/usr/local/apache-maven-3.6.3/
export PATH=$PATH:$MAVEN_HOME/bin

5、刷新配置生效
 source /etc/profile

测试mvn命令是否生效
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# mvn -v
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: /usr/local/apache-maven-3.6.3
Java version: 1.8.0_271, vendor: Oracle Corporation, runtime: /usr/local/jdk1.8.0_271/jre
Default locale: en_US, platform encoding: UTF-8

6、配置阿里镜像源
vim apache-maven-3.6.3/conf/settings.xml
找到<mirrors>标签在标签里面添加阿里云镜像配置
<mirror>
   <id>alimaven</id>
   <name>aliyun maven</name>
   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
   <mirrorOf>central</mirrorOf>    
</mirror>

```


## 安装Python3.7

```
1、官网下载指定版本
wget https://www.python.org/ftp/python/3.7.9/Python-3.7.9.tar.xz

2、解压安装在/usr/local文件下
 cp Python-3.7.9.tar.xz /usr/local/

需要两次解压：
    第一步：xz -d Python-3.7.9.tar.xz 
    第二步： tar -xvf Python-3.7.9.tar 
3、安装gcc编译器
yum -y install gcc

4、3.7版本之后需要一个新的包libffi-devel
yum install libffi-devel -y

还需要安装个依赖包：
yum -y install zlib*


5、进入python文件夹，生成编译脚本(指定安装目录)
 ./configure --prefix=/usr/local/Python-3.7.9/

6、编译
make

7、编译成功后，编译安装：make install
make install

8、建立Python3和pip3的软链:
ln -s /usr/local/Python-3.7.9/bin/python3 /usr/bin/python3
ln -s /usr/local/Python-3.7.9/bin/pip3 /usr/bin/pip3

9、配置环境变量
export PYTHON_HOME=/usr/local/Python-3.7.9/
export PATH=$PATH:$PYTHON_HOME/bin

source /etc/profile

查看是否安装成功：
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# python3 -V
Python 3.7.9
[root@iZ2vcdckpocdm8z7a36gl1Z ~]# pip3 -V
pip 20.1.1 from /usr/local/Python-3.7.9/lib/python3.7/site-packages/pip (python 3.7)
```

到此就安装成功！


接下来我们配置下国内镜像源：
```
1、创建配置文件
mkdir -p ~/.pip
进入目录发现pip.conf自动创建
2、添加内容
[global]
timeout=6000
index-url=http://mirrors.cloud.aliyuncs.com/pypi/simple/

[install]
trusted-host=mirrors.cloud.aliyuncs.com

国内镜像源：
    阿里云 https://mirrors.aliyun.com/pypi/simple/
    中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
    豆瓣(douban) http://pypi.douban.com/simple/
    清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/
    中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```

安装包经常会出现pip版本过低，使用如下命令进行更新：
```
python3 -m pip install --upgrade pip
```