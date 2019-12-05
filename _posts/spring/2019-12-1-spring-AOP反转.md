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

