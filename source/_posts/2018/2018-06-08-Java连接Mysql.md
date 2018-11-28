---
layout: posts
title: Java连接MySQL常见问题
categories: Java学习笔记
tags: 
    - MySQL
    - Java
---
使用数据库版本8.0.11 JDBC版本8.0.11
### 1.首先查看mysql服务是否正常启动。
### 2.报错如下：
```java.sql.SQLException: The server time zone value ‘XXXXXX’ is unrecognized or represents more than one time zone.
```

#### 解决办法：
##### 方法1： mysql命令行下执行 
```
set global time_zone='+8:00'
```

##### 方法2：建立连接时带上参数"serverTimezone=UTC"

### 3.默认useSSL为开启，需显示关闭useSSL=false,否则报错

