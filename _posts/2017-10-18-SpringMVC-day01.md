---
layout: post
title:  "SpringMVC-day01"
categories: SpringMVC
tags: SpringMVC
author: Jason
excerpt_separator: ">"
---

* 目录
{:toc}

## Spring MVC介绍

> 和struts2都属于web表现层框架

![springFramework](/img/springFramework.png)

## 入门程序

1. 导包
2. 在web.xml中配置前端控制器（指定上下文路径classpath:springmvc.xml）

```
<!--前端控制器-->
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 默认找/WEB-INF/[servlet名称]-servlet.xml -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>

    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
        拦截规则：
            1. /* 拦截所有 建议不适用
            2. *.action *.do 拦截以do action结尾的请求 肯定能使用
            3. / 拦截所有（不包括jsp）（包括.js, .png, .css等） 推荐使用
        -->
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>
```

3. springmvc.xml配置扫描注解

4. 方法上@RequestMapping(value = 请求路径)

```
public ModelAndView itemList() {...}
New ModelAndView
设置数据
设置JSP页面的路径
```

## 架构分析

### Spring MVC架构
![springmvc](/img/springmvc.png)

- 一个中心：前端控制器
- 三个组件：处理器映射器，处理器适配器，视图解析器（组件由springmvc提供）
- Handler处理器 JSP视图 由程序员书写

## 整合mybatis
整合思想：
1. sqlMapConfig.xml 核心配置文件 （别名）

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 设置别名 -->
    <typeAliases>
        <!-- 2. 指定扫描包，会把包内所有的类都设置别名，别名的名称就是类名，大小写不敏感 -->
        <package name="cc.jasonos.springmvc.pojo" />
    </typeAliases>

</configuration>
```

2. applicationConetxt.xml 数据源 druid 读取db.properties Mybatis工厂 Mapper代理开发的扫描方式 基本包

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-4.0.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.0.xsd">

    <!-- 加载配置文件 -->
    <context:property-placeholder location="classpath:db.properties"/>

    <!-- 数据库连接池 -->
    <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"
          destroy-method="close">
        <property name="driverClassName" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="maxActive" value="10"/>
        <property name="maxIdle" value="5"/>
    </bean>

    <!--Mybatis的工厂 -->
    <bean id="sqlSessionFactoryBean" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 核心配置文件的位置 -->
        <property name="configLocation" value="classpath:sqlMapConfig.xml"/>
    </bean>

    <!-- Mapper动态代理开发   扫描 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!-- 基本包 -->
        <property name="basePackage" value="cc.jasonos.springmvc.dao"/>
    </bean>

    <!--注解事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!--开启注解-->
    <tx:annotation-driven transaction-manager="transactionManager"/>

</beans>
```

3. 到此，spring+mybatis整合完毕
4. springmvc.xml 三大组件 扫描基本包（controller，service全扫描）
5. web.xml 配置监听器来读取applicationContext.xml 配置前端控制器来读取springmvc.xml 配置过滤器解决乱码

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <!-- 配置controller,service扫描包 -->
    <context:component-scan base-package="cc.jasonos.springmvc" />

    <!--注解驱动-->
    <mvc:annotation-driven conversion-service="conversionService"/>

    <!-- 配置Conveter转换器  转换工厂 （日期、去掉前后空格）。。 -->
    <bean id="conversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <!-- 配置 多个转换器-->
        <property name="converters">
            <set>
                <bean class="cc.jasonos.springmvc.conversion.DateConveter"/>
            </set>
        </property>
    </bean>

    <!--上传图片配置实现类-->
    <bean class="org.springframework.web.multipart.commons.CommonsMultipartResolver" id="multipartResolver">
        <!--上传图片大小 B 5M = 1*1024*1024*5-->
        <property name="maxUploadSize" value="5000000"/>
    </bean>

    <!--异常处理器-->
    <bean class="cc.jasonos.springmvc.exception.CustomExceptionResolver"/>

    <!--springmvc拦截器-->
    <mvc:interceptors>
        <!--多个拦截器-->
        <mvc:interceptor>
            <mvc:mapping path="/item/**"/>
            <!--自定义的拦截器类-->
            <bean class="cc.jasonos.springmvc.interceptor.Interceptor1"/>
        </mvc:interceptor>
    </mvc:interceptors>

    <!--视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

## 参数绑定

1. 默认参数绑定 Request Response Session Model 实现关系 ModelMap
2. 简单类型参数绑定 方法的形参上（Integer id String ...）
3. pojo参数绑定 Items items input name = name Items对象中的属性名一致
4. 包装类型 QueryVo（里面Items） QueryVo vo   items.name
5. 自定义参数类型 转换日期格式 
在springmvc.xml中配置转换器的工厂 Converters list set array

## Struts2和springmvc区别
1. springmvc的入口是一个servlet即前端控制器；而struts2入口是一个filter过滤器
2. springmvc基于方法开发（一个url对应一个方法），请求参数传递到方法的形参，建议单例；struts2是基于类开发，传递参数是通过类的属性，只能设计为多例
3. springmvc通过参数解析器将request请求解析，并给方法中的形参赋值，将数据和视图封装成ModelAndView对象，最后将ModelAndView中的模型数据通过**request域**传输到页面。JSP视图解析器默认是 jstl；Struts采用**值栈**存储请求和响应的数据，通过OGNL存取数据
