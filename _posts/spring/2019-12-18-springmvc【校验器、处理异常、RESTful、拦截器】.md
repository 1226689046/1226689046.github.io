---

layout: post
title: SpringMVC
tags:
- spring
categories: spring
description: SpringMVC
---

## SpringMVC【校验器、统一处理异常、RESTful、拦截器】

#### 一、校验器

1. jar包

   

```xml
<dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>6.1.0.Final</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.jboss.logging/jboss-logging -->
        <dependency>
            <groupId>org.jboss.logging</groupId>
            <artifactId>jboss-logging</artifactId>
            <version>3.4.1.Final</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/javax.validation/validation-api -->
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>2.0.1.Final</version>
        </dependency>
```

##### 配置校验器

```xml

<!--    校验器配置区域-->
    <bean id="validator" class="org.springframework.validation.beanvalidation.LocalValidatorFactoryBean">
<!--        校验器-->
        <property name="providerClass" value="org.hibernate.validator.HibernateValidator"/>
        <property name="validationMessageSource" ref="messageSource"/>
    </bean>




<!--    错误信息-->
    <bean id="messageSource" class="org.springframework.context.support.ReloadableResourceBundleMessageSource">
<!--        资源文件-->
        <property name="basenames">
            <list>
                <value>classpath:CustomValidationMessages</value>
            </list>
        </property>
        <property name="fileEncodings" value="utf-8"/>
<!--        缓存时间-->
        <property name="cacheSeconds" value="120"/>
    </bean>




然后定义webBinder
 <!--    定义webBinder-->
        <bean id="customBinder" class="org.springframework.web.bind.support.ConfigurableWebBindingInitializer">
            <property name="conversionService" ref="converRegister"/>


<!--            将校验器添加到webBinder中
个人感觉这个方法是启动的时候会调用。。这个类-->
            <property name="validator" ref="validator"/>
        </bean>




将WebBinder添加到适配器中
<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"
          p:ignoreDefaultModelOnRedirect="true">
        <property name="messageConverters">
            <list>
                <bean class="org.springframework.http.converter.json.MappingJackson2HttpMessageConverter"/>
            </list>
        </property>
        <property name="webBindingInitializer" ref="customBinder"/>
    </bean>
```

在resource文件夹下搞定CustomValidationMessages文件

```
items.name.length.error=长度不匹配
items.cratetime.is.notnull=不可以为空
```

在bean中，可以这样写

```java
package parambind;

import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
import java.util.Date;

public class GoodBean {
    private Integer id;
    //商品名称的长度请限制在1到30个字符
    @Size(min = 1, max = 30, message = "{items.name.length.error}")
    private String name;

    private Float price;

    private String pic;

    //请输入商品生产日期
    @NotNull(message = "{items.createtime.is.notnull}")
    private Date createtime;

    private String detail;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name == null ? null : name.trim();
    }

    public Float getPrice() {
        return price;
    }

    public void setPrice(Float price) {
        this.price = price;
    }

    public String getPic() {
        return pic;
    }

    public void setPic(String pic) {
        this.pic = pic == null ? null : pic.trim();
    }

    public Date getCreatetime() {
        return createtime;
    }

    public void setCreatetime(Date createtime) {
        this.createtime = createtime;
    }

    public String getDetail() {
        return detail;
    }

    public void setDetail(String detail) {
        this.detail = detail == null ? null : detail.trim();
    }

    @Override
    public String toString() {
        return "GoodBean{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", price=" + price +
                ", pic='" + pic + '\'' +
                ", createtime=" + createtime +
                ", detail='" + detail + '\'' +
                '}';
    }
}

```

在需要校验的字段上，都打上注解，然后使用message来获取上面那个文件中的文件信息





##### 分组校验

分组校验使得校验方式更加灵活。有时候并不需要把当前配置的所有的属性都给校验，而是当前方法执行某些校验。



步骤：

- 定义分组的接口【主要是标识】
- 定义校验规则用于哪一组
- 在Controller中使用校验分组

首先，写一个接口：

```java
//用于校验某某组
public interface ValidateGroup {
}

```

