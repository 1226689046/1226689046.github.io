---
layout: post
title: Spring基本配置
tags:
- spring
categories: spring
description: Spring的基本配置，以后不许再去搜索引擎搜索了。
---

# Spring的基本配置

以前对于Spring的学习，一直都是在不断的重复基础的内容，没有进行记录，这是一个非常不好的习惯，如果进行记录了，下一次就可以直接到这里看，而不用重复的进行无用的搜索。总结真的是一个很好的习惯

## 基本理解

1. Spring实现松耦合
2. 非侵入式框架：可以不修改原有的类，从而增强JavaBean的功能
3. 切面编程：AOP切面编程，在执行某些代码前，执行另外的代码
4. IOC反转，控制反转，以前写代码都是自己手动new出来一个类，但是现在就可以直接将类的控制权交给Spring容器，让Spring容器来进行创建

> ioc的思想最核心的地方在于，资源不由使用资源的双方管理，而由不使用资源的第三方管理，这可以带来很多好处。**第一，资源集中管理，实现资源的可配置和易管理**。**第二，降低了使用资源双方的依赖程度，也就是我们说的耦合度**。
>
> 也就是说，甲方要达成某种目的不需要直接依赖乙方，它只需要达到的目的告诉第三方机构就可以了，比如甲方需要一双袜子，而乙方它卖一双袜子，它要把袜子卖出去，并不需要自己去直接找到一个卖家来完成袜子的卖出。它也只需要找第三方，告诉别人我要卖一双袜子。这下好了，甲乙双方进行交易活动，都不需要自己直接去找卖家，相当于程序内部开放接口，卖家由第三方作为参数传入。甲乙互相不依赖，而且只有在进行交易活动的时候，甲才和乙产生联系。反之亦然。这样做什么好处么呢，甲乙可以在对方不真实存在的情况下独立存在，而且保证不交易时候无联系，想交易的时候可以很容易的产生联系。甲乙交易活动不需要双方见面，避免了双方的互不信任造成交易失败的问题。**因为交易由第三方来负责联系，而且甲乙都认为第三方可靠。那么交易就能很可靠很灵活的产生和进行了**。这就是ioc的核心思想。生活中这种例子比比皆是，支付宝在整个淘宝体系里就是庞大的ioc容器，交易双方之外的第三方，提供可靠性可依赖可灵活变更交易方的资源管理中心。另外人事代理也是，雇佣机构和个人之外的第三方。
> ==========================update===========================
>
> 在以上的描述中，诞生了两个专业词汇，依赖注入和控制反转所谓的依赖注入，则是，甲方开放接口，在它需要的时候，能够讲乙方传递进来(注入)所谓的控制反转，甲乙双方不相互依赖，交易活动的进行不依赖于甲乙任何一方，整个活动的进行由第三方负责管理。

5. 其他的好处
   - 不用自己组装，直接那拿来用就好了
   - 有一些类只需要是单例的就可以了，无需重复创建，不浪费控件，这一点可以使用Ioc容器来进行管理
   - 类的管理对于使用者来说是透明的
   - 同意配置，便于修改

## 基本步骤

1. 引入jar包（也可以使用Maven工程）

   1. **commons-logging-1.1.3.jar      日志**
   2. **spring-beans-3.2.5.RELEASE.jar     bean节点**
   3. **spring-context-3.2.5.RELEASE.jar    spring上下文节点**
   4. **spring-core-3.2.5.RELEASE.jar     spring核心功能**
   5. **spring-expression-3.2.5.RELEASE.jar   spring表达式相关表**

2. 编写配置文件（ applicationContext.xml)

   1. 可以在文档中寻找到配置文件的头部

      >  <beans xmlns="http://www.springframework.org/schema/beans"
      >    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      >    xmlns:p="http://www.springframework.org/schema/p"
      >    xmlns:context="http://www.springframework.org/schema/context"
      >    xsi:schemaLocation="
      >      http://www.springframework.org/schema/beans
      >      http://www.springframework.org/schema/beans/spring-beans.xsd
      >      http://www.springframework.org/schema/context
      >      http://www.springframework.org/schema/context/spring-context.xsd">
      >
      > </beans>   

   2. Core模块是：Ioc容器，解决对象创建和之间的依赖关系

   3. maven中需要添加对应的依赖

3. 在resource目录下创建配置文件并且加载

   1. **Bean工厂，BeanFactory【功能简单】 **

      > ```java
      > Resource resource = new ClassPathResource("applicationContext.xml");
      > BeanFactory beanFactory = new XmlBeanFactory(resource);
      > ```

   2. **应用上下文，ApplicationContext【功能强大，一般我们使用这个】**

      > ```java
      > ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
      > ```

4. 在Spring中，有三种配置方式

   - 使用xml配置文件
   - 注解
   - JavaConfig（使用一个类当作配置文件）

