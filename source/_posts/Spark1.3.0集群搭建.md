---
title: Spark1.3.0集群搭建
date: 2019-09-13 17:00:00
tags: Spark
categories: Spark
---
Spark1.3.0集群搭建

## CentOS 6.5集群搭建

### VirtualBox安装

使用Virtual Box安装包，一步一步安装即可。Oracle_VM_VirtualBox_Extension_Pack-4.1.40-101594.vbox-extpack。之所以选用Virtual Box是因为它比VMWare更加稳定。使用VMWare运行hadoop集群或者spark集群时，有时会出现休眠后重启时，某些进程莫名挂掉的问题。而Virtual Box没有这种情况。之所以选择Virtual Box 4.1版本，是因为更高的版本就不兼容win7了。
由于用的是Mac，所以自己安装VirturlBox。

[Virtual Box 官网](https://www.virtualbox.org/)

### CentOS 6.5安装
  
使用CentOS 6.5镜像即可，CentOS-6.5-i386-minimal.iso。
创建虚拟机：打开Virtual Box，点击“新建”按钮，点击“下一步”，输入虚拟机名称为spark1，选择操作系统为Linux，选择版本为Red Hat，分配1024MB内存，后面的选项全部用默认，在Virtual Disk File location and size中，一定要自己选择一个目录来存放虚拟机文件，最后点击“create”按钮，开始创建虚拟机。
设置虚拟机网卡：选择创建好的spark1虚拟机，点击“设置”按钮，在网络一栏中，连接方式中，选择“Bridged Adapter”。
安装虚拟机中的CentOS 6.5操作系统：选择创建好的虚拟机spark1，点击“开始”按钮，选择安装介质（即本地的CentOS 6.5镜像文件），选择第一项开始安装-Skip-欢迎界面Next-选择默认语言-Baisc Storage Devices-Yes, discard any data-主机名:spark1-选择时区-设置初始密码为hadoop-Replace Existing Linux System-Write changes to disk-CentOS 6.5自己开始安装。
安装完以后，CentOS会提醒你要重启一下，就是reboot，你就reboot就可以了。

### CentOS 6.5网络配置
  
先临时性设置虚拟机ip地址：ifconfig eth0 192.168.1.107，在/etc/hosts文件中配置本地ip（192.168.1.107）到host（spark1）的映射。
配置windows主机上的hosts文件：C:\Windows\System32\drivers\etc\hosts，192.168.1.107 spark1。
使用SecureCRT从windows上连接虚拟机，自己可以上网下一个SecureCRT的绿色版，网上很多。
永久性配置CentOS网络。
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
TYPE=Ethernet
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.1.107
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
```

重启网卡 service network restart。
即使更换了ip地址，重启网卡，可能还是联不通网。那么可以先将IPADDR、NETMASK、GATEWAY给删除，将BOOTPROTO改成dhcp。然后用service network restart重启网卡。此时linux会自动给分配一个ip地址，用ifconfig查看分配的ip地址。然后再次按照之前说的，配置网卡，将ip改成自动分配的ip地址。最后再重启一次网卡。

由于用的是Mac，所以自己安装SecureCRT，自己安装破解版。

[SecureCRT官网](https://www.vandyke.com/products/securecrt/)

### CentOS 6.5防火墙和DNS配置
  
关闭防火墙
```
service iptables stop
chkconfig iptables off
vi /etc/selinux/config
SELINUX=disabled
```
自己在win7的控制面板中，关闭windows的防火墙！

配置dns服务器
```
vi /etc/resolv.conf
nameserver 61.139.2.69
ping www.baidu.com
```

### CentOS 6.5 yum配置

修改repo，使用WinSCP（网上很多，自己下一个），将CentOS6-Base-163.repo上传到CentOS中的/usr/local目录下。
```
cd /etc/yum.repos.d/
rm -rf *
```
将自己的repo文件移动到/etc/yum.repos.d/目录中：cp /usr/local/CentOS6-Base-163.repo .，修改repo文件，把所有gpgcheck属性修改为0。

配置yum
```
yum clean all
yum makecache
yum install telnet
```

### JDK 1.7安装

将jdk-7u60-linux-i586.rpm通过WinSCP上传到虚拟机中
安装JDK：rpm -ivh jdk-7u65-linux-i586.rpm
配置jdk相关的环境变量
```
vi .bashrc
export JAVA_HOME=/usr/java/latest
export PATH=$PATH:$JAVA_HOME/bin
source .bashrc
```
测试jdk安装是否成功：
```
java -version
rm -f /etc/udev/rules.d/70-persistent-net.rules
```
由于用的是Mac，所以自己安装FileZilla，没有安装WinSCP。

### 安装第二台和第三台虚拟机

安装上述步骤，再安装两台一模一样环境的虚拟机，因为后面hadoop和spark都是要搭建集群的。
集群的最小环境就是三台。因为后面要搭建ZooKeeper、kafka等集群。
另外两台机器的hostname分别设置为spark2和spark3即可，ip分别为192.168.1.108和192.168.1.109
在安装的时候，另外两台虚拟机的centos镜像文件必须重新拷贝一份，放在新的目录里，使用各自自己的镜像文件。
虚拟机的硬盘文件也必须重新选择一个新的目录，以更好的区分。
安装好之后，记得要在三台机器的/etc/hosts文件中，配置全三台机器的ip地址到hostname的映射，而不能只配置本机，这个很重要！
在windows的hosts文件中也要配置全三台机器的ip地址到hostname的映射。

### 配置集群ssh免密码登录
  
首先在三台机器上配置对本机的ssh免密码登录。生成本机的公钥，过程中不断敲回车即可，ssh-keygen命令默认会将公钥放在/root/.ssh目录下。ssh-keygen -t rsa。将公钥复制为authorized_keys文件，此时使用ssh连接本机就不需要输入密码了。
```
cd /root/.ssh
cp id_rsa.pub authorized_keys
```

接着配置三台机器互相之间的ssh免密码登录。使用ssh-copy-id -i spark命令将本机的公钥拷贝到指定机器的authorized_keys文件中（方便好用）。

## Hadoop 2.4.1集群搭建

### 安装hadoop包

使用hadoop-2.4.1.tar.gz，使用WinSCP上传到CentOS的/usr/local目录下
将hadoop包进行解压缩：tar -zxvf hadoop-2.4.1.tar.gz
对hadoop目录进行重命名：mv hadoop-2.4.1 hadoop
配置hadoop相关环境变量
```
vi .bashrc
export HADOOP_HOME=/usr/local/hadoop
export PATH=$HADOOP_HOME/bin:$HADOOP_HOME/sbin
source .bashrc
```

由于用的是Mac，所以自己使用FileZilla，不是WinSCP。

### 修改core-site.xml
```
<property>
  <name>fs.default.name</name>
  <value>hdfs://spark1:9000</value>
</property>
```
### 修改hdfs-site.xml
```
<property>
  <name>dfs.name.dir</name>
  <value>/usr/local/data/namenode</value>
</property>
<property>
  <name>dfs.data.dir</name>
  <value>/usr/local/data/datanode</value>
</property>
<property>
  <name>dfs.tmp.dir</name>
  <value>/usr/local/data/tmp</value>
</property>
<property>
  <name>dfs.replication</name>
  <value>3</value>
</property>
修改mapred-site.xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
修改yarn-site.xml
<property>
  <name>yarn.resourcemanager.hostname</name>
  <value>spark1</value>
</property>
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
```
### 修改slaves文件
```
spark1
spark2
spark3
```
### 在另外两台机器上搭建hadoop
使用如上配置在另外两台机器上搭建hadoop，可以使用scp命令将spark1上面的hadoop安装包和.bashrc配置文件都拷贝过去。
```
scp -r /usr/local/hadoop root@spark2:/usr/local/
scp -r /usr/local/hadoop root@spark3:/usr/local/
scp .bashrc root@spark2:~/
scp .bashrc root@spark3:~/
```
要记得对.bashrc文件进行source，以让它生效。
记得在spark2和spark3的/usr/local目录下创建data目录。

### 启动hdfs集群

格式化namenode：在spark1上执行以下命令，```hdfs namenode -format```
启动hdfs集群：```start-dfs.sh```
 验证启动是否成功：jps、访问spark:50070 (关闭CentOS 6.5 防火墙)。spark1：namenode、datanode、secondarynamenode。spark2：datanode。spark3：datanode。

### 启动yarn集群
启动yarn集群：```start-yarn.sh```
 验证启动是否成功：jps、访问spark:8088(关闭CentOS 6.5 防火墙)。spark1：resourcemanager、nodemanager。spark2：nodemanager。spark3：nodemanager。

### Hive 0.13搭建
安装hive包
将课程提供的apache-hive-0.13.1-bin.tar.gz使用WinSCP上传到spark1的/usr/local目录下。
解压缩hive安装包：tar -zxvf apache-hive-0.13.1-bin.tar.gz。
重命名hive目录：mv apache-hive-0.13.1-bin hive。
配置hive相关的环境变量。
```
vi .bashrc
export HIVE_HOME=/usr/local/hive
export PATH=$HIVE_HOME/bin
source .bashrc
```
### 安装mysql
在spark1上安装mysql
使用yum安装mysql server
```
yum install -y mysql-server
service mysqld start
chkconfig mysqld on
```
使用yum安装mysql connector
```
yum install -y mysql-connector-java
```
将mysql connector拷贝到hive的lib包中
```
cp /usr/share/java/mysql-connector-java-5.1.17.jar /usr/local/hive/lib
```
在mysql上创建hive元数据库，并对hive进行授权
```
create database if not exists hive_metadata;
grant all privileges on hive_metadata.* to 'hive'@'%' identified by 'hive';
grant all privileges on hive_metadata.* to 'hive'@'localhost' identified by 'hive';
grant all privileges on hive_metadata.* to 'hive'@'spark1' identified by 'hive';
flush privileges;
use hive_metadata;
```
### 配置hive-site.xml
```
mv hive-default.xml.template hive-site.xml
vi hive-site.xml
<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:mysql://spark1:3306/hive_metadata?createDatabaseIfNotExist=true</value>
</property>
<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>com.mysql.jdbc.Driver</value>
</property>
<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>hive</value>
</property>
<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>hive</value>
</property>
<property>
  <name>hive.metastore.warehouse.dir</name>
  <value>/user/hive/warehouse</value>
</property>
```
### 配置hive-env.sh和hive-config.sh
```
mv hive-env.sh.template hive-env.sh
vi /usr/local/hive/bin/hive-config.sh
export JAVA_HOME=/usr/java/latest
export HIVE_HOME=/usr/local/hive
export HADOOP_HOME=/usr/local/hadoop
```
### 验证hive是否安装成功

直接输入hive命令，可以进入hive命令行

## ZooKeeper 3.4.5集群搭建

### 安装ZooKeeper包
将zookeeper-3.4.5.tar.gz使用WinSCP拷贝到spark1的/usr/local目录下。
对zookeeper-3.4.5.tar.gz进行解压缩：tar -zxvf zookeeper-3.4.5.tar.gz。
对zookeeper目录进行重命名：mv zookeeper-3.4.5 zk。
配置zookeeper相关的环境变量。
```
vi .bashrc
export ZOOKEEPER_HOME=/usr/local/zk
export PATH=$ZOOKEEPER_HOME/bin
source .bashrc
```
### 配置zoo.cfg
```
cd zk/conf
mv zoo_sample.cfg zoo.cfg
vi zoo.cfg
dataDir=/usr/local/zk/data
server.0=spark1:2888:3888	
server.1=spark2:2888:3888
server.2=spark3:2888:3888
```
### 设置zk节点标识
```
cd zk
mkdir data
cd data
vi myid
0
```
### 搭建zk集群
在另外两个节点上按照上述步骤配置ZooKeeper，使用scp将zk和.bashrc拷贝到spark2和spark3上即可。
```
scp -r /usr/local/zk root@spark2:/usr/local/
scp -r /usr/local/zk root@spark3:/usr/local/
scp ~/.bashrc root@spark2:~/
scp ~/.bashrc root@spark3:~/
source ~/.bashrc
```
唯一的区别是spark2和spark3的标识号分别设置为1和2。

### 启动ZooKeeper集群
分别在三台机器上执行：zkServer.sh start。
检查ZooKeeper状态：zkServer.sh status。

## kafka_2.9.2-0.8.1集群搭建

### 安装scala 2.11.4
  
将课程提供的scala-2.11.4.tgz使用WinSCP拷贝到spark1的/usr/local目录下。
对scala-2.11.4.tgz进行解压缩：tar -zxvf scala-2.11.4.tgz。
对scala目录进行重命名：mv scala-2.11.4 scala。

### 配置scala相关的环境变量。
```
vi .bashrc
export SCALA_HOME=/usr/local/scala
export PATH=$SCALA_HOME/bin
source .bashrc
```
查看scala是否安装成功：scala -version
按照上述步骤在spark2和spark3机器上都安装好scala。使用scp将scala和.bashrc拷贝到spark2和spark3上即可。
```
scp -r /usr/local/scala root@spark2:/usr/local/
scp -r /usr/local/scala root@spark3:/usr/local/
scp ~/.bashrc root@spark2:~/
scp ~/.bashrc root@spark3:~/
source ~/.bashrc
```
### 安装Kafka包
将课程提供的kafka_2.9.2-0.8.1.tgz使用WinSCP拷贝到spark1的/usr/local目录下。
对kafka_2.9.2-0.8.1.tgz进行解压缩：tar -zxvf kafka_2.9.2-0.8.1.tgz。
对kafka目录进行改名：mv kafka_2.9.2-0.8.1 kafka。

### 配置kafka。
```
vi /usr/local/kafka/config/server.properties
```
broker.id：依次增长的整数，0、1、2、3、4，集群中Broker的唯一id
zookeeper.connect=192.168.1.107:2181,192.168.1.108:2181,192.168.1.109:2181
安装slf4j。
将课程提供的slf4j-1.7.6.zip上传到/usr/local目录下。unzip slf4j-1.7.6.zip。
把slf4j中的slf4j-nop-1.7.6.jar复制到kafka的libs目录下面。

### 搭建kafka集群
按照上述步骤在spark2和spark3分别安装kafka。用scp把kafka拷贝到spark2和spark3行即可。
```
scp -r /usr/local/kafka root@spark2:/usr/local/
scp -r /usr/local/kafka root@spark3:/usr/local/
```
唯一区别的，就是server.properties中的broker.id，要设置为1和2。

### 启动kafka集群
在三台机器上分别执行以下命令：nohup bin/kafka-server-start.sh config/server.properties &。

解决kafka Unrecognized VM option ‘UseCompressedOops’问题。
```
vi bin/kafka-run-class.sh 
if [ -z "$KAFKA_JVM_PERFORMANCE_OPTS" ]; then
  KAFKA_JVM_PERFORMANCE_OPTS="-server  -XX:+UseCompressedOops -XX:+UseParNewGC -XX:+UseConcMarkSweepGC -XX:+CMSClassUnloadingEnabled -XX:+CMSScavengeBeforeRemark -XX:+DisableExplicitGC -Djava.awt.headless=true"
fi
```
去掉-XX:+UseCompressedOops即可
使用jps检查启动是否成功。

### 测试kafka集群
使用基本命令检查kafka是否搭建成功。
```
bin/kafka-topics.sh --zookeeper spark1:2181,spark2:2181,spark3:2181 --topic Test --replication-factor 1 --partitions 1 --create

bin/kafka-console-producer.sh --broker-list spark1:9092,spark2:9092,spark3:9092 --topic Test

bin/kafka-console-consumer.sh --zookeeper spark1:2181,spark2:2181,spark3:2181 --topic Test --from-beginning
```

## Spark 1.3.0集群搭建

## 安装spark包
将spark-1.3.0-bin-hadoop2.4.tgz使用WinSCP上传到/usr/local目录下。
解压缩spark包：tar zxvf spark-1.3.0-bin-hadoop2.4.tgz。
更改spark目录名：mv spark-1.3.0-bin-hadoop2.4 spark。
设置spark环境变量。
```
vi .bashrc
export SPARK_HOME=/usr/local/spark
export PATH=$SPARK_HOME/bin
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
source .bashrc
```
### 修改spark-env.sh文件
```
cd /usr/local/spark/conf
cp spark-env.sh.template spark-env.sh
vi spark-env.sh
export JAVA_HOME=/usr/java/latest
export SCALA_HOME=/usr/local/scala
export SPARK_MASTER_IP=192.168.1.107
export SPARK_WORKER_MEMORY=1g
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
```
### 修改slaves文件
```
cp slaves.template slaves
vi slaves
spark2
spark3
```
### 安装spark集群
在另外两个节点进行一模一样的配置，使用scp将spark和.bashrc拷贝到spark2和spark3即可。
```
scp -r /usr/local/spark root@spark2:/usr/local/
scp -r /usr/local/spark root@spark3:/usr/local/
scp ~/.bashrc root@spark2:~/
scp ~/.bashrc root@spark3:~/
source ~/.bashrc
```
### 启动spark集群
在spark目录下的sbin目录
执行，/start-all.sh
使用jsp和8080端口可以检查集群是否启动成功
进入spark-shell查看是否正常

## WordCount

### Java开发wordcount程序

### 安装eclipse
[eclipse download](https://www.eclipse.org/downloads/)

### 配置maven

打开eclipse->新建Maven Project->maven-archetype-quickstart->groupId:cn.spark->artifactId:spark-study-java->package:cn.spark.study->替换pom.xml-quick fix(第一次要等待很长时间)->Buid Baph->Configure Build Path->Libaries->JRE System Libarary->Edit->Workspace default JRE。
```
//: pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>cn.spark</groupId>
  <artifactId>spark-study-java</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>spark-study-java</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
	  <groupId>org.apache.spark</groupId>
	  <artifactId>spark-core_2.10</artifactId>
	  <version>1.3.0</version>
	</dependency>
	<dependency>
	  <groupId>org.apache.spark</groupId>
	  <artifactId>spark-sql_2.10</artifactId>
	  <version>1.3.0</version>
	  </dependency>
	<dependency>
	  <groupId>org.apache.spark</groupId>
	  <artifactId>spark-hive_2.10</artifactId>
	  <version>1.3.0</version>
	</dependency>
	<dependency>
	  <groupId>org.apache.spark</groupId>
	  <artifactId>spark-streaming_2.10</artifactId>
	  <version>1.3.0</version>
	</dependency>
	<dependency>
	  <groupId>org.apache.hadoop</groupId>
	  <artifactId>hadoop-client</artifactId>
	  <version>2.4.1</version>
	</dependency>
	<dependency>
	  <groupId>org.apache.spark</groupId>
	  <artifactId>spark-streaming-kafka_2.10</artifactId>
	  <version>1.3.0</version>
	</dependency>
  </dependencies>
  
  <build>
    <sourceDirectory>src/main/java</sourceDirectory>
    <testSourceDirectory>src/main/test</testSourceDirectory>
	
    <plugins>
      <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
          <archive>
            <manifest>
              <mainClass></mainClass>
            </manifest>
          </archive>
        </configuration>
        <executions>
          <execution>
            <id>make-assembly</id>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
      </plugin>

      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>exec-maven-plugin</artifactId>
        <version>1.2.1</version>
        <executions>
          <execution>
            <goals>
              <goal>exec</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <executable>java</executable>
          <includeProjectDependencies>true</includeProjectDependencies>
          <includePluginDependencies>false</includePluginDependencies>
          <classpathScope>compile</classpathScope>
          <mainClass>cn.spark.study.App</mainClass>
        </configuration>
      </plugin>

      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>1.6</source>
          <target>1.6</target>
        </configuration>
      </plugin>

    </plugins>
  </build>
</project>
```

### 新建spark.txt，新建包cn.spark.study.core，新建WordCountLocal.java
```
//: spark.txt
Spark is new technology that sits on top of Hadoop Distributed File System (HDFS) that is characterized as ¡°a fast and general engine for large-scale data processing.¡± Spark has three key features that make it the most interesting up and coming technology to rock the big data world since Apache Hadoop in 2005.

1. For iterative analysis like logistic regression, Random Forests, or other advanced algorithms, Spark has demonstrated 100X increase in speed that scales to hundreds of millions of rows.

2. Spark has native support for the latest and greatest programming languages Java, Scala, and of course Python.

3. Spark has generality or platform compatibility in both directions meaning it integrates nicely with SQL engines (Shark), Machine Learning (MLlib), and streaming (Spark Streaming) without requiring new software installed on the cluster using Hadoop¡¯s new YARN cluster manager.

At Alpine, we have made it dead simple to get started with Spark by including the technology in our latest build out of the box. We require no additional software or hardware to leverage our extensive list of operators for data transformation, exploration, and building advanced analytic models. We leverage Hadoop Yarn (Hadoop NextGen) to launch Spark job without any pre-installation of Spark or modification of cluster configuration. This empowers our customers to have seamless integration of our Spark implementation and their Hadoop stack. For example, we have analyzed 50 Million rows of account data in 50 seconds on a 20 node cluster recently at last month GigaOM conference.

As a Spark certified company, Alpine Data Labs will be at the Summit. We¡¯d love to see you there!

Want to meet with us?  Click here to set up an appointment at your convenience. Or just send a tweet to our Product & Marketing Director Joel Horwitz @JSHorwitz.
```

```
//: WordCountLocal.java
package cn.spark.study.core;

import java.util.Arrays;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.api.java.function.FlatMapFunction;
import org.apache.spark.api.java.function.Function2;
import org.apache.spark.api.java.function.PairFunction;
import org.apache.spark.api.java.function.VoidFunction;

import scala.Tuple2;

/**
 * 使用java开发本地测试wordcount程序
 * @author xdx
 *
 */
public class WordCountLocal {
	public static void main(String[] args) {
		// 第一步：创建SparkConf对象，设置Spark应用的配置信息
		// 使用setMaster()可以设置Spark应用程序要连接的Spark集群的master节点的url
		// 但是如果设置为local则代表，在本地运行
		SparkConf conf = new SparkConf()
				.setAppName("WordCountLocal")
				.setMaster("local");
		
		// 第二步：创建JavaSparkContext对象
		// 在Spark中，SparkContext是Spark所有功能的一个入口，你无论是用java、scala，甚至是python编写
			// 都必须要有一个SparkContext，它的主要作用，包括初始化Spark应用程序所需的一些核心组件，包括
			// 调度器（DAGSchedule、TaskScheduler），还会去到Spark Master节点上进行注册，等等
		// 一句话，SparkContext，是Spark应用中，可以说是最最重要的一个对象
		// 但是呢，在Spark中，编写不同类型的Spark应用程序，使用的SparkContext是不同的，如果使用scala，
			// 使用的就是原生的SparkContext对象
			// 但是如果使用Java，那么就是JavaSparkContext对象
			// 如果是开发Spark SQL程序，那么就是SQLContext、HiveContext
			// 如果是开发Spark Streaming程序，那么就是它独有的SparkContext
			// 以此类推
		JavaSparkContext sc = new JavaSparkContext(conf);
		
		// 第三步：要针对输入源（hdfs文件、本地文件，等等），创建一个初始的RDD
		// 输入源中的数据会打散，分配到RDD的每个partition中，从而形成一个初始的分布式的数据集
		// 我们这里呢，因为是本地测试，所以呢，就是针对本地文件
		// SparkContext中，用于根据文件类型的输入源创建RDD的方法，叫做textFile()方法
		// 在Java中，创建的普通RDD，都叫做JavaRDD
		// 在这里呢，RDD中，有元素这种概念，如果是hdfs或者本地文件呢，创建的RDD，每一个元素就相当于
		// 是文件里的一行
		JavaRDD<String> lines = sc.textFile("spark.txt");
		
		// 第四步：对初始RDD进行transformation操作，也就是一些计算操作
		// 通常操作会通过创建function，并配合RDD的map、flatMap等算子来执行
		// function，通常，如果比较简单，则创建指定Function的匿名内部类
		// 但是如果function比较复杂，则会单独创建一个类，作为实现这个function接口的类
		
		// 先将每一行拆分成单个的单词
		// FlatMapFunction，有两个泛型参数，分别代表了输入和输出类型
		// 我们这里呢，输入肯定是String，因为是一行一行的文本，输出，其实也是String，因为是每一行的文本
		// 这里先简要介绍flatMap算子的作用，其实就是，将RDD的一个元素，给拆分成一个或多个元素
		JavaRDD<String> words = lines.flatMap(new FlatMapFunction<String, String>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public Iterable<String> call(String line) throws Exception {
				return Arrays.asList(line.split(" "));
			}
		});
		
		// 接着，需要将每一个单词，映射为(单词, 1)的这种格式
			// 因为只有这样，后面才能根据单词作为key，来进行每个单词的出现次数的累加
		// mapToPair，其实就是将每个元素，映射为一个(v1,v2)这样的Tuple2类型的元素
			// 如果大家还记得scala里面讲的tuple，那么没错，这里的tuple2就是scala类型，包含了两个值
		// mapToPair这个算子，要求的是与PairFunction配合使用，第一个泛型参数代表了输入类型
			// 第二个和第三个泛型参数，代表的输出的Tuple2的第一个值和第二个值的类型
		// JavaPairRDD的两个泛型参数，分别代表了tuple元素的第一个值和第二个值的类型
		JavaPairRDD<String, Integer> pairs = words.mapToPair(new PairFunction<String, String, Integer>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public Tuple2<String, Integer> call(String word) throws Exception {
				return new Tuple2<String, Integer>(word, 1);
			}
		});
		
		// 接着，需要以单词作为key，统计每个单词出现的次数
		// 这里要使用reduceByKey这个算子，对每个key对应的value，都进行reduce操作
		// 比如JavaPairRDD中有几个元素，分别为(hello, 1) (hello, 1) (hello, 1) (world, 1)
		// reduce操作，相当于是把第一个值和第二个值进行计算，然后再将结果与第三个值进行计算
		// 比如这里的hello，那么就相当于是，首先是1 + 1 = 2，然后再将2 + 1 = 3
		// 最后返回的JavaPairRDD中的元素，也是tuple，但是第一个值就是每个key，第二个值就是key的value
		// reduce之后的结果，相当于就是每个单词出现的次数
		JavaPairRDD<String, Integer> wordCounts = pairs.reduceByKey(new Function2<Integer, Integer, Integer>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public Integer call(Integer v1, Integer v2) throws Exception {
				return v1 + v2;
			}
		});
		
		// 到这里为止，我们通过几个Spark算子操作，已经统计出了单词的次数
		// 但是，之前我们使用的flatMap、mapToPair、reduceByKey这种操作，都叫做transformation操作
		// 一个Spark应用中，光是有transformation操作，是不行的，是不会执行的，必须要有一种叫做action
		// 接着，最后，可以使用一种叫做action操作的，比如说，foreach，来触发程序的执行
		wordCounts.foreach(new VoidFunction<Tuple2<String, Integer>>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public void call(Tuple2<String, Integer> wordCount) throws Exception {
				System.out.println(wordCount._1 + " appeared " + wordCount._2 + " times.");
			}
		});
		
		sc.close();
	}
}
/* Output:
Hadoop appeared 1 times.
processing. appeared 1 times.
Spark appeared 6 times.
it appeared 2 times.
parallel appeared 1 times.
its appeared 1 times.
operators appeared 1 times.
YARN, appeared 1 times.
engine. appeared 1 times.
Runs appeared 1 times.
standalone appeared 1 times.
optimizer, appeared 1 times.
shells. appeared 1 times.
complex appeared 1 times.
state-of-the-art appeared 1 times.
learning, appeared 1 times.
Use appeared 1 times.
applications appeared 1 times.
over appeared 1 times.
streaming, appeared 1 times.
easy appeared 1 times.
for appeared 3 times.
faster. appeared 1 times.
make appeared 1 times.
engine appeared 1 times.
these appeared 1 times.
performance appeared 1 times.
the appeared 3 times.
application. appeared 1 times.
SQL. appeared 1 times.
DataFrames, appeared 1 times.
Mesos, appeared 2 times.
R, appeared 2 times.
can appeared 4 times.
HDFS, appeared 1 times.
build appeared 1 times.
Cassandra, appeared 1 times.
achieves appeared 1 times.
Apache appeared 6 times.
including appeared 1 times.
large-scale appeared 1 times.
Kubernetes, appeared 1 times.
sources. appeared 2 times.
analytics. appeared 1 times.
libraries appeared 2 times.
Combine appeared 1 times.
query appeared 1 times.
batch appeared 1 times.
It appeared 1 times.
scheduler, appeared 1 times.
both appeared 1 times.
streaming appeared 1 times.
Access appeared 1 times.
machine appeared 1 times.
Everywhere appeared 1 times.
Generality appeared 1 times.
stack appeared 1 times.
And appeared 1 times.
high appeared 1 times.
Speed appeared 1 times.
is appeared 1 times.
80 appeared 1 times.
run appeared 1 times.
seamlessly appeared 1 times.
Kubernetes. appeared 1 times.
Spark™ appeared 1 times.
runs appeared 1 times.
same appeared 1 times.
You appeared 2 times.
on appeared 5 times.
interactively appeared 1 times.
Ease appeared 1 times.
data appeared 4 times.
apps. appeared 1 times.
offers appeared 1 times.
in appeared 4 times.
using appeared 2 times.
DAG appeared 1 times.
Alluxio, appeared 1 times.
diverse appeared 1 times.
100x appeared 1 times.
execution appeared 1 times.
hundreds appeared 1 times.
Python, appeared 2 times.
from appeared 1 times.
other appeared 1 times.
standalone, appeared 1 times.
use appeared 1 times.
physical appeared 1 times.
workloads appeared 1 times.
Run appeared 1 times.
mode, appeared 1 times.
EC2, appeared 1 times.
you appeared 1 times.
that appeared 1 times.
or appeared 2 times.
a appeared 5 times.
data, appeared 1 times.
high-level appeared 1 times.
Java, appeared 1 times.
SQL appeared 2 times.
Hive, appeared 1 times.
Hadoop, appeared 1 times.
to appeared 1 times.
 appeared 9 times.
analytics appeared 1 times.
GraphX, appeared 1 times.
Write appeared 1 times.
of appeared 3 times.
cluster appeared 1 times.
access appeared 1 times.
MLlib appeared 1 times.
quickly appeared 1 times.
Scala, appeared 2 times.
HBase, appeared 1 times.
and appeared 8 times.
unified appeared 1 times.
SQL, appeared 1 times.
combine appeared 1 times.
Streaming. appeared 1 times.
powers appeared 1 times.
cloud. appeared 1 times.
*///:~
```
### spark-submit提交到spark集群进行执行

编写WordCountCluster.java
```
//: WordCountCluster.java
package cn.spark.study.core;

