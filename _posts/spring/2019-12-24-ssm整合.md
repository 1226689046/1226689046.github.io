---

layout: post
title: Spring  ioc
tags:
- spring
categories: spring
description: Spring ioc
---

## SpringIoc知识点

IOC：控制和反转。



控制指的是对象内部成员的控制权

反转是  管理权限不给当前对象，而是用第三方容器进行管理



- IoC(思想，设计模式)主要的实现方式有两种：依赖查找，**依赖注入**。
- 依赖注入是一种更可取的方式(实现的方式)



ioc的好处：

> - 不用自己封装
> - 单例，不浪费资源
> - 便于测试
> - 便于进行AOP操作。
> - 统一配置，便于修改

#### 原理

IOC容器就是一个大工厂，管理suo有对象以及依赖关系

> - 原理就是通过Java的**反射技术**来实现的！通过反射我们可以获取类的所有信息(成员变量、类名等等等)！
> - 再通过配置文件(xml)或者注解来**描述**类与类之间的关系
> - 我们就可以通过这些配置信息和反射技术来**构建**出对应的对象和依赖关系了！



1. 根据Bean的注册信息，在容器内部创建Bean定义注册表
2. 根据注册表 加载、实例化Bean 创建之间的依赖关系
3. 将准备就绪的Bean放在Map缓存池中，等待调用。

> Spring容器(Bean工厂)可简单分成两种：
>
> - BeanFactory
>
> - - 这是最基础、面向Spring的
>
> - ApplicationContext
>
> - - 这是在BeanFactory基础之上，面向使用Spring框架的开发者。提供了一系列的功能！
>
> 几乎所有的应用场合**都是**使用ApplicationContext！





> - **Bean自身的方法**：如调用 Bean 构造函数实例化 Bean，调用 Setter 设置 Bean 的属性值以及通过
>
>   的 init-method 和 destroy-method 所指定的方法；
>
> - **Bean级生命周期接口方法**：如 BeanNameAware、 BeanFactoryAware、 InitializingBean 和 DisposableBean，这些接口方法由 Bean 类直接实现；
>
> - **容器级生命周期接口方法**：在上图中带“★” 的步骤是由 InstantiationAwareBean PostProcessor 和 BeanPostProcessor 这两个接口实现，一般称它们的实现类为“ **后处理器**” 。 后处理器接口一般不由 Bean 本身实现，它们独立于 Bean，实现类以容器附加装置的形式注册到Spring容器中并通过接口反射为Spring容器预先识别。当Spring 容器创建任何 Bean 的时候，这些后处理器都会发生作用，所以这些后处理器的影响是全局性的。当然，用户可以通过合理地编写后处理器，让其仅对感兴趣Bean 进行加工处理





> - XML配置
> - 注解
> - JavaConfig
> - 基于Groovy DSL配置(这种很少见)



### 依赖注入

> - **属性注入**-->通过`setter()`方法注入
> - 构造函数注入
> - 工厂方法注入





一些面试题：

## 2.1什么是spring?

> 什么是spring?

Spring 是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring框架**目标是简化Java企业级应用开发**，并通过POJO为基础的编程模型促进良好的编程习惯。

## 2.2使用Spring框架的好处是什么？

> 使用Spring框架的好处是什么？

