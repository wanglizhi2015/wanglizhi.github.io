---
title: linux系统安装jdk方法
tags: jdk安装
layout: post
---

### linux系统安装jdk

1、在home目录下新建software目录

[root@localhost /]# mkdir home/software

2、在usr目录下新建java目录

[root@localhost /]# mkdir usr/java

3、将jdk压缩文件上传至home/software目录下

4、解压jdk

[root@localhost /]# tar -zxvf jdk-8u191-linux-x64.tar.gz

5、将解压后的jdk移至usr/java目录下

[root@localhost /]#  mv jdk1.8.0_191/ /usr/java/

6、配置环境变量

编辑etc下的profile文件（vi /etc/profile）并在最后添加jdk环境变量：

```
export JAVA_HOME=/usr/java/jdk1.8.0_191
export CLASSPATH=.:%JAVA_HOME%/lib/dt.jar:%JAVA_HOME%/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin

```
7、刷新profile,使其生效

[root@localhost /]# source /etc/profile