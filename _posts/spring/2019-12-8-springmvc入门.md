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

- ModelAndView就是将试图路径和数据封装起来 ，想跳转到哪个网页，需要传递什么数据，都写在这个类里。