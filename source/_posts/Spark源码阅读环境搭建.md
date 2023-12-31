---
title: Spark源码阅读环境搭建
date: 2019-09-15 17:16:44
tags: Spark
categories: Spark
---
Spark源码阅读环境搭建

## Maven安装

* [Maven](http://maven.apache.org/index.html)

```
cd /Users/xdx/Workspace/Softwares
wget http://archive.apache.org/dist/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz
sudo mkdir /usr/local/maven
sudo tar -zxvf apache-maven-3.6.2-bin.tar.gz -C /usr/local/maven
vim ~/.bash_profile
/* Input:
# MAVEN
export MAVEN_HOME=/usr/local/maven/apache-maven-3.6.2
export PATH=$PATH:$MAVEN_HOME/bin
export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m"
*///:~
source ~/.bash_profile
mvn -v
/* Output:
Apache Maven 3.6.2
...
*///:~
```

## Spark源码编译

```
cd /Users/xdx/Workspace/SparkProjects
wget http://archive.apache.org/dist/spark/spark-1.2.0/spark-1.2.0.tgz
tar -zxvf spark-1.2.0.tgz
cd spark-1.2.0
mvn -T 5 -DskipTests clean package
/* Output:
[ERROR] Failed to execute goal net.alchim31.maven:scala-maven-plugin:3.2.0:compile (scala-compile-first) on project spark-streaming-flume_2.10: Execution scala-compile-first of goal net.alchim31.maven:scala-maven-plugin:3.2.0:compile failed.: CompileFailed -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/PluginExecutionException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <args> -rf :spark-streaming-flume_2.10
...
*///:~
vim pom.xml
/* Input:
 <dependency>
        <!-- Failed to execute goal net.alchim31.maven:scala-maven-plugin:3.2.0:compile -->
        <groupId>net.alchim31.maven</groupId>
        <artifactId>scala-maven-plugin</artifactId>
        <version>3.2.0</version>
      </dependency>
*///:~
mvn -T 5 -DskipTests clean package
/* Output:
[ERROR] Failed to execute goal net.alchim31.maven:scala-maven-plugin:3.2.0:compile (scala-compile-first) on project spark-streaming-flume_2.10: Execution scala-compile-first of goal net.alchim31.maven:scala-maven-plugin:3.2.0:compile failed.: CompileFailed -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/PluginExecutionException
[ERROR] 
[ERROR] After correcting the problems, you can resume the build with the command
[ERROR]   mvn <args> -rf :spark-streaming-flume_2.10
...
*///:~
cd /Users/xdx/Workspace/Softwares
wget http://archive.apache.org/dist/maven/maven-3/3.2.2/binaries/apache-maven-3.2.2-bin.tar.gz
sudo tar -zxvf apache-maven-3.2.2-bin.tar.gz -C /usr/local/maven
vim ~/.bash_profile
/* Input:
# MAVEN
export MAVEN_HOME=/usr/local/maven/apache-maven-3.2.2
export PATH=$PATH:$MAVEN_HOME/bin
export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m"
*///:~
source ~/.bash_profile
mvn -v
/* Output:
Apache Maven 3.6.2
...
*///:~
exit
mvn -v
/* Output:
Apache Maven 3.2.2
...
*///:~
cd /Users/xdx/Workspace/SparkProjects
cd spark-1.2.0
mvn -T 5 -DskipTests clean package
/* Output:
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO] 
[INFO] Spark Project Parent POM ........................... SUCCESS [  3.898 s]
[INFO] Spark Project Networking ........................... SUCCESS [ 10.270 s]
[INFO] Spark Project Shuffle Streaming Service ............ SUCCESS [  8.470 s]
[INFO] Spark Project Core ................................. SUCCESS [02:48 min]
[INFO] Spark Project Bagel ................................ SUCCESS [ 44.801 s]
[INFO] Spark Project GraphX ............................... SUCCESS [01:36 min]
[INFO] Spark Project Streaming ............................ SUCCESS [02:07 min]
[INFO] Spark Project Catalyst ............................. SUCCESS [02:16 min]
[INFO] Spark Project SQL .................................. SUCCESS [01:14 min]
[INFO] Spark Project ML Library ........................... SUCCESS [01:31 min]
[INFO] Spark Project Tools ................................ SUCCESS [ 29.564 s]
[INFO] Spark Project Hive ................................. SUCCESS [01:31 min]
[INFO] Spark Project REPL ................................. SUCCESS [ 39.208 s]
[INFO] Spark Project Assembly ............................. SUCCESS [ 35.906 s]
[INFO] Spark Project External Twitter ..................... SUCCESS [ 44.000 s]
[INFO] Spark Project External Flume Sink .................. SUCCESS [ 22.857 s]
[INFO] Spark Project External Flume ....................... SUCCESS [ 58.495 s]
[INFO] Spark Project External MQTT ........................ SUCCESS [ 40.438 s]
[INFO] Spark Project External ZeroMQ ...................... SUCCESS [ 46.336 s]
[INFO] Spark Project External Kafka ....................... SUCCESS [ 36.279 s]
[INFO] Spark Project Examples ............................. SUCCESS [01:13 min]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 10:00 min (Wall Clock)
[INFO] Finished at: 2019-09-15T21:41:11+08:00
[INFO] Final Memory: 64M/1263M
[INFO] ------------------------------------------------------------------------
*///:~
```

## Idea调试JavaWordCount

* JavaWordCount -> Edit Configurations -> Run/Debug Configurations

```
/* Input:
VM options: -Dspark.master=local
Program arguments: README.md
*///:~
```

* View -> Tool Windows -> Maven -> Spark Project External Flume Sink -> Generate Sources and Update Folders

* View -> Tool Windows -> Maven -> Spark Project Hive -> Generate Sources and Update Folders

```
/* Output:
Error: A JNI error has occurred, please check your installation and try again
Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/spark/api/java/function/FlatMapFunction
	at java.lang.Class.getDeclaredMethods0(Native Method)
	at java.lang.Class.privateGetDeclaredMethods(Class.java:2701)
	at java.lang.Class.privateGetMethodRecursive(Class.java:3048)
	at java.lang.Class.getMethod0(Class.java:3018)
	at java.lang.Class.getMethod(Class.java:1784)
	at sun.launcher.LauncherHelper.validateMainClass(LauncherHelper.java:544)
	at sun.launcher.LauncherHelper.checkAndLoadMain(LauncherHelper.java:526)
Caused by: java.lang.ClassNotFoundException: org.apache.spark.api.java.function.FlatMapFunction
	at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	... 7 more
*///:~
```

```
//: pom.xml
...
<!--<scope>provided</scope>-->
...
```

```
/* Output:
Exception in thread "main" java.lang.SecurityException: class "javax.servlet.FilterRegistration"'s signer information does not match signer information of other classes in the same package
	at java.lang.ClassLoader.checkCerts(ClassLoader.java:898)
	at java.lang.ClassLoader.preDefineClass(ClassLoader.java:668)
	at java.lang.ClassLoader.defineClass(ClassLoader.java:761)
	at java.security.SecureClassLoader.defineClass(SecureClassLoader.java:142)
	at java.net.URLClassLoader.defineClass(URLClassLoader.java:468)
	at java.net.URLClassLoader.access$100(URLClassLoader.java:74)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:369)
	at java.net.URLClassLoader$1.run(URLClassLoader.java:363)
	at java.security.AccessController.doPrivileged(Native Method)
	at java.net.URLClassLoader.findClass(URLClassLoader.java:362)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
	at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
	at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
	at org.eclipse.jetty.servlet.ServletContextHandler.<init>(ServletContextHandler.java:136)
	at org.eclipse.jetty.servlet.ServletContextHandler.<init>(ServletContextHandler.java:129)
	at org.eclipse.jetty.servlet.ServletContextHandler.<init>(ServletContextHandler.java:98)
	at org.apache.spark.ui.JettyUtils$.createServletHandler(JettyUtils.scala:96)
	at org.apache.spark.ui.JettyUtils$.createServletHandler(JettyUtils.scala:87)
	at org.apache.spark.ui.WebUI.attachPage(WebUI.scala:67)
	at org.apache.spark.ui.WebUI$$anonfun$attachTab$1.apply(WebUI.scala:60)
	at org.apache.spark.ui.WebUI$$anonfun$attachTab$1.apply(WebUI.scala:60)
	at scala.collection.mutable.ResizableArray$class.foreach(ResizableArray.scala:59)
	at scala.collection.mutable.ArrayBuffer.foreach(ArrayBuffer.scala:47)
	at org.apache.spark.ui.WebUI.attachTab(WebUI.scala:60)
	at org.apache.spark.ui.SparkUI.initialize(SparkUI.scala:50)
	at org.apache.spark.ui.SparkUI.<init>(SparkUI.scala:63)
	at org.apache.spark.ui.SparkUI$.create(SparkUI.scala:153)
	at org.apache.spark.ui.SparkUI$.createLiveUI(SparkUI.scala:108)
	at org.apache.spark.SparkContext.<init>(SparkContext.scala:260)
	at org.apache.spark.api.java.JavaSparkContext.<init>(JavaSparkContext.scala:61)
	at org.apache.spark.examples.JavaWordCount.main(JavaWordCount.java:44)
*///:~
```

* Maven Helper

* SonarLint

* IDEA Mind Map

```
vim ~/.bash_profile
/* Input:
# MAVEN
export MAVEN_HOME=/usr/local/maven/apache-maven-3.6.2
export PATH=$PATH:$MAVEN_HOME/bin
export MAVEN_OPTS="-Xmx2g -XX:ReservedCodeCacheSize=512m"
*///:~
source ~/.bash_profile
exit
cd /Users/xdx/Workspace/SparkProjects
wget http://archive.apache.org/dist/spark/spark-2.4.4/spark-2.4.4.tgz
tar -zxvf spark-2.4.4.tgz
cd spark-2.4.4
mvn -T 5 -DskipTests clean package
/* Output:
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for Spark Project Parent POM 2.4.4:
[INFO] 
[INFO] Spark Project Parent POM ........................... SUCCESS [01:54 min]
[INFO] Spark Project Tags ................................. SUCCESS [ 28.305 s]
[INFO] Spark Project Sketch ............................... SUCCESS [ 11.713 s]
[INFO] Spark Project Local DB ............................. SUCCESS [  6.649 s]
[INFO] Spark Project Networking ........................... SUCCESS [01:27 min]
[INFO] Spark Project Shuffle Streaming Service ............ SUCCESS [  3.910 s]
[INFO] Spark Project Unsafe ............................... SUCCESS [02:36 min]
[INFO] Spark Project Launcher ............................. SUCCESS [01:20 min]
[INFO] Spark Project Core ................................. SUCCESS [12:41 min]
[INFO] Spark Project ML Local Library ..................... SUCCESS [02:38 min]
[INFO] Spark Project GraphX ............................... SUCCESS [29:15 min]
[INFO] Spark Project Streaming ............................ SUCCESS [30:22 min]
[INFO] Spark Project Catalyst ............................. SUCCESS [31:43 min]
[INFO] Spark Project SQL .................................. SUCCESS [49:35 min]
[INFO] Spark Project ML Library ........................... SUCCESS [03:48 min]
[INFO] Spark Project Tools ................................ SUCCESS [ 56.221 s]
[INFO] Spark Project Hive ................................. SUCCESS [02:31 min]
[INFO] Spark Project REPL ................................. SUCCESS [ 17.349 s]
[INFO] Spark Project Assembly ............................. SUCCESS [  2.984 s]
[INFO] Spark Integration for Kafka 0.10 ................... SUCCESS [20:45 min]
[INFO] Kafka 0.10+ Source for Structured Streaming ........ SUCCESS [01:16 min]
[INFO] Spark Project Examples ............................. SUCCESS [ 57.738 s]
[INFO] Spark Integration for Kafka 0.10 Assembly .......... SUCCESS [  5.975 s]
[INFO] Spark Avro ......................................... SUCCESS [ 59.228 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  01:43 h (Wall Clock)
[INFO] Finished at: 2019-09-16T23:55:40+08:00
[INFO] ------------------------------------------------------------------------
*///:~
```

* JavaWordCount -> Create 'JavaWordCount.main()' -> Run/Debug Configurations

```
/* Input:
VM options: -Dspark.master=local
Program arguments: README.md
*///:~
```

-> Run 'JavaWordCount.main()'