import java.util.Arrays;

import org.apache.spark.SparkConf;
import org.apache.spark.api.java.JavaPairRDD;
import org.apache.spark.api.java.JavaRDD;
import org.apache.spark.api.java.JavaSparkContext;
import org.apache.spark.api.java.function.FlatMapFunction;
import org.apache.spark.api.java.function.Function2;
import org.apache.spark.api.java.function.PairFunction;
import org.apache.spark.api.java.function.VoidFunction;

import scala.Tuple2;

/**
 * 将java开发的wordcount程序部署到spark集群上运行
 * @author xdx
 *
 */
public class WordCountCluster {
	public static void main(String[] args) {
		// 如果要在spark集群上运行，需要修改的，只有两个地方
		// 第一，将SparkConf的setMaster()方法给删掉，默认它自己会去连接
		// 第二，我们针对的不是本地文件了，修改为hadoop hdfs上的真正的存储大数据的文件
		
		// 实际执行步骤：
		// 1、将spark.txt文件上传到hdfs上去
		// 2、使用我们最早在pom.xml里配置的maven插件，对spark工程进行打包
		// 3、将打包后的spark工程jar包，上传到机器上执行
		// 4、编写spark-submit脚本
		// 5、执行spark-submit脚本，提交spark应用到集群执行
		
		SparkConf conf = new SparkConf()
				.setAppName("WordCountCluster");
		
		JavaSparkContext sc = new JavaSparkContext(conf);
		
		JavaRDD<String> lines = sc.textFile("hdfs://spark1:9000/spark.txt");
		
		JavaRDD<String> words = lines.flatMap(new FlatMapFunction<String, String>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public Iterable<String> call(String line) throws Exception {
				return Arrays.asList(line.split(" "));
			}
		});
		
