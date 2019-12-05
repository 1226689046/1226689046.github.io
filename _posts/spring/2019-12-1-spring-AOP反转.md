---
layout: post
title: Spring AOP反转
tags:
- spring
categories: spring
description: Spring的AOP反转
---

# Spring的AOP反转

若手动创建一个aop的例子，需要创建一个代理工厂。这个东西现在交给spring容器来管理，只需要关心切面类、切入点和切入表达式就可以了。



#### 使用Spring注解实现AOP

加入aop的包（依赖）

需要在命名空间中加入：

```
xmlns:aop="http://www.springframework.org/schema/aop"

        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd"
        
```

- 切面的依赖

> <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
> <dependency>
>     <groupId>org.aspectj</groupId>
>     <artifactId>aspectjweaver</artifactId>
>     <version>1.9.5</version>
> </dependency>

使用

```
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

开启代理

#### 使用注解实现aop

```java
@Component//把这个对象加入容器中，让容器来管理
@Aspect//这个注解是定义为切面类
public class AOP {  
    @Before("execution(* aop.*.*(..))")  
    public void begin(){    
        System.out.println("开始事务");    
    }   
    @After("execution(* aop.*.*(..))") 
    public void end(){   
        System.out.println("结束事务"); 
    }
}
```

注意切入表达式的书写规则。。

切入类：

```java
@Repository//这个在dao层使用  持久层
public class UserDao implements iUser{  
    @Override   
    public void save() { 
        System.out.println("存储数据");  
    }
}
```

切入类的接口：

```java
//接口
public interface iUser {   
    void save();
}
```

注意，在从容器中getBean的时候，需要用的是iUser接口。因为Aspect切面技术是基于接口的动态代理技术，而不是实现类的动态代理技术。



但是如果目标类没有接口，则会使用cglib代理。

动态代理和cglib代理是自动切换的。

#### 各种api

- **@Aspect**               指定一个类为切面类
- **@Pointcut("execution(\* cn.itcast.e_aop_anno.\*.\*(..))")  指定切入点表达式**
- **@Before("pointCut_()")**         前置通知: 目标方法之前执行
- **@After("pointCut_()")**         **后置通知：目标方法之后执行（始终执行）**
- @AfterReturning("pointCut_()")       返回后通知： **执行方法结束前执行(异常不执行)**
- @AfterThrowing("pointCut_()")       异常通知:  出现异常时候执行
- @Around("pointCut_()")         环绕通知： 环绕目标方法执行

#### 优化

```java
@Pointcut("execution(* aop.*.*(..))")
public void printTxt(){
    
}
```

调用的时候，使用方法名（）这样就可以调用这个切入表达式

```xml
<bean id="userDao" class="aop.UserDao"/>
<bean id="noInterfaceUser" class="aop.NoInterfaceUser"/>
<bean id="aop" class="aop.AOP"/>
<!--配置切面-->
<aop:config>
    <!--配置切入点-->
    <aop:pointcut id="pointCut" expression="execution(* aop.*.*(..))"/> 
    <!--配置切入信息-->
    <aop:aspect ref="aop">  
    	<aop:before method="begin" pointcut-ref="pointCut"/>    
    	<aop:after method="end" pointcut-ref="pointCut"/>  
    </aop:aspect>
</aop:config>
```

#### 切入点表达式

```xml
 【拦截所有public方法】 
    <aop:pointcut expression="execution(public * *(..))" id="pt"/>

     【拦截所有save开头的方法 】 
    <aop:pointcut expression="execution(* save*(..))" id="pt"/>

     【拦截指定类的指定方法, 拦截时候一定要定位到方法】 
    <aop:pointcut expression="execution(public * cn.itcast.g_pointcut.OrderDao.save(..))" id="pt"/>

     【拦截指定类的所有方法】 
    <aop:pointcut expression="execution(* cn.itcast.g_pointcut.UserDao.*(..))" id="pt"/>

     【拦截指定包，以及其自包下所有类的所有方法】 
    <aop:pointcut expression="execution(* cn..*.*(..))" id="pt"/>

     【多个表达式】 
    <aop:pointcut expression="execution(* cn.itcast.g_pointcut.UserDao.save()) || execution(* cn.itcast.g_pointcut.OrderDao.save())" id="pt"/>
    <aop:pointcut expression="execution(* cn.itcast.g_pointcut.UserDao.save()) or execution(* cn.itcast.g_pointcut.OrderDao.save())" id="pt"/>
     下面2个且关系的，没有意义 
    <aop:pointcut expression="execution(* cn.itcast.g_pointcut.UserDao.save()) &amp;&amp; execution(* cn.itcast.g_pointcut.OrderDao.save())" id="pt"/>
    <aop:pointcut expression="execution(* cn.itcast.g_pointcut.UserDao.save()) and execution(* cn.itcast.g_pointcut.OrderDao.save())" id="pt"/>

     【取非值】 
    <aop:pointcut expression="!execution(* cn.itcast.g_pointcut.OrderDao.save())" id="pt"/>
```

