---
layout: post
title: Spring DAO模块知识要点
tags:
- spring
categories: spring
description: SpringDAO模块
---

# Spring的DAO模块

Spring有良好的对jdbc的支持。

Spring的jdbc，需要导入spring-jdbc spring-tx包。

#### Spring使用c3p0连接池

```xml
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>  
    <property name="jdbcUrl" value="jdbc:mysql:///ccbprj"/> 
    <property name="user" value="root"/> 
    <property name="password" value="123456wsr"/>   
    <property name="initialPoolSize" value="3"/> 
    <property name="maxPoolSize" value="10"/>   
    <property name="maxStatements" value="100"/>   
    <property name="acquireIncrement" value="2"/>
</bean>
```

- c3p0常用的配置

- ```xml
   <!--acquireIncrement：链接用完了自动增量3个。 -->
      <property name="acquireIncrement">3</property>
  
      <!--acquireRetryAttempts：链接失败后重新试30次。-->
      <property name="acquireRetryAttempts">30</property>
   
      <!--acquireRetryDelay；两次连接中间隔1000毫秒。 -->
      <property name="acquireRetryDelay">1000</property>
   
      <!--autoCommitOnClose：连接关闭时默认将所有未提交的操作回滚。 -->
      <property name="autoCommitOnClose">false</property>
   
      <!--automaticTestTable：c3p0测试表，没什么用。-->
      <property name="automaticTestTable">Test</property>
   
      <!--breakAfterAcquireFailure：出错时不把正在提交的数据抛弃。-->
      <property name="breakAfterAcquireFailure">false</property>
   
      <!--checkoutTimeout：100毫秒后如果sql数据没有执行完将会报错，如果设置成0，那么将会无限的等待。 --> 
      <property name="checkoutTimeout">100</property>
   
      <!--connectionTesterClassName：通过实现ConnectionTester或QueryConnectionTester的类来测试连接。类名需制定全路径。Default: com.mchange.v2.c3p0.impl.DefaultConnectionTester-->
      <property name="connectionTesterClassName"></property>
   
      <!--factoryClassLocation：指定c3p0 libraries的路径，如果（通常都是这样）在本地即可获得那么无需设置，默认null即可。-->
      <property name="factoryClassLocation">null</property>
   
      <!--forceIgnoreUnresolvedTransactions：作者强烈建议不使用的一个属性。--> 
      <property name="forceIgnoreUnresolvedTransactions">false</property>
   
      <!--idleConnectionTestPeriod：每60秒检查所有连接池中的空闲连接。--> 
      <property name="idleConnectionTestPeriod">60</property>
   
      <!--initialPoolSize：初始化时获取三个连接，取值应在minPoolSize与maxPoolSize之间。 --> 
      <property name="initialPoolSize">3</property>
   
      <!--maxIdleTime：最大空闲时间,60秒内未使用则连接被丢弃。若为0则永不丢弃。-->
      <property name="maxIdleTime">60</property>
   
      <!--maxPoolSize：连接池中保留的最大连接数。 -->
      <property name="maxPoolSize">15</property>
   
      <!--maxStatements：最大链接数。-->
      <property name="maxStatements">100</property>
   
      <!--maxStatementsPerConnection：定义了连接池内单个连接所拥有的最大缓存statements数。Default: 0  -->
      <property name="maxStatementsPerConnection"></property>
   
      <!--numHelperThreads：异步操作，提升性能通过多线程实现多个操作同时被执行。Default: 3--> 
      <property name="numHelperThreads">3</property>
   
      <!--overrideDefaultUser：当用户调用getConnection()时使root用户成为去获取连接的用户。主要用于连接池连接非c3p0的数据源时。Default: null--> 
      <property name="overrideDefaultUser">root</property>
   
      <!--overrideDefaultPassword：与overrideDefaultUser参数对应使用的一个参数。Default: null-->
      <property name="overrideDefaultPassword">password</property>
   
      <!--password：密码。Default: null--> 
      <property name="password"></property>
   
      <!--preferredTestQuery：定义所有连接测试都执行的测试语句。在使用连接测试的情况下这个一显著提高测试速度。注意： 测试的表必须在初始数据源的时候就存在。Default: null-->
      <property name="preferredTestQuery">select id from test where id=1</property>
   
      <!--propertyCycle：用户修改系统配置参数执行前最多等待300秒。Default: 300 --> 
      <property name="propertyCycle">300</property>
   
      <!--testConnectionOnCheckout：因性能消耗大请只在需要的时候使用它。Default: false -->
      <property name="testConnectionOnCheckout">false</property>
   
      <!--testConnectionOnCheckin：如果设为true那么在取得连接的同时将校验连接的有效性。Default: false -->
      <property name="testConnectionOnCheckin">true</property>
   
      <!--user：用户名。Default: null-->
      <property name="user">root</property>
   
      <!--usesTraditionalReflectiveProxies：动态反射代理。Default: false-->
      <property name="usesTraditionalReflectiveProxies">false</property>
  ```

  