		JavaPairRDD<String, Integer> pairs = words.mapToPair(new PairFunction<String, String, Integer>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public Tuple2<String, Integer> call(String word) throws Exception {
				return new Tuple2<String, Integer>(word, 1);
			}
		});
		
		JavaPairRDD<String, Integer> wordCounts = pairs.reduceByKey(new Function2<Integer, Integer, Integer>() {
			private static final long serialVersionID = 1L;
			
			@Override 
			public Integer call(Integer v1, Integer v2) throws Exception {
				return v1 + v2;
			}
		});
		
		wordCounts.foreach(new VoidFunction<Tuple2<String, Integer>>() {
			private static final long serialVersionID = 1L;
			
			@Override
			public void call(Tuple2<String, Integer> wordCount) throws Exception {
				System.out.println(wordCount._1 + " appeared " + wordCount._2 + " times.");
			}
		});
		
		sc.close();
	}
}
/* Output:
*///:~
```

将spark.txt上传到spark1，将spark.txt上传到hdfs
将spark-study-java打包，Run As->Run Configurations->Maven Build->New->spark-study-java->Run
将/spark-study-java/target/spark-study-java-0.0.1-SNAPSHOT-jar-with-dependencies.jar上传到spark
编写wordcount.sh，使用spark-submit进行执行

```
//: wordcount.sh
/usr/local/spark/bin/spark-submit \
--class cn.spark.sparktest.core.WordCountCluster \
--num-executors 3 \
--driver-memory 100m \
--executor-memory 100m \
--executor-cores 3 \
/root/Workspace/SparkProjects/wordcount/SparkTest-0.0.1-SNAPSHOT-jar-with-dependencies.jar \
```

### Scala开发wordcount程序
scala ide for eclipse download
在Java Build Path中，添加spark依赖包(spark-assembly-1.3.0-hadoop2.4.0.jar)，如果与scala ide for eclipse原生的scala版本发生冲突，则移除原生的scala，重新配置scala compiler
```
//: WordCount.scala
package cn.spark.study.core

