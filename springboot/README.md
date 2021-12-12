### 1. 什么是SpringBoot

1. 用来简化spring应用的初始搭建以及开发过程 使用特定的方式来进行配置（properties或yml文件）
2. 创建独立的spring引用程序 main方法运行
3. 嵌入的Tomcat 无需部署war文件
4. 简化maven配置





### 2. 什么是 JavaConfig ？

Spring JavaConfig 是 Spring 社区的产品，它提供了配置 Spring IoC 容器的纯 Java 方法。因此它有助于避免使用 XML 配置。 使用 JavaConfig 的优点在于：

* 面向对象的配置。

  由于配置被定义为 JavaConfig 中的类，因此用户可以充分利用 Java 中的面向对象功能。一个配置类可以继承另一个，重写它的[@Bean](https://my.oschina.net/bean) 方法等。

* 减少或消除 XML 配置。

  基于依赖注入原则的外化配置的好处已被证明。但是，许多开发人员不希望在 XML 和 Java 之间来回切换。 JavaConfig 为开发人员提供了一种纯 Java 方法来配置与 XML 配置概念相似的 Spring 容器。从技术角度来讲，只使用 JavaConfig 配置类来配置容器是可行的，但实际上很多人认为将 JavaConfig 与 XML 混合匹配是理想的。

* 类型安全和重构友好。

  JavaConfig 提供了一种类型安全的方法来配置 Spring 容器。由于Java 5.0 对泛型的支持，现在可以按类型而不是按名称检索 bean，不需要任何强制转换或基于字符串的查找。





### 3. Spring Boot 有哪些优点？

1. 快速创建独立运行的spring项目与主流框架集成

2. 使用嵌入式的servlet容器，应用无需打包成war包

3. starters自动依赖与版本控制

4. 大量的自动配置，简化开发，也可修改默认值

5. 准生产环境的运行应用监控

6. 与云计算的天然集成





### 4. Spring Boot **提供了哪些核心功能？**

1. 独立运行 Spring 项目
2. 内嵌 Servlet 容器 Spring Boot 可以选择内嵌 Tomcat、Jetty 或者 Undertow，这样我们无须以 war 包形式部署项目。
3. 提供 Starter 简化 Maven 配置 例如，当你使用了 spring-boot-starter-web ，会自动加入如下依赖：spring-boot-starter-web 的 pom 文件
4. 自动配置 Spring Bean Spring Boot 检测到特定类的存在，就会针对这个应用做一定的配置，进行自动配置 Bean ，这样会极大地减少我们要使用的配置。
5. 准生产的应用监控 Spring Boot 提供基于 HTTP、JMX、SSH 对运行时的项目进行监控。

无代码生成和 XML 配置 Spring Boot 没有引入任何形式的代码生成，它是使用的 Spring 4.0 的条件 @Condition 注解以实现根据条件进行配置。同时使用了 Maven /Gradle 的依赖传递解析机制来实现 Spring 应用里面的自动配置。





### 5. 如何重新加载Spring Boot上的更改，而无需重新启动服务器？

这可以使用DEV工具来实现。通过这种依赖关系，您可以节省任何更改，嵌入式tomcat将重新启动。

Spring Boot有一个开发工具（DevTools）模块，它有助于提高开发人员的生产力。





### 6. Spring Boot中的监视器是什么？

Spring boot actuator是spring启动框架中的重要功能之一。Spring boot监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。 有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为HTTP URL访问的REST端点来检查状态。





### 7. Springboot自动配置的原理

在spring程序main方法中 添加@SpringBootApplication或者@EnableAutoConfiguration 会自动去maven中读取每个starter中的spring.factories文件 该文件里配置了所有需要被创建spring容器中的bean





### 8. Spring Boot 的核心配置文件有哪几个？它们的区别是什么？

Spring Boot 的核心配置文件是 application 和 bootstrap 配置文件。

application 配置文件这个容易了解，主要用于 Spring Boot 项目的自动化配置。

bootstrap 配置文件有以下几个应用场景。 使用 Spring Cloud Config 配置中心时，这时需要在 bootstrap 配置文件中增加连接到配置中心的配置属性来加载外部配置中心的配置信息； 少量固定的不能被覆盖的属性； 少量加密/解密的场景；





### 9. Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？

启动类上面的注解是@SpringBootApplication，它也是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解：

* @SpringBootConfiguration：组合了 @Configuration 注解，实现配置文件的功能。
* @EnableAutoConfiguration：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })。
* @ComponentScan：Spring组件扫描





### 10. 为什么我们需要 spring-boot-maven-plugin?

spring-boot-maven-plugin 提供了一些像 jar 一样打包或者运行应用程序的命令。

spring-boot:run 运行你的 SpringBooty 应用程序。

spring-boot：repackage 重新打包你的 jar 包或者是 war 包使其可执行

spring-boot：start 和 spring-boot：stop 管理 Spring Boot 应用程序的生命周期（也可以说是为了集成测试）。

spring-boot:build-info 生成执行器可以使用的构造信息。





### 11. 什么是Swagger？你用Spring Boot实现了它吗？

Swagger广泛用于可视化API，使用Swagger UI为前端开发人员提供在线沙箱。

Swagger是用于生成RESTful Web服务的可视化表示的工具，规范和完整框架实现。它使文档能够以与服务器相同的速度更新。当通过Swagger正确定义时，消费者可以使用最少量的实现逻辑来理解远程服务并与其进行交互。因此，Swagger消除了调用服务时的猜测。





### 12. 什么是Spring Profiles？

Spring Profiles允许用户根据配置文件（dev，test，prod等）来注册bean。因此，当应用程序在开发中运行时，只有某些bean可以加载，而在PRODUCTION中，某些其他bean可以加载。假设我们的要求是Swagger文档仅适用于QA环境，并且禁用所有其他文档。这可以使用配置文件来完成。Spring Boot使得使用配置文件非常简单。





### 13. 配置文件的加载顺序

由jar包外向jar包内进行寻找;

1. 优先加载带profile jar包外部的application-{profile}.properties或application.yml(带spring.profile)配置文件
2. jar包内部的application-{profile}.properties或application.yml(带spring.profile)配置文件
3. 再来加载不带profile jar包外部的application.properties或application.yml(不带spring.profile)配置文件
4. jar包内部的application.properties或application.yml(不带spring.profile)配置文件





### 14.Spring Boot、Spring MVC 和 Spring 有什么区别?

Spring 是一个“引擎”

Spring MVC是基于Spring的一个 MVC 框架

Spring Boot是基于 Spring的一套快速开发整合包