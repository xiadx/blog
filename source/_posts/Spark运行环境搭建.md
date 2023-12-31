---
title: Spark运行环境搭建
date: 2019-09-13 17:25:44
tags: Spark
categories: Spark
---
Spark运行环境搭建

## Mac安装VirtualBox

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Mac安装SecureCRT

* [SecureCRT](https://www.vandyke.com/products/securecrt/)

## VirtualBox安装CentOS7-Minimal

* [CentOS7](http://vault.centos.org/)
* VirtualBox -> 设置 -> 存储 -> 盘片
* VirtualBox -> 设置 -> 网络 -> 桥接网卡

## CentOS7静态IP设置

* 动态IP设置
```
ping www.baidu.com
/* Output:
ping: www.baidu.com: Name or service not known
*///:~
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
/* Input:
ONBOOT=yes
*///:~
service network restart
ping www.baidu.com
/* Output:
PING www.a.shifen.com (182.61.200.7) 56(84) bytes of data.
...
*///:~
```

* 查看DNS
```
vi /etc/resolv.conf
/* Output:
211.100.225.34
124.207.160.106
*///:~
```

* 查看Mac局域网IP
```
ifconfig
/* Output:
...
en0: flags=8863<UP,BROADCAST,SMART,RUNNING,SIMPLEX,MULTICAST> mtu 1500
        ether 88:e9:fe:53:af:ef 
        inet6 fe80::8c0:3e8a:b41a:98b9%en0 prefixlen 64 secured scopeid 0x6 
        inet 192.168.1.101 netmask 0xffffff00 broadcast 192.168.1.255
        nd6 options=201<PERFORMNUD,DAD>
        media: autoselect
        status: active
...
*///:~
```

* 静态IP设置
```
vi /etc/sysconfig/network-scripts/ifcfg-enp0s3
/* Input:
DEVICE=enp0s3
TYPE=Ethernet
ONBOOT=yes
BOOTPROTO=static
IPADDR=192.168.1.108
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=211.100.225.34
DNS2.124.207.160.106
*///:~
service network restart
ping www.baidu.com
/* Output:
PING www.a.shifen.com (182.61.200.7) 56(84) bytes of data.
...
*///:~
```

## VirtualBox生成备份

* VirtualBox -> 生成 -> 备份

## SecureCRT连接VirtualBoxCentOS7

* SecureCRT -> Connect -> Quick Connect -> Hostname(192.168.1.108) -> Username(root) PublicKey -> Properties -> Use identity or certificate file(/Users/xdx/.ssh/id_rsa.pub)
* SecureCRT -> Options -> Session Options -> Appearance -> Solarized Darcula -> Font

## Mac终端文件夹颜色设置

```
vim ~/.bash_profile
/* Input:
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8

export CLICOLOR=1
export LSCOLORS=Exfxcxdxbxegedabagacad
export GREP_OPTIONS="--color=auto"
*///:~
source ~/.bash_profile
```

## VIM安装与配置

* VIM安装
```
yum -y install vim
```

* VIM配置
```
vim ~/.vimrc
/* Input:
syntax on
set tabstop=4
set shiftwidth=4
set softtabstop=4
set smarttab
set expandtab
set encoding=utf-8
*///:~
source ~/.vimrc
```

## CentOS7关闭防火墙和SELinux

* 关闭防火墙
```
systemctl stop firewalld.service
systemctl disable firewalld.service
```

* 关闭SELinux
```
vim /etc/sysconfig/selinux
/* Input:
SELINUX=disabled
*///:~
```

## 安装lrzsz

```
yum -y install lrzsz
```

## 安装JDK

* [downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html)

* getconf LONG_BIT
```
getconf LONG_BIT
/* Output:
64
*///:~
```

* 安装JDK
```
jdk-7u80-linux-x64.tar.gz
rz -bye
mkdir /usr/java
tar -zxvf jdk-7u80-linux-x64.tar.gz -C /usr/java
vim ~/.bash_profile
/* Input:
# JAVA
export JAVA_HOME=/usr/java/jdk1.7.0
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
*///:~
source ~/.bash_profile
java -version
/* Output:
java version "1.7.0_80"
Java(TM) SE Runtime Environment (build 1.7.0_80-b15)
Java HotSpot(TM) 64-Bit Server VM (build 24.80-b11, mixed mode)
*///:~
```

## 安装Scala

* [downloads](http://www.scala-lang.org/download/)

```
yum -y install wget
wget http://downloads.typesafe.com/scala/2.11.5/scala-2.11.5.tgz
mkdir /usr/local/scala
tar -zxvf scala-2.11.5.tgz -C /usr/local/scala
vim ~/.bash_profile
/* Input:
# SCALA
export SCALA_HOME=/usr/local/scala/scala-2.11.5
export PATH=$PATH:$SCALA_HOME/bin
*///:~
source ~/.bash_profile
scala -version
/* Output:
Scala code runner version 2.11.5 -- Copyright 2002-2013, LAMP/EPFL
*///:~
```

## 安装Spark

* [downloads](http://spark.apache.org/downloads.html)

```
wget http://archive.apache.org/dist/spark/spark-1.2.0/spark-1.2.0-bin-hadoop1.tgz
mkdir /usr/local/spark
tar -zxvf spark-1.2.0-bin-hadoop1.tgz -C /usr/local/spark
vim ~/.bash_profile
/* Input:
# SPARK
export SPARK_HOME=/usr/local/spark/spark-1.2.0-bin-hadoop1
export PATH=$PATH:$SPARK_HOME/bin
export CLASSPATH=.:$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
*///:~
source ~/.bash_profile
pyspark --master local[2]
/* Output:
Python 2.7.5 (default, Oct 30 2018, 23:45:53)
...
*///:~
>>> exit()
cd /usr/local/spark/spark-1.2.0-bin-hadoop1/conf/
cp spark-env.sh.template spark-env.sh
vim spark-env.sh
/* Input:
export SPARK_MASTER_IP=127.0.0.1
export SPARK_LOCAL_IP=127.0.0.1
*///:~
cd ~
spark-shell
/* Output:
Spark assembly has been built with Hive, including Datanucleus jars on classpath
...
scala> 
*///:~
scala> :q
```

## WordCount

```
cd /usr/local/spark/spark-1.2.0-bin-hadoop1/bin
spark-shell
scala> val lines = sc.textFile("../README.md", 2)
/* Output:
19/09/14 10:35:42 INFO MemoryStore: ensureFreeSpace(32768) called with curMem=0, maxMem=280248975
19/09/14 10:35:42 INFO MemoryStore: Block broadcast_0 stored as values in memory (estimated size 32.0 KB, free 267.2 MB)
19/09/14 10:35:42 INFO MemoryStore: ensureFreeSpace(4959) called with curMem=32768, maxMem=280248975
19/09/14 10:35:42 INFO MemoryStore: Block broadcast_0_piece0 stored as bytes in memory (estimated size 4.8 KB, free 267.2 MB)
19/09/14 10:35:42 INFO BlockManagerInfo: Added broadcast_0_piece0 in memory on localhost:42664 (size: 4.8 KB, free: 267.3 MB)
19/09/14 10:35:42 INFO BlockManagerMaster: Updated info of block broadcast_0_piece0
19/09/14 10:35:42 INFO SparkContext: Created broadcast 0 from textFile at <console>:12
lines: org.apache.spark.rdd.RDD[String] = ../README.md MappedRDD[1] at textFile at <console>:12
*///:~
scala> val words = lines.flatMap(line => line.split(" "))
/* Output:
words: org.apache.spark.rdd.RDD[String] = FlatMappedRDD[2] at flatMap at <console>:14
*///:~
scala> val ones = words.map(w => (w, 1))
/* Output:
ones: org.apache.spark.rdd.RDD[(String, Int)] = MappedRDD[3] at map at <console>:16
*///:~
scala> val counts = ones.reduceByKey(_ + _)
/* Output:
19/09/14 10:41:24 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
19/09/14 10:41:24 WARN LoadSnappy: Snappy native library not loaded
19/09/14 10:41:24 INFO FileInputFormat: Total input paths to process : 1
counts: org.apache.spark.rdd.RDD[(String, Int)] = ShuffledRDD[4] at reduceByKey at <console>:18
*///:~
scala> counts.foreach(println)
/* Output:
...
(For,2)
(processing.,1)
(Spark,15)
(particular,3)
(Programs,1)
(The,1)
...
*///:~
scala> :q
```