import org.apache.spark.SparkConf
import org.apache.spark.SparkContext

/**
 * @author xdx
 */
object WordCount {
  def main(args: Array[String]) {
    val conf = new SparkConf()
      .setAppName("WordCount")
      .setMaster("local")
    val sc = new SparkContext(conf)
    val lines = sc.textFile("spark.txt", 1)
    val words = lines.flatMap { line => line.split(" ") }
    val pairs = words.map { word => (word, 1) }
    val wordCounts = pairs.reduceByKey { _ + _ }
    wordCounts.foreach(wordCount => println(wordCount._1 + " appeared " + wordCount._2 + " times."))
  }
}
/* Output:
(MLlib), appeared 1 times.
For appeared 2 times.
Product appeared 1 times.
it appeared 3 times.
operators appeared 1 times.
sits appeared 1 times.
Hadoop��s appeared 1 times.
have appeared 3 times.
tweet appeared 1 times.
stack. appeared 1 times.
modification appeared 1 times.
conference. appeared 1 times.
we appeared 2 times.
requiring appeared 1 times.
This appeared 1 times.
simple appeared 1 times.
manager. appeared 1 times.
software appeared 2 times.
any appeared 1 times.
make appeared 1 times.
implementation appeared 1 times.
seconds appeared 1 times.
& appeared 1 times.
out appeared 1 times.
Data appeared 1 times.
engine appeared 1 times.
directions appeared 1 times.
month appeared 1 times.
the appeared 7 times.
technology appeared 3 times.
2. appeared 1 times.
Alpine, appeared 1 times.
We��d appeared 1 times.
box. appeared 1 times.
100X appeared 1 times.
most appeared 1 times.
build appeared 1 times.
love appeared 1 times.
be appeared 1 times.
��a appeared 1 times.
Apache appeared 1 times.
At appeared 1 times.
Alpine appeared 1 times.
our appeared 5 times.
including appeared 1 times.
as appeared 1 times.
us? appeared 1 times.
dead appeared 1 times.
iterative appeared 1 times.
leverage appeared 2 times.
Want appeared 1 times.
File appeared 1 times.
programming appeared 1 times.
account appeared 1 times.
recently appeared 1 times.
engines appeared 1 times.
is appeared 2 times.
Horwitz appeared 1 times.
on appeared 3 times.
features appeared 1 times.
pre-installation appeared 1 times.
speed appeared 1 times.
at appeared 3 times.
using appeared 1 times.
convenience. appeared 1 times.
top appeared 1 times.
integrates appeared 1 times.
meaning appeared 1 times.
customers appeared 1 times.
new appeared 3 times.
We appeared 2 times.
Python. appeared 1 times.
Random appeared 1 times.
launch appeared 1 times.
processing.�� appeared 1 times.
set appeared 1 times.
has appeared 4 times.
NextGen) appeared 1 times.
world appeared 1 times.
Learning appeared 1 times.
seamless appeared 1 times.
Director appeared 1 times.
generality appeared 1 times.
or appeared 4 times.
Yarn appeared 1 times.
Java, appeared 1 times.
appointment appeared 1 times.
As appeared 1 times.
YARN appeared 1 times.
Machine appeared 1 times.
company, appeared 1 times.
installed appeared 1 times.
50 appeared 2 times.
see appeared 1 times.
of appeared 10 times.
cluster appeared 4 times.
three appeared 1 times.
analytic appeared 1 times.
Or appeared 1 times.
Forests, appeared 1 times.
rows appeared 1 times.
millions appeared 1 times.
rows. appeared 1 times.
Hadoop appeared 4 times.
characterized appeared 1 times.
Spark appeared 10 times.
integration appeared 1 times.
job appeared 1 times.
native appeared 1 times.
greatest appeared 1 times.
general appeared 1 times.
Million appeared 1 times.
extensive appeared 1 times.
here appeared 1 times.
big appeared 1 times.
Joel appeared 1 times.
1. appeared 1 times.
send appeared 1 times.
(HDFS) appeared 1 times.
3. appeared 1 times.
without appeared 2 times.
for appeared 3 times.
models. appeared 1 times.
require appeared 1 times.
just appeared 1 times.
@JSHorwitz. appeared 1 times.
Labs appeared 1 times.
latest appeared 2 times.
regression, appeared 1 times.
node appeared 1 times.
coming appeared 1 times.
your appeared 1 times.
up appeared 2 times.
analysis appeared 1 times.
20 appeared 1 times.
advanced appeared 2 times.
Distributed appeared 1 times.
no appeared 1 times.
large-scale appeared 1 times.
since appeared 1 times.
started appeared 1 times.
empowers appeared 1 times.
transformation, appeared 1 times.
by appeared 1 times.
like appeared 1 times.
compatibility appeared 1 times.
2005. appeared 1 times.
both appeared 1 times.
an appeared 1 times.
streaming appeared 1 times.
(Shark), appeared 1 times.
analyzed appeared 1 times.
Streaming) appeared 1 times.
made appeared 1 times.
nicely appeared 1 times.
configuration. appeared 1 times.
with appeared 3 times.
algorithms, appeared 1 times.
meet appeared 1 times.
data appeared 4 times.
interesting appeared 1 times.
in appeared 5 times.
logistic appeared 1 times.
GigaOM appeared 1 times.
Summit. appeared 1 times.
increase appeared 1 times.
hundreds appeared 1 times.
support appeared 1 times.
scales appeared 1 times.
Click appeared 1 times.
building appeared 1 times.
other appeared 1 times.
course appeared 1 times.
exploration, appeared 1 times.
rock appeared 1 times.
key appeared 1 times.
you appeared 1 times.
hardware appeared 1 times.
that appeared 4 times.
a appeared 3 times.
fast appeared 1 times.
their appeared 1 times.
example, appeared 1 times.
last appeared 1 times.
SQL appeared 1 times.
demonstrated appeared 1 times.
will appeared 1 times.
to appeared 10 times.
get appeared 1 times.
platform appeared 1 times.
 appeared 7 times.
languages appeared 1 times.
list appeared 1 times.
there! appeared 1 times.
(Spark appeared 1 times.
Scala, appeared 1 times.
and appeared 7 times.
Marketing appeared 1 times.
(Hadoop appeared 1 times.
certified appeared 1 times.
additional appeared 1 times.
System appeared 1 times.
*///:~
```