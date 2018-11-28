---
layout: posts
title: hibernate的基本使用
categories: Java学习笔记
tags:
    - hibernate
---
hibernate框架的主要作用就是将面向对象语言转化为操作关系数据的语言。
## 一、导入相关jar包
1. 1、将下载的hibernate解压缩，导入`hibernate-release-5.3.5.Final\lib\required`目录下的所有jar包。
2. 2、导入mysql驱动
## 二、编写持久化类
注意几点：
1. 1、类名对应表名
2. 2、成员变量对应数据库字段
3. 3、提供getter和setter方法即可
## 三、创建持久类的映射文件
注意：
* 命名规则为：类名.hbm.xml
* 位置：和持久化类同目录下
```
<?xml version="1.0" encoding="UTF-8"?>
<!-- 以下来自hibernate核心类下的 /org/hibernate/hibernate-mapping-3.0.dtd的约束-->
<!DOCTYPE hibernate-mapping PUBLIC
    "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
    "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping package="cn.wangsr.domain"><!-- 包名 -->
	<class name="Customer" table="Customer"><!-- 表名 -->
		<id name="custId" column="cust_id"></id><!-- 主键 -->
		<property name="custName" column="cust_name"></property><!-- 其它字段 -->
	</class>
</hibernate-mapping>
```
<!-- more -->
## 创建主配置文件
注意：
* 命名规则为：hibernate.cfg.xml
* 位置：在src目录下
```
<?xml version="1.0" encoding="UTF-8"?>
<!-- 在hibernate核心包目录下 /org/hibernate/hibernate-configuration-3.0.dtd-->
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory>
	<!-- 连接数据库信息 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql://localhost:3306/first_hibernate</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">123456</property>
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
	<!-- 可选项 -->
		<property name="hibernate.hbm2ddl.auto">update</property>
		<property name="hibernate.show_sql">true</property>
	<!-- 映射文件位置 -->
		<mapping resource="cn/wangsr/domain/Customer.hbm.xml"/>
	</session-factory>
</hibernate-configuration>
```

## 使用
```
public class HibernateDemo1 {
	@Test
	public void test1() {
		Customer c = new Customer();
		c.setCustName("abc");
		//解析主配置文件
		Configuration cfg =new Configuration();
		cfg.configure();
		//根据配置创建SessionFactory
		SessionFactory factory = cfg.buildSessionFactory();
		//根据SessionFactory创建Session
		Session session = factory.openSession();
		//开启事务
		Transaction tx = session.beginTransaction();
		//执行操作（保存）
		session.save(c);

    /*更新
		Customer c = session.get(Customer.class, 1L);
		c.setCustName("666");
		session.update(c);*/
    /*删除id未1
		Customer c = session.get(Customer.class, 1L);
		session.delete(c);*/


		//提交事务
		tx.commit();
		//释放资源
		session.close();
		factory.close();
	}
}
```
## 抽取工具类
目的：是SessionFactory只创建一次
实现：使用静态初始化块
```
public class HibernateUtil{
  private static SessionFactory factory;
  static{
    Configuration cfg = new Configuration();
    cfg.configure();
    factory = cfg.buildSessionFactory();
  }
  public static Session openSession(){
    return factory.openSession();
  }

}
```