- **轻量**：Spring 是轻量的，基本的版本大约2MB。
- **控制反转**：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
- **面向切面的编程**(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
- **容器**：Spring 包含并管理应用中对象的生命周期和配置。
- **MVC框架**：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
- **事务管理**：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
- **异常处理**：Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

## 2.3Spring由哪些模块组成?

> Spring由哪些模块组成?

简单可以分成6大模块：

- Core
- AOP
- ORM
- DAO
- Web
- Spring EE



## 2.4BeanFactory 实现举例

> BeanFactory 实现举例

Bean工厂是工厂模式的一个实现，提供了控制反转功能，**用来把应用的配置和依赖从正真的应用代码中分离**。

在spring3.2之前最常用的是XmlBeanFactory的，但现在被废弃了，取而代之的是：XmlBeanDefinitionReader和DefaultListableBeanFactory

## 2.5什么是Spring的依赖注入？

> 什么是Spring的依赖注入？

依赖注入，是IOC的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。你**不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务**，之后一个容器（IOC容器）负责把他们组装起来。

## 2.6有哪些不同类型的IOC（依赖注入）方式？

> 有哪些不同类型的IOC（依赖注入）方式？

- **构造器依赖注入**：构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
- **Setter方法注入**：Setter方法注入是容器通过调用无参构造器或无参static工厂 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。
- 工厂注入：这个是遗留下来的，很少用的了！

## 2.7哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？

> 哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？

你两种依赖方式都可以使用，构造器注入和Setter方法注入。最好的解决方案是**用构造器参数实现强制依赖，setter方法实现可选依赖**。

## 2.8什么是Spring beans?

> 什么是Spring beans?

Spring beans 是那些**形成Spring应用的主干的java对象**。它们被Spring IOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中``的形式定义。

这里有四种重要的方法给Spring容器**提供配置元数据**。

- XML配置文件。
- 基于注解的配置。
- 基于java的配置。
- Groovy DSL配置

## 2.9解释Spring框架中bean的生命周期

> 解释Spring框架中bean的生命周期

- Spring容器 从XML 文件中读取bean的定义，并实例化bean。
- Spring根据bean的定义填充所有的属性。
- 如果bean实现了BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
- 如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
- 如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
- 如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
- 如果有BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用。
- 如果bean实现了 DisposableBean，它将调用destroy()方法。

## 2.10解释不同方式的自动装配

> 解释不同方式的自动装配

- no：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。
- byName：通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
- byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。
- constructor：这个方式类似于byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。
- autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。

只用注解的方式时，**注解默认是使用byType的**！

## 2.11IOC的优点是什么？

> IOC的优点是什么？

IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。**最小的代价和最小的侵入性使松散耦合得以实现**。IOC容器支持加载服务时的**饿汉式初始化和懒加载**。

## 2.12哪些是重要的bean生命周期方法？ 你能重载它们吗？

> 哪些是重要的bean生命周期方法？ 你能重载它们吗？

有两个重要的bean 生命周期方法，第一个是`setup`， 它是在容器加载bean的时候被调用。第二个方法是 `teardown` 它是在容器卸载类的时候被调用。

The bean 标签有两个重要的属性（`init-method`和`destroy-method`）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（`@PostConstruct`和`@PreDestroy`）。

## 2.13怎么回答面试官：你对Spring的理解？

> 怎么回答面试官：你对Spring的理解？

来源：

- https://www.zhihu.com/question/48427693?sort=created



## 2.14Spring框架中的单例Beans是线程安全的么？

> Spring框架中的单例Beans是线程安全的么？

Spring框架并没有对单例bean进行任何多线程的封装处理。关于单例bean的线程安全和并发问题需要开发者自行去搞定。但实际上，大部分的Spring bean并没有可变的状态(比如Serview类和DAO类)，所以在某种程度上说Spring的单例bean是线程安全的。**如果你的bean有多种状态的话**（比如 View Model 对象），就**需要自行保证线程安全**。

最浅显的解决办法就是将多态bean的作用域由“singleton”变更为“prototype”

## 2.15FileSystemResource和ClassPathResource有何区别？

> FileSystemResource和ClassPathResource有何区别？

在FileSystemResource 中需要给出spring-config.xml文件在你项目中的相对路径或者绝对路径。在ClassPathResource中spring会在ClassPath中自动搜寻配置文件，所以要把ClassPathResource文件放在ClassPath下。

如果将spring-config.xml保存在了src文件夹下的话，只需给出配置文件的名称即可，因为src文件夹是默认。

简而言之，**ClassPathResource在环境变量中读取配置文件，FileSystemResource在配置文件中读取配置文件**。