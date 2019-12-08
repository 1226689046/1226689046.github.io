---

layout: post
title: SpringMVC-Controller
tags:
- spring
categories: spring
description: SpringMVC-Controller
---

# SpringMVC-Controller

对于Controller的知识点，主要有下面几个

> - 编码过滤器
> - 使用注解开发
> - 注解`@RequestMapping`详解
> - 业务方法接收参数
> - 字符串转日期
> - 重定向和转发
> - 返回JSON

#### 编码过滤器

```xml
<filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>
            org.springframework.web.filter.CharacterEncodingFilter
        </filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

 <!-- 注册springmvc框架核心控制器 -->
    <servlet>
        <servlet-name>DispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>

        <!--到类目录下寻找我们的配置文件-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:hello.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>DispatcherServlet</servlet-name>
        <!--映射的路径为.action-->
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>

```

##### 注解的方式开发Controller

1. 开启自动扫描

```xml
<!--    开启自动扫描-->
    <context:component-scan base-package="springmvc.controller"/>
```

2. 将类使用@Controller注解标记

```java
@Controller
public class HelloController {
    @RequestMapping(value = "/req.action")
    public String sayHello(String username,String password){
        System.out.println(username);
        System.out.println(password);
//        System.out.println("aaa");
        return "/index.jsp";
    }
}
```

#### RequestMapping

@RequestMapping可以控制访问路径和请求方式

@Controller也可以添加@RequestMapping，这样就可以有不同的“命名空间”

```java
@RequestMapping(value = "/req.action",method = RequestMethod.POST)
```

#### 接收参数

参数名和传递的参数名对应即可。

甚至可以将封装好的bean放在参数里面，可以直接给封装好。。

还可以封装成list

```java
@RequestMapping(value = "/lst.action",method = RequestMethod.GET)
    public String getStringParamLst(String[] username){
        System.out.println(new ArrayList<String>(Arrays.asList(username)));
//        System.out.println("aaa");
        return "/index.jsp";
    }
```

也可以将list封装成bean的属性。。。





如果想收集不同的bean，比如  room的属性user room的属性admin

那么传递参数的时候就应该使用user.username  admin.username  会自动封装成room





- 字符串转日期

```java
@InitBinder //注解是没有重写方法的，但是可以使用这个注解说明要重写这个方法。
    protected void initBinder(HttpServletRequest request, ServletRequestDataBinder binder) throws Exception {
        binder.registerCustomEditor(
                Date.class,
                new CustomDateEditor(new SimpleDateFormat("yyyy-MM-dd"), true));

    }
```

#### 结果重定向和转发

```java
 @RequestMapping(value = "/textReditect.action")
    public String testRedirect(){
        System.out.println("第一个重定向");
        return "redirect:/two.action";
    }
    @RequestMapping(value = "/two.action")
    public String two(){
        System.out.println("two");
        return "/index.jsp";
    }
```

返回的时候添加一个关键字就可以了