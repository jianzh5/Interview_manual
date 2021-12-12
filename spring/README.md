### 1. Spring Framework 中有多少个模块，它们分别是什么？

![image-20210719083608960](http://img.javadaily.cn/image-20210719083608960.png)

Spring 核心容器 – 该层基本上是 Spring Framework 的核心。它包含以下模块：

* Spring Core

* Spring Bean
* SpEL (Spring Expression Language)
* Spring Context

数据访问/集成 – 该层提供与数据库交互的支持。它包含以下模块：

* JDBC (Java DataBase Connectivity)
* ORM (Object Relational Mapping)
* OXM (Object XML Mappers)
* JMS (Java Messaging Service)
* Transaction

Web – 该层提供了创建 Web 应用程序的支持。它包含以下模块：

* Web
* Web – Servlet
* Web – Socket
* Web – Portlet

AOP - 该层支持面向切面编程

Instrumentation- 该层为类检测和类加载器实现提供支持。

Test - 该层为使用 JUnit 和 TestNG 进行测试提供支持。

Messaging – 该模块为 STOMP 提供支持。它还支持注解编程模型，该模型用于从 WebSocket 客户端路由和处理 STOMP 消息。

Aspects – 该模块为与 AspectJ 的集成提供支持。





### 2. 什么是 Spring IOC 容器？

Spring 框架的核心是 Spring 容器。容器创建对象，将它们装配在一起，配置它们并管理它们的完整生命周期。Spring 容器使用依赖注入来管理组成应用程序的组件。容器通过读取提供的配置元数据来接收对象进行实例化，配置和组装的指令。该元数据可以通过 XML，Java 注解或 Java 代码提供。

![image-20210719084334710](http://img.javadaily.cn/image-20210719084334710.png)





### 3. 什么是依赖注入？

在依赖注入中，您不必创建对象，但必须描述如何创建它们。您不是直接在代码 中将组件和服务连接在一起，而是描述配置文件中哪些组件需要哪些服务。由 IoC 容器将它们装配在一起。





### 4. 可以通过多少种方式完成依赖注入？

通常，依赖注入可以通过三种方式完成，即：

* 构造函数注入

* setter 注入
* 接口注入

在Spring Framework 中，仅使用构造函数和 setter 注入。





### 5. 构造函数注入和 setter 注入的区别

![image-20210719084515672](http://img.javadaily.cn/image-20210719084515672.png)





### 6. spring 中有多少种 IOC 容器？

BeanFactory - BeanFactory 就像一个包含 bean 集合的工厂类。它会在客户端要求时实例化 bean。

ApplicationContext - ApplicationContext 接口扩展了 BeanFactory 接口。它在BeanFactory 基础上提供了一些额外的功能。





### 7. BeanFactory 和 ApplicationContext的区别

![image-20210719084605442](http://img.javadaily.cn/image-20210719084605442.png)





### 8. 列举 IoC 的一些好处。

IoC 的一些好处是：

* 它将最小化应用程序中的代码量。
* 它将使您的应用程序易于测试，因为它不需要单元测试用例中的任何单例或 JNDI 查找机制。
* 它以最小的影响和最少的侵入机制促进松耦合。
* 它支持即时的实例化和延迟加载服务。





### 9. Spring IoC 的实现机制。

Spring 中的 IoC 的实现原理就是工厂模式加反射机制。 示例：

```java
interface Fruit {
	public abstract void eat();
}

class Apple implements Fruit{ 
	public void eat(){
		System.out.println("Apple");
	}
}

class Orange implements Fruit{ 
	public void eat(){
		System.out.println("Orange");
	}
}

class Factory {
	public static Fruit getInstance(String ClassName){ 
		Fruit f=null;
		try {
			f=(Fruit)Class.forName(ClassName).newInstance();
		}
		catch (Exception e) {
			e.printStackTrace();
		}
		return f;
	}
}

class Client {
	public static void main(String[] a) {
		Fruit f = Factory.getInstance("io.github.dunwu.spring.Apple");
		if(f!=null){
			f.eat();
		}
	}
}
```





### 10. spring 支持几种 bean scope？

Spring bean 支持 5 种 scope：

* Singleton - 每个 Spring IoC 容器仅有一个单实例。
* Prototype - 每次请求都会产生一个新的实例。
* Request - 每一次 HTTP 请求都会产生一个新的实例，并且该 bean 仅在当前 HTTP 请求内有效。
* Session - 每一次 HTTP 请求都会产生一个新的 bean，同时该 bean 仅在当前 HTTP session 内有效。
* Global-session - 类似于标准的 HTTP Session 作用域，不过它仅仅在基于 portlet 的 web 应用中才有意义。

Portlet 规范定义了全局 Session 的概念，它被所有构成某个 portlet web 应用的各种不同的 portlet 所共享。在 globalsession 作用域中定义的 bean 被限定于全局 portlet Session的生命周期范围内。如果你在 web 中使用 global session 作用域来标识 bean，那么 web会自动当成 session 类型来使用。 仅当用户使用支持 Web 的 ApplicationContext 时，最后三个才可用。





###  11. spring bean 容器的生命周期是什么样的？

spring bean 容器的生命周期流程如下：

1. Spring 容器根据配置中的 bean 定义中实例化 bean。
2. Spring 使用依赖注入填充所有属性，如 bean 中所定义的配置。
3. 如果 bean 实现 BeanNameAware 接口，则工厂通过传递 bean 的 ID 来调用 setBeanName()。
4. 如果 bean 实现 BeanFactoryAware 接口，工厂通过传递自身的实例来调用 setBeanFactory()。
5. 如果存在与 bean 关联的任何 BeanPostProcessors，则调用preProcessBeforeInitialization() 方法。
6. 如果为 bean 指定了 init 方法（ <bean> 的 init-method 属性），那么将调 用它。
7. 最后，如果存在与 bean 关联的任何 BeanPostProcessors，则将调用postProcessAfterInitialization() 方法。
8. 如果 bean 实现 DisposableBean 接口，当 spring 容器关闭时，会调用destory()。
9. 如果为 bean 指定了 destroy 方法（ <bean> 的 destroy-method 属性），那么将 调用它。





### 12. 什么是 spring 装配

当 bean 在 Spring 容器中组合在一起时，它被称为装配或 bean 装配。Spring 容器需要知道需要什么 bean 以及容器应该如何使用依赖注入来将 bean 绑定在一起，同时装配 bean。





### 13. 自动装配有哪些方式？

Spring 容器能够自动装配 bean。也就是说，可以通过检查 BeanFactory 的内容让 Spring自动解析 bean 的协作者。

自动装配的不同模式：
no - 这是默认设置，表示没有自动装配。应使用显式 bean 引用进行装配。
byName - 它根据 bean 的名称注入对象依赖项。它匹配并装配其属性与 XML文件中由相同名称定义的 bean。
byType - 它根据类型注入对象依赖项。如果属性的类型与 XML 文件中的一个bean 名称匹配，则匹配并装配属性。构造函数- 它通过调用类的构造函数来注
入依赖项。它有大量的参数。
autodetect - 首先容器尝试通过构造函数使用 autowire 装配，如果不能，则尝试通过 byType 自动装配。





### 14. 自动装配有什么局限？

覆盖的可能性 - 您始终可以使用 <constructor-arg> 和 <property> 设置指定依赖项，这将覆盖自动装配。

基本元数据类型 - 简单属性（如原数据类型，字符串和类）无法自动装配。

令人困惑的性质 - 总是喜欢使用明确的装配，因为自动装配不太精确。





### 15. @Component, @Controller, @Repository,@Service 有何区别？

`@Component` ：这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。

`@Controller` ：这将一个类标记为 Spring Web MVC 控制器。标有它的 Bean 会自动导入到IoC 容器中。

`@Service` ：此注解是组件注解的特化。它不会对@Component 注解提供任何其他行为。您可以在服务层类中使用@Service 而不是 @Component，因为它以更好的方式指定了意图。

`@Repository` ：这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 SpringDataAccessException。





### 16. @Required 注解有什么用？

@Required 应用于 bean 属性 setter 方法。此注解仅指示必须在配置时使用 bean 定义中的显式属性值或使用自动装配填充受影响的 bean 属性。如果尚未填充受影响的 bean 属性，则容器将抛出 `BeanInitializationException`。

```java
public class Employee {
	private String name;
	@Required
	public void setName(String name){ 
		this.name=name;
	}
	public string getName(){
		return name;
	}
}
```





### 17. @Autowired 注解有什么用？

@Autowired 可以更准确地控制应该在何处以及如何进行自动装配。此注解用于在 setter方法，构造函数，具有任意名称或多个参数的属性或方法上自动装配 bean。默认情况下，它是类型驱动的注入。

```java
public class Employee {
	private String name;
	@Autowired
	public void setName(String name){ 
		this.name=name;
	}
	public string getName(){
		return name;
	}
}
```





### 18. @Qualifier 注解有什么用？

当您创建多个相同类型的 bean 并希望仅使用属性装配其中一个 bean 时，您可以使用@Qualifier 注解和 @Autowired 通过指定应该装配哪个确切的 bean 来消除歧义。 例如，这里我们分别有两个类，Employee 和 EmpAccount。在 EmpAccount 中，使用@Qualifier指定了必须装配 id 为 emp1 的 bean。

```JAVA
Employee.java
public class Employee {
	private String name;
	@Autowired
	public void setName(String name){ 
		this.name=name;
	}
	public string getName() {
		return name;
	}
}
```

```JAVA
EmpAccount.java
public class EmpAccount {
	private Employee emp;
	@Autowired
	@Qualifier(emp1)
	public void showName() {
		System.out.println(“Employee name : ”+emp.getName);
	}
}
```





### 19. @RequestMapping 注解有什么用？

@RequestMapping 注解用于将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法。此注释可应用于两个级别：类级别：映射请求的 URL 方法级别：映射 URL 以及 HTTP 请求方法





### 20. Spring DAO 有什么用？

Spring DAO 使得 JDBC，Hibernate 或 JDO 这样的数据访问技术更容易以一种统一的方式工作。这使得用户容易在持久性技术之间切换。它还允许您在编写代码时，无需考虑捕获每种技术不同的异常。





### 21. 列举 Spring DAO 常见的异常。

![image-20210719102239970](http://img.javadaily.cn/image-20210719102239970.png)







### 22. 列举 spring 支持的事务管理类型

Spring 支持两种类型的事务管理：

1. 程序化事务管理：在此过程中，在编程的帮助下管理事务。它为您提供极大的灵活性，但维护起来非常困难。
2. 声明式事务管理：在此，事务管理与业务代码分离。仅使用注解或基于XML 的配置来管理事务。





### 23. 什么是 AOP？

AOP(Aspect-Oriented Programming), 即 面向切面编程, 它与 OOP( Object-Oriented Programming, 面向对象编程) 相辅相成, 提供了与 OOP 不同的抽象软件结构的视角. 在OOP 中, 我们以类(class)作为我们的基本单元, 而 AOP 中的基本单元是 Aspect(切面)





### 24. 什么是 Aspect？

aspect 由 pointcount 和 advice 组成, 它既包含了横切逻辑的定义, 也包括了连接点的定义。
Spring AOP 就是负责实施切面的框架, 它将切面所定义的横切逻辑编织到切面所指定的连接点中. AOP 的工作重心在于如何将增强编织目标对象的连接点上, 这里包含两个工作:
(1)  如何通过 pointcut 和 advice 定位到特定的 joinpoint 上
(2)  如何在 advice 中编写切面代码.

![image-20210719102427606](http://img.javadaily.cn/image-20210719102427606.png)

可以简单地认为, 使用 @Aspect 注解的类就是切面。





### 25. 什么是切点（JoinPoint）

程序运行中的一些时间点, 例如一个方法的执行, 或者是一个异常的处理.在 Spring AOP中, join point 总是方法的执行点。





### 26. 什么是通知（Advice）

特定 JoinPoint 处的 Aspect 所采取的动作称为 Advice。Spring AOP 使用一个 Advice 作为拦截器，在 JoinPoint “周围”维护一系列的拦截器。





### 27. 有哪些类型的通知（Advice）？

* Before - 这些类型的 Advice 在 joinpoint 方法之前执行，并使用@Before 注解标记进行配置。
* After Returning - 这些类型的 Advice 在连接点方法正常执行后执行，并使用@AfterReturning 注解标记进行配置。
* After Throwing - 这些类型的 Advice 仅在joinpoint 方法通过抛出异常退出并使用 @AfterThrowing 注解标记配置时执行。
* After(finally) - 这些类型的 Advice 在连接点方法之后执行，无论方法退 出 是正常还是异常返回，并使用 @After 注解标记进行配置。
* Around - 这些类型的 Advice 在连接点之前和之后执行，并使用@Around 注解标记进行配置。





### 28. AOP 有哪些实现方式？

实现 AOP 的技术，主要分为两大类：

**静态代理**  指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；

* 编译时编织（特殊编译器实现）

* 类加载时编织（特殊的类加载器实现）。



**动态代理** 在运行时在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。

* JDK 动态代理
* CGLIB





### 29. Spring AOP and AspectJ AOP 有什么区别？

Spring AOP 基于动态代理方式实现；

AspectJ 基于静态代理方式实现。

SpringAOP 仅支持方法级别的 PointCut；提供了完全的 AOP 支持，它还支持属性级别的 PointCut。





### 30. Spring MVC 框架有什么用？

Spring Web MVC 框架提供 模型-视图-控制器 架构和随时可用的组件，用于开发灵活且松散耦合的 Web 应用程序。MVC 模式有助于分离应用程序的不同方面，如输入逻辑，业务逻辑和 UI 逻辑，同时在所有这些元素之间提供松散耦合。





### 31. 描述一下 DispatcherServlet 的工作流程

DispatcherServlet 的工作流程可以用一幅图来说明：

![image-20210719103052937](http://img.javadaily.cn/image-20210719103052937.png)

1.  向服务器发送 HTTP 请求，请求被前端控制器 DispatcherServlet 捕获。
2.  DispatcherServlet 根据 -servlet.xml 中的配置对请求的 URL 进行解析，得到请求资源标识符（URI）。然后根据该 URI，调用 HandlerMapping 获得该 Handler 配置的所有相关的对象（包括 Handler 对象以及 Handler 对象对应的拦截器），最后以 HandlerExecutionChain 对象的形式返回。
3.  DispatcherServlet 根据获得的 Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得 HandlerAdapter 后，此时将开始执
    行拦截器的 preHandler(...)方法）。
4.  提取 Request 中的模型数据，填充 Handler 入参，开始执行 Handler（ Controller)。在填充 Handler 的入参过程中，根据你的配置，Spring 将
    帮你做一些额外的工作：
    * HttpMessageConveter：将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息。
    * 数据转换：对请求消息进行数据转换。如 String 转换成 Integer、Double等。
    * 数据格式化：对请求消息进行数据格式化。如将字符串转换成格式化数字或格式化日期等。
    * 数据验证：验证数据的有效性（长度、格式等），验证结果存储到 BindingResult 或 Error 中。
5.  Handler(Controller)执行完成后，向 DispatcherServlet 返回一个ModelAndView 对象；
6.  根据返回的 ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的 ViewResolver)返回给 DispatcherServlet。
7.  ViewResolver 结合 Model 和 View，来渲染视图。
8.  视图负责将渲染结果返回给客户端。





### 32. 介绍一下 WebApplicationContext

WebApplicationContext 是 ApplicationContext 的扩展。它具有 Web 应用程序所需的一些额外功能。它与普通的 ApplicationContext 在解析主题和决定与哪个 servlet 关联的能力方面有所不同。