5. 创建对象的几种方式

   -  无参构造函数 

     - > <bean id="user" name="user" class="pub.giao.bean.User"></bean>
       >
       > 然后使用 application.getBean("user",User.class)就可以。

   - 有参构造函数

     - > ```java
       > <bean id="userWithParam" name="userWithParam" class="pub.giao.bean.User">    <constructor-arg index="0" name="id" type="int" value="1"/>    <constructor-arg index="1" name="username" type="java.lang.String" value="张三"/></bean>
       > ```

       注意，这里的type不会自动拆箱装箱

       注意，如果是引用的一个类型，那么value要换成ref

   - 工厂

     - 静态工厂

       - ```java
         <bean id="staticFactoryUser" class="pub.giao.factory.StaticUserFactory" factory-method="newInstance"/>
         ```

          工厂静态方法创建对象，直接使用class指向静态类，指定静态方法就行了 

         ```java
         public class StaticUserFactory {    
             public static User newInstance(){        
                 return new User(2,"静态工厂创建");    
             }
         }
         ```

     - 非静态工厂

       - ```java
         <!--    先写好工厂-->    
         <bean id="noStaticFactoryUser" class="pub.giao.factory.NoStaticUserFactory" />
         <!--    声明bean-->    
         <bean id="noStaticUser" class="pub.giao.bean.User" factory-bean="noStaticFactoryUser" factory-method="newInstance"/>
         ```

6. c名称空间（第一次听说这个东西）

   - 在创建存在有参构造函数的类的时候，需要用到<constructor-arg> 这个标签，也可以使用c名称空间

     >  <bean id="userService" class="bb.UserService" c:userDao-ref="">
     >
     >    </bean>

   - 缺点：不能装配集合

7. 装载集合

   - 对象的属性/构造函数的参数是一个集合

     - **在构造函数上，普通类型**

     ```xml
     <bean id="userService" class="bb.UserService" >        
         <constructor-arg >            
             <list>                
                 //普通类型            
                 <value></value>    
             </list>     
         </constructor-arg>  
     </bean>
     ```

     - **在属性上,引用类型**

     ```xml
     <property name="userDao">    
         <list>            
             <ref></ref>   
         </list>   
     </property>
     ```

8. 注解方式

   1. 开启注解扫描器

      >  <context:component-scan base-package=""></context:component-scan> 

   2. > - **@ComponentScan扫描器**
      >
      > - **@Configuration表明该类是配置类**
      >
      > - **@Component  指定把一个对象加入IOC容器--->@Name也可以实现相同的效果【一般少用】**
      >
      > - **@Repository  作用同@Component； 在持久层使用**
      >
      > - **@Service    作用同@Component； 在业务逻辑层使用**
      >
      > - **@Controller   作用同@Component； 在控制层使用**
      >
      > - **@Resource  依赖关系**
      >
      > - - **如果@Resource不指定值，那么就根据类型来找，相同的类型在IOC容器中不能有两个**
      >   - **如果@Resource指定了值，那么就根据名字来找**

   3. 使用注解方式：

      - @Repository

        > ```java
        > @Repositorypublic class Room {  
        >     public void getSize(){    
        >         System.out.println("获取大小");  
        >     }
        > }
        > ```

      - @Service

        > ```java
        > @Servicepublic class RoomService {   
        >     @Resource(name="room")  
        >     private Room room; 
        >     public void tellSize(){ 
        >         room.getSize();  
        >     }
        > }
        > ```

      - @Controller

        > ```java
        > @Controllerpublic class RoomAction {
        >     @Resource(name="roomService") 
        >     public RoomService roomService; 
        >     public String aa(){        
        >         roomService.tellSize();
        >         return "success";  
        >     }
        > }
        > ```

      这样就可以实现自动封装啥的

#### 使用Java方式的配置

1. 官方推荐的是注解-> class-> xml

2. > ```java
   > @org.springframework.context.annotation.Configuration
   > public class Configuration { 
   >     @Bean("javauser")   
   >     public User user(){  
   >         System.out.println("Configuration中创建");       
   >         return new User();   
   >     }
   > }
   > ```

#### 混合使用

> **注解和XML配置是可以混合使用的，JavaConfig和XML也是可以混合使用的…**
>
> 如果JavaConfig的配置类是分散的，我们一般再创建一个更高级的配置类（root），然后使用**@Import来将配置类进行组合**
> 如果XML的配置文件是分散的，我们也是创建一个更高级的配置文件（root），然后**使用来将配置文件组合**
>
> **在JavaConfig引用XML**
>
> - **使用@ImportResource()**
>
> **在XML引用JavaConfig**
>
> - **使用 <bean>节点就行了**

##### scope属性

1. 控制是单例的还是多例的
   1. 单例  **singleton** 
      - 在IOC容器之前创建
   2. 多例  **prototype** 
      - 使用的时候才创建

#### 其他属性

1. ### **lazy-init属性**

   - 只有在单例模式singleton下有效，如果为true，则单例会在使用的时候才创建

2. ### init-method和destroy-method

   - 在创建对象之前执行init-method
   - 在IOC容器销毁的时候执行destroy-method
   - 

先把流行的框架再熟悉一遍，然后就可以回头使劲刷基础、剖析源码了。加油！