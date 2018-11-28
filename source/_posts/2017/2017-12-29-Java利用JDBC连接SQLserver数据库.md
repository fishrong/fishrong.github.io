---
layout: posts
title: Java利用JDBC连接SQLserver数据库及简单的增删改操作
categories: Java学习笔记
tags: 
    - java
    - JDBC
    - SQLserver
---
## 准备工作  
#### 1.下载JDBC驱动包。[sqljdbc4.jar](https://pan.baidu.com/s/1kU9JoLD),也可以从官网下载[https://www.microsoft.com/zh-cn/download/details.aspx?id=11774](https://www.microsoft.com/zh-cn/download/details.aspx?id=11774)。  
#### 2.用sql server身份验证方式连接数据库。
如果安装sql server时是以windows身份验证安装的，没有为sql server添加sql sever身份用户，需要首先添加用户：
打开Microsoft SQL Server Management Studio并以windows验证方式登录，左侧的对象资源管理器->安全性->登录名，右击sa->属性，为sa用户添加密码，选择sql server身份验证，在“状态”项中授予连接到数据库和登录启用;
#### 3.以下代码是基于新建了一个名为`test`的数据库，在该库下的建了一个`student`表。  
　　1.表的结构如下：   
![](http://onkin31ah.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20171229135531.jpg)   
　　2.可以先在表中增加一些值。右键`dbo.student`这个表->编辑前200行。    
![](http://onkin31ah.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE.jpg)
## 开始编程  
#### 1.打开Eclipse新建一个项目。
<!-- more -->
#### 2.在项目目录下右键`src`->Build Path->Configure Build Path->Libraries->Add External JARs->找到之前下载的JDBC驱动包，添加即可。
#### 3.在项目下添加以下三个class。  
##### Student.java  
```java
public class Student {
	String No;//学号
	String Name;//姓名
	public Student(String sNo,String sName){
		No=sNo;//成员变量初始化
		Name=sName;
	}
}

```
##### StudentSystem.java  
```
import java.sql.*;//导入包
public class StudentSystem {
	Connection conn; //数据库连接对象
    Statement stmt;//执行简单的SQL语句
    PreparedStatement pstm;//执行带参数的SQL语句
   ResultSet rs;//保存查询结果
   String url = "jdbc:sqlserver://localhost:1433;DatabaseName=test;";//本地数据库地址及数据库名（DatabaseName是自己建立的数据库名）
   StudentSystem()throws Exception{
	   conn = DriverManager.getConnection(url, "sa", "123456");//加载驱动并连接数据库（其中第一个参数为数据库地址，第二、三个参数分别为数据库用户名和密码）
   }
   //查询并显示全部学生信息
   void selectAll()throws SQLException{
	   stmt = conn.createStatement();//创建Statement对象
	   rs = stmt.executeQuery("select * from student");//执行SQL语句
	   while(rs.next()){
           String name = rs.getString("Name");
           String No = rs.getString("No");
           System.out.println("No:" + No + "\tName:" + name);
	   }
   }
   //查找特定学生信息
   void selectNo(String No)throws SQLException{
	   stmt = conn.createStatement();
	   rs = stmt.executeQuery("select * from student where No='"+No+"'");
	   if(rs.next()){
		   String name = rs.getString("Name");
           String No1 = rs.getString("No");
           System.out.println("No:" + No1 + "\tName:" + name);
	   }
   }
 //插入学生信息
   void insertStudent(Student s)throws SQLException{
	   pstm=conn.prepareStatement("insert into student values(?,?)");
	   pstm.setString(1,s.No);//插入values的第一个参数；
	   pstm.setString(2, s.Name);
	   
	   pstm.executeUpdate();
	   pstm.close();
   }
   //删除指定学生信息
   void deleteNo(String No)throws SQLException{
	   pstm=conn.prepareStatement("delete from student where No=?");
	   pstm.setString(1, No);
	   pstm.executeUpdate();
	   pstm.close();
   }
   //修改学生信息
   void updateStudent(Student s)throws SQLException{
	   pstm=conn.prepareStatement("update student set Name=? where No=?");
	   pstm.setString(1,s.Name);//插入values的第一个参数；
	   pstm.setString(2, s.No);
	   pstm.executeUpdate();
	   pstm.close();
   }
   void closeConn()throws SQLException{conn.close();}
  
}

```
##### MainStudent.java  
```
public class MainStudent {

	public static void main(String[] args)throws Exception {
		
		StudentSystem ss=new StudentSystem();//创建数据库操作实例
		Student s=new Student("2017","st1");//创建学生对象
//		ss.insertStudent(s);//将学生信息插入到数据库
//		ss.deleteNo("001");//删除学号为001学生的信息
//		ss.updateStudent(s);//修改学号为s.No学生的姓名
//以上三条语句根据需要选择打开
		ss.selectAll();//查询并显示所有学生信息
		ss.closeConn();//关闭数据库连接
	}
}

```
以上程序运行后会输出数据库中的所有信息。