---

layout: post
title: Springboot
tags:
- spring
categories: spring
description: Springboot
---

## SpringBoot

#### 一、SpringBoot启动



- 在idea中直接使用启动（最常用）
- 使用mvn 命令来启动
- 使用mvn编译，而后在class目录生成jar包，使用Java命令来启动



#### 二、项目属性配置

SpringBoot中集成了tomcat，可以使用properties文件 也可以使用yml文件。

创建项目：选择Spring init.... 就是SpringBoot的项目。选择web和mysql等

注意，所有的包都要放在SpringbootdemoApplication这个文件的目录下

配置文件

yml文件：

```properties
server:
  port: 80
  servlet:
    context-path: /springboot
#注意一定要加空格。这个是在配置文件里面声明配置信息（常量）
mixmoney: 1




server:
  port: 80
  servlet:
    context-path: /springboot

limit:
  mixmoney: 2
  desc: 最少要发${limit.mixmoney}元


```

在相关类上使用

```java
//读取配置文件，使用前缀为...的。也可以使用@Value()来读取
@Component
@ConfigurationProperties(prefix = "limit")
public class LimitConfig {
    private BigDecimal mixmoney;
    private String desc;

    public BigDecimal getMixmoney() {
        return mixmoney;
    }

    public void setMixmoney(BigDecimal mixmoney) {
        this.mixmoney = mixmoney;
    }

    public String getDesc() {
        return desc;
    }

    public void setDesc(String desc) {
        this.desc = desc;
    }

    @Override
    public String toString() {
        return "LimitConfig{" +
                "mixmoney=" + mixmoney +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```



application-dev.yml：开发的环境

application-prod.yml：生产环境

这个地方都是在application.yml来指定的

```properties
spring:
  profiles:
    active: dev
```

在使用jar包运行的时候，可以进行传参：

java -jar -Dspring.profiles.active=prod ......





可以切换到目录下，使用 

mvs:spring-boot:run  命令来启动

打包：

mvn:clean package  在taget目录下会生成一个jar文件





##### 模版渲染

thymeleaf



```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```



```java
@Controller + @ResponseBody  = @RestController
 
```

@RequestMapping  @GetMapping等都是可以传递数组的

```java
 @RequestMapping({"/test","/test1"})
    public String test(){
        return "index";
    }
```

# SpringData jpa

定义了一系列持久化的标准。只是定义了一系列接口，只是一个标准而已，并没有做任何事情.springboot jpa 就是对hibernate的整合，不用写一行sql语句。



- GET    /users    获取用户列表
- POST  /users  创建一个用户
- PUT   /user/id  更改用户信息

在Springboot中使用：

先引入依赖

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>
```

配置信息:

```properties
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/springtest
    username: root
    password: 123456wsr

  jpa:
    hibernate:
      ddl-auto: create   #这个是初始化的时候执行的是更新表的操作还是创建表的操作
    show-sql: true

```

与数据库表映射的类

```java
package com.example.springbootdemo;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import java.math.BigDecimal;

@Entity
public class Luckymoney {
    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public BigDecimal getMoney() {
        return money;
    }

    public void setMoney(BigDecimal money) {
        this.money = money;
    }

    public String getSender() {
        return sender;
    }

    public void setSender(String sender) {
        this.sender = sender;
    }

    public String getReceiver() {
        return receiver;
    }

    public void setReceiver(String receiver) {
        this.receiver = receiver;
    }

    public Luckymoney(Integer id, BigDecimal money, String sender, String receiver) {
        this.id = id;
        this.money = money;
        this.sender = sender;
        this.receiver = receiver;
    }

    @Id
    @GeneratedValue
    private Integer id;
    private BigDecimal money;
    private String sender;
    private String receiver;
}

```

注意：报错You must configure either the server or JDBC driver (via the serverTimezone configuration property) to use a more specifc time zone value if you want to utilize time zone support.

需要添加时区?serverTimezone=UTC



创建一个dao  接口，里面有很多东西

```java
import com.example.springbootdemo.Luckymoney;
import org.springframework.data.jpa.repository.JpaRepository;

public interface LuckMoneyDao extends JpaRepository<Luckymoney,Integer> {

}

```

继承自JpaRepository，两个泛型，第一个是实体类，第二个是主键的类型

里面封装了很多，不需要写任何sql语句就可以实现查询，很方便。

```java

@RestController
public class LuckyMoneyController {

    @Autowired
    private LuckMoneyDao luckMoneyDao;
    /**
     *
     */
    @GetMapping("/getLms")
    public List<Luckymoney> lst(){
        List<Luckymoney> lst = luckMoneyDao.findAll();
        return lst;
    }

    /**
     * 创建红包
     */
    @PostMapping("/luckymoneys")
    public Luckymoney create(@RequestParam("sender") String sender, @RequestParam("money")BigDecimal money){

        Luckymoney luckymoney = new Luckymoney();
        luckymoney.setSender(sender);
        luckymoney.setMoney(money);
        return luckMoneyDao.save(luckymoney);
    }

    @GetMapping("/luckmoneys/{id}")
    public Luckymoney findById(@PathVariable("id")Integer id){
        return luckMoneyDao.findById(id).orElse(null);
//        return luckMoneyDao.getOne(id);
    }
}

```

#### 三、优点







简化配置；入门级微框架

