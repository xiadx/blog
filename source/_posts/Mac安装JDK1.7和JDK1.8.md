---
title: Mac安装JDK1.7和JDK1.8
date: 2019-09-13 00:00:44
tags: Java
categories: Java
---
Mac安装JDK1.7和JDK1.8

## JDK

* [JDK](https://www.oracle.com/technetwork/java/javase/archive-139210.html)

```
which java
/* Output:
/usr/bin/java
*///:~
cd /usr/bin
ls -l | grep java$
/* Output:
lrwxr-xr-x   1 root   wheel        74 Apr 30 09:30 java -> /System/Library/Frameworks/JavaVM.framework/Versions/Current/Commandsjava
*///:~
cd /Library/Java/JavaVirtualMachines
ls -l
/* Output:
drwxr-xr-x  3 root  wheel  96 Sep 20 18:10 jdk1.7.0_80.jdk
drwxr-xr-x  3 root  wheel  96 Jun 25 12:41 jdk1.8.0_191.jdk
*///:~
/usr/libexec/java_home -v 1.7
/* Output:
/Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home
*///:~
vim ~/.bash_profile
/* Input:
# JAVA
export JAVA_7_HOME="$(/usr/libexec/java_home -v 1.7)"
export JAVA_8_HOME="$(/usr/libexec/java_home -v 1.8)"
alias jdk1.7="export JAVA_HOME=$JAVA_7_HOME"
alias jdk1.8="export JAVA_HOME=$JAVA_8_HOME"
export JAVA_HOME=$JAVA_8_HOME
*///:~
source ~/.bash_profile
java -version
/* Output:
java version "1.8.0_191"
...
*///:~
jdk1.7
java -version
/* Output:
java version "1.7.0_80"
...
*///:~
cd ~
exit
```