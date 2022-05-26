---
title: mysql windows 定时任务备份方案
tags: 数据库备份
layout: post
---

#### 1、新建mysql_bakup.bat脚本如下：
```
@echo off
echo ------------------------- 
echo mysql backup 
echo ------------------------- 
set "Ymd=%date:~,4%%date:~5,2%%date:~8,2%"
cd C:\Program Files\MySQL\MySQL Server 5.7\bin
mysqldump -h localhost -u root -123456 -P 3306 bhxq_db >C:\db_backup\db_%Ymd%.sql
cd \ 
@echo off 
```

#### 2、 windows窗口 win+r 打开运行窗口,输入：compmgmt.msc,如下图所示：
![mysqlWindows备份步骤]( https://wanglizhi2015.github.io/assets/images/posts/mysql-win-bak/1.jpg )
#### 3、 系统工具->任务计划程序->任务计划程序库->右键创建任务
![mysqlWindows备份步骤]( https://wanglizhi2015.github.io/assets/images/posts/mysql-win-bak/2.jpg )
#### 4、 常规->勾选->不管用户是否登录都要运行
![mysqlWindows备份步骤]( https://wanglizhi2015.github.io/assets/images/posts/mysql-win-bak/3.jpg )
#### 5、 触发器->新建->设置定时任务执行时间和频次
![mysqlWindows备份步骤]( https://wanglizhi2015.github.io/assets/images/posts/mysql-win-bak/4.jpg )
#### 6、 操作->新建->选择第一步新建的bat脚本->确定
![mysqlWindows备份步骤]( https://wanglizhi2015.github.io/assets/images/posts/mysql-win-bak/5.jpg )