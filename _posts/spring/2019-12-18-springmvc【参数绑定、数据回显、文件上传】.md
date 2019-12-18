---

layout: post
title: SpringMVC
tags:
- spring
categories: spring
description: SpringMVC
---

# SpringMVC 【数据绑定、参数回显、文件上传】

Spring中默认支持的参数类型：

- **HttpServletRequest**
- **HttpServletResponse**
- **HttpSession**
- **Model**

需要进行自定义参数绑定的地方：日期类型的转换等等。

## 自定义参数转换器 **Converter**

1. 配置日期转换器

   想要完成什么功能，就直接在重载方法里写就可以了。并且支持泛型。

   ```java
   public class CustomDateConver implements Converter<String, Date> {
       @Override
       public Date convert(String source) {
           try {
               return new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse(source);
           } catch (ParseException e) {
               e.printStackTrace();
           }
           return null;
       }
   }
   ```

2. 配置转换器

   转换器声明了写的所有转换器conver，使用一个工厂bean的参数

   webbinder 将请求绑定在转换器上

   适配器是

```xml
<!--转换器-->
    <bean id="converRegister" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <list>
                <bean class="parambind.CustomDateConver"/>
                <bean class="parambind.StringTrimConver"/>
            </list>
        </property>
    </bean>
<!--    定义webBinder-->
    <bean id="customBinder" class="org.springframework.web.bind.support.ConfigurableWebBindingInitializer">
        <property name="conversionService" ref="converRegister"/>
    </bean>

<!--    注解适配器-->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter">
        <property name="webBindingInitializer" ref="customBinder"/>
    </bean>
```

如果是使用driven 则如此：

```xml
<mvc:annotation-driven conversion-service="conversionService">
    </mvc:annotation-driven>
    <!-- conversionService -->
    <bean id="conversionService"
          class="org.springframework.context.support.ConversionServiceFactoryBean">
        <!-- 转换器 -->
        <property name="converters">
            <list>
                <bean class="parambind.CustomDateConver"/>
                <bean class="parambind.StringTrimConver"/>
            </list>
        </property>
    </bean>
```

注意，像RequestMappingHandlerAdapter和mvc:annotation-driven这样的，只需要声明一个。需要进行合并。

一直没有走转换器的原因就是配置了两个注解适配器。，上一个注解适配器是为@ResponseBody准备的。

这种情况会自动匹配Converter<String,Date> 如果来源是String,目标是Date，则会自动经过这个转换器。



#### 文件上传

在SpringMVC中文件上传需要用到的jar包

- **commons-fileupload-1.2.2.jar**
- **commons-io-2.4.jar**

1. 配置上传解析器**id必须为这个名字，否则会报错**

   ```xml
   
       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
           <property name="maxUploadSize">
               <value>5242880</value>
           </property>
       </bean>
   ```

   

2. ```java
    @RequestMapping("/upload.action")
       public void upload(MultipartFile picture){
           System.out.println(getClass().getClassLoader().getResource("/").getPath());
           try {
               picture.transferTo(new File("E:\\blog\\miemiemie\\springstudy\\img\\pic.txt"));
           } catch (IOException e) {
               e.printStackTrace();
           }
       }
   ```

   使用MultipartFile来进行获取。注意：参数名称和Input的name必须是一样的。

3. form表单，注意enctype

4. 

5. ```html
   <form action="/upload.action"  method="post" enctype="multipart/form-data">
       <input type="file" name="picture">
       <input type="submit" value="提交">
   </form>
   ```