该接口主要是为了做一个标志，谁谁谁属于这个组。

第二步，在属性值上比如@NotNull中加上group={ValidateGroup.class}

第三步，在@Validated中加上 value={ValidateGroup.class}

这样就可以完成分组校验





### 二、统一异常处理类

实现处理类

```java

public class CustomExceptionHandler implements HandlerExceptionResolver {

    /*
        http请求->dispatcherServlet->handler(controller)->service->mapper  异常全都向上抛出。之后会经过统一异常处理类(实现HandlerExceptionResolver接口)
     */
    //前端控制器DispatcherServlet在进行HandlerMapping、调用HandlerAdapter执行Handler过程中，如果遇到异常就会执行此方法
    //handler最终要执行的Handler，它的真实身份是HandlerMethod
    //Exception ex就是接收到异常信息

    @Override
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {
        e.printStackTrace();
        System.out.println("出错了");
        return new ModelAndView();
    }
}

```

配置统一异常处理器

```xml
<bean class="parambind.CustomExceptionHandler"/>
```





#### 三、RESTful

- **每一个URI代表一种资源；**
- **客户端和服务器之间，传递这种资源的某种表现层；**
- **客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"**。

这种开发理念是对http的非常好的诠释。

非RESTful   http://localhost/aaa/t.action?id=1

RESTful       http://localhost/aaa/t/1

使用这种风格（理念）步骤：

1. 更改requestDispatcher  以前是actioin  现在要拦截所有的。

   ```xml
   <!-- 注册springmvc框架核心控制器 -->
       <servlet>
           <servlet-name>DispatcherServlet</servlet-name>
           <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
   
           <!--到类目录下寻找我们的配置文件-->
           <init-param>
               <param-name>contextConfigLocation</param-name>
               <param-value>classpath:restful.xml</param-value>
           </init-param>
       </servlet>
       <servlet-mapping>
           <servlet-name>DispatcherServlet</servlet-name>
           <!--映射的路径为.action-->
           <url-pattern>/</url-pattern>
       </servlet-mapping>
   ```

2. Controller这样写

   ```java
   @Controller
   @RequestMapping("/rest")
   public class RestFulController {
   
       @RequestMapping("/user/{id}")
       @ResponseBody
       public String t(@PathVariable("id")String id){
           System.out.println(id);
           return id;
       }
   }
   
   ```

3. **当DispatcherServlet拦截/开头的所有请求，对静态资源的访问就报错：我们需要配置对静态资源的解析**

```xml
    <mvc:resources location="/js/" mapping="/js/**" />
    <mvc:resources location="/img/" mapping="/img/**" />
```

`/**`就表示不管有多少层，都对其进行解析，`/*`代表的是当前层的所有资源..





### 四、Spring拦截器

SpringMVC也有拦截器的。

**用户请求到DispatherServlet中，DispatherServlet调用HandlerMapping查找Handler，HandlerMapping返回一个拦截的链儿（多个拦截），springmvc中的拦截器是通过HandlerMapping发起的。**



```xml
<!--    配置拦截器-->
    <mvc:interceptors>
        <!--        配置多个拦截器，将会按照顺序执行-->
        <mvc:interceptor>
            <mvc:mapping path="/**"/>
            <bean class="restful.CustomHandlerInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>
```

```java

public class CustomHandlerInterceptor implements HandlerInterceptor {

    //在执行Handler之前，用于进行用户的校验
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //如果返回false则拦截，不继续执行handler
        System.out.println("preHandle");
        //如果返回true则直接放行
        return false;
    }

    //在执行handler返回ModelAndView之前。
    //如果需要向页面提供一些公用 的数据或配置一些视图信息，使用此方法实现 从modelAndView入手
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandler");
    }

    //执行handler之后执行此方法
    //主要进行日志的操作，进行方法执行性能监控，在preHandle中设置一个时间点，在afterCompletion设置一个时间，两个时间点的差就是执行时长
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}

```

注意：需要添加

```xml
<mvc:annotation-driven/>
```

注解，否则会失效。

