---

layout: post
title: SpringMVC入门
tags:
- spring
categories: spring
description: SpringMVC入门
---

# Spring MVC入门

1. 相关包

   > **前6个是Spring的核心功能包【IOC】，第7个是关于web的包，第8个是SpringMVC包**
   >
   > - org.springframework.context-3.0.5.RELEASE.jar
   > - org.springframework.expression-3.0.5.RELEASE.jar
   > - org.springframework.core-3.0.5.RELEASE.jar
   > - org.springframework.beans-3.0.5.RELEASE.jar
   > - org.springframework.asm-3.0.5.RELEASE.jar
   > - commons-logging.jar
   > - org.springframework.web-3.0.5.RELEASE.jar
   > - org.springframework.web.servlet-3.0.5.RELEASE.jar

继承Controller类，就完成了创建

```java
import org.springframework.web.servlet.ModelAndView;
import org.springframework.web.servlet.mvc.Controller;
public class HelloAction implements Controller { 
    @Override   
    public ModelAndView handleRequest(javax.servlet.http.HttpServletRequest httpServletRequest, javax.servlet.http.HttpServletResponse httpServletResponse) throws Exception {    
        
        ModelAndView md = new ModelAndView();
        md.setViewName("/hello.jsp");
        return md;
    }
}
```

配置文件hello.xml，就是配置一下Controller

```xml
<bean class="springmvc.controller.HelloAction" name="/hello.action"/>
```

配置web.xml

```xml
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

- ModelAndView就是将试图路径和数据封装起来 ，想跳转到哪个网页，需要传递什么数据，都写在这个类里。	页面放置在webapp下就可以

### SpringMVC的工作流程

- **用户发送请求**
- **请求交由核心控制器处理**
- **核心控制器找到映射器，映射器看看请求路径是什么**
- **核心控制器再找到适配器，看看有哪些类实现了Controller接口或者对应的bean对象**
- **将带过来的数据进行转换，格式化等等操作**
- **找到我们的控制器Action，处理完业务之后返回一个ModelAndView对象**
- **最后通过视图解析器来对ModelAndView进行解析**
- **跳转到对应的JSP/html页面**

#### 映射器

映射器表示的是控制器与访问地址的映射关系。

映射器的默认值是这样的

```xml
<!-- 控制类 -->
<bean class="springmvc.controller.HelloAction" id="helloAction"/>
<!-- 注册映射器(handler包)(框架)【可省略】 -->
 <bean class="org.springframework.web.servlet.handler.SimpleUrlHandlerMapping">
        <property name="mappings">
            <props>
                <prop key="/idx.action">helloAction</prop>
                <prop key="/idx1.action">helloAction</prop>
            </props>
        </property>
    </bean>
```

#### 适配器

```xml
<!-- 适配器【可省略】 -->
<bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"></bean>
```

#### 视图解析器

将结果封装成一个ModelAndView，Spring会使用视图解析器来解析这个ModelAndView

比如，上面的例子中，ModelAndView 的 setViewName("")设置的是一个路径。若返回一个字符串，就会出问题（404）。所以使用视图解析器配置前缀和后缀。

```xml
<!--    视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```

配置好视图解析器之后，就可以返回一个字符串 <!-- 前缀+视图逻辑名+后缀=真实路径 -->

#### 接收参数