```java
@Autowiredprivate 
DataSource dataSource;
public void setDataSource(DataSource dataSource) {   
    this.dataSource = dataSource;
}
@Overridepublic void save() {  
    System.out.println("存储数据");   
    String sql = "insert into wx_login values(1,1,1,'2019-12-06',1)"; 
    try {     
        Connection c = dataSource.getConnection();      
        Statement stmt = c.createStatement();   
        stmt.execute(sql);      
        stmt.close();     
        c.close();   
    } catch (SQLException e) { 
        e.printStackTrace();  
    }
}
```

直接使用连接池 DateSource获取连接。

#### 使用Spring的JdbcTemplate

```xml
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">    <property name="dataSource" ref="dataSource"/>
</bean>
```

```java
@Autowired
private JdbcTemplate jdbcTemplate;
```

##### 使用jdbcTemplate查询

jdbcTemplate封装了很多query方法。

```java
jdbcTemplate.query("select * from wx_login",new RowMapper<User>(){ 
    @Override   
    public User mapRow(ResultSet rs, int rowNum) throws SQLException {   
        User u = new User();  
        u.setUsername(rs.getString("username"));  
        return u; 
    }
});
```

#### Spring的事务

一般来讲，事务的控制都是从service层做，service是业务逻辑层，一旦执行成功，说明该功能没错。

一个service会调用多个dao，dao并不知道是否完全正确。

Spring的事务控制是数据Spring Dao模块的。

- 事务控制的两张方式

  - 编程式事务控制

  - 声明式事务控制

##### 编程式事务

自己动手去控制，就叫【编程式事务】

jdbc:

conn.setAutoCommite(false);//设置手动控制事务

- **【细粒度的事务控制： 可以对指定的方法、指定的方法的某几行添加事务控制】**
- **(比较灵活，但开发起来比较繁琐： 每次都要开启、提交、回滚.)**

##### 声明式事务

在Spring中，提供的事务控制就是声明式事务

Spring提供了对事务控制的实现。

- 如果用户想要使用Spring的事务控制，**只需要配置就行了**。

- 当不用Spring事务的时候，直接移除就行了。

- Spring的事务控制是**基于AOP实现的**。因此它的**耦合度是非常低**的。

- 【粗粒度的事务控制： **只能给整个方法应用事务，不可以对方法的某几行应用事务。**】

- - (因为aop拦截的是方法。)

**Spring给我们提供了事务的管理器类**，事务管理器类又分为两种，因为**JDBC的事务和Hibernate的事务是不一样的**。

- Spring声明式事务管理器类：

- - Jdbc技术：DataSourceTransactionManager
  - Hibernate技术：HibernateTransactionManager

1. xml配置spring控制事务

   > 第一步：配置事务管理器
   >
   > ```xml
   > <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">   
   >     <property name="dataSource" ref="dataSource"/>
   > </bean>
   > ```
   >
   > 第二步：配置管理方式
   >
   > ```xml
   > <tx:advice id="txAdvice" transaction-manager="txManager">       
   >     <tx:attributes>
   >         <!--            配置所有的方法，并不是只读-->          
   >         <tx:method name="*" read-only="false"/>     
   >     </tx:attributes>  
   > </tx:advice>
   > ```
   >
   > 第三步：配置切入点
   >
   > ```xml
   > <aop:config>   
   >     <aop:pointcut id="pt" expression="execution(* aop.UserService.*(..))"/>  
   >     <aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
   > </aop:config>
   > ```
   >
   > 这样就可以完成事务管理。如果执行的方法报错，就会回滚

2. 以注解的方式配置Spring事务

   > 第一步和XML的是一样的，**必须配置事务管理器类：**
   >
   > ```xml
   >     <!--1.配置事务的管理器类:JDBC-->   
   > <bean id="txManage" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">   
   >     <!--引用数据库连接池-->        
   >     <property name="dataSource" ref="dataSource"/> 
   > </bean>
   > ```
   >
   > 第二步：开启以注解的方式来实现事务控制
   >
   > ```xml
   >     <!--开启以注解的方式实现事务控制-->   
   > <tx:annotation-driven transaction-manager="txManage"/>
   > ```
   >
   > 最后，**想要控制哪个方法事务，在其前面添加@Transactional这个注解就行了！**如果想要控制整个类的事务，那么在类上面添加就行了。
   >
   > 

   ##### 事务属性

   