---
layout: posts
title: hibernate常见问题
categories: Java学习笔记
tags: 
    - hibernate
---
#### 报错：java.lang.NoClassDefFoundError: javax/xml/bind/JAXBException（运行环境：jdk10)
解决方法一：将JDK换成8或以下
解决办法二：导入下面4个jar包
1. [javax.activation-1.2.0.jar](http://search.maven.org/remotecontent?filepath=com/sun/activation/javax.activation/1.2.0/javax.activation-1.2.0.jar)  
2. [jaxb-api-2.3.0.jar](http://search.maven.org/remotecontent?filepath=javax/xml/bind/jaxb-api/2.3.0/jaxb-api-2.3.0.jar)
3. [jaxb-core-2.3.0.jar](http://search.maven.org/remotecontent?filepath=com/sun/xml/bind/jaxb-core/2.3.0/jaxb-core-2.3.0.jar)
4. [jaxb-impl-2.3.0.jar](http://search.maven.org/remotecontent?filepath=com/sun/xml/bind/jaxb-impl/2.3.0/jaxb-impl-2.3.0.jar)  
具体原因参考：https://stackoverflow.com/questions/43574426/how-to-resolve-java-lang-noclassdeffounderror-javax-xml-bind-jaxbexception-in-j
