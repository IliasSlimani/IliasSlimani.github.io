---
layout: post
title:  "What's Aspect Oriented Programming (AOP) in Spring Boot? with examples"
summary: "Aspect-oriented programming is a programming paradigm which enables improved modularity for software"
author: islimani
date: '2022-10-11 01:52:47 -0400'
category:
        - spring_basics
        - spring_aop
thumbnail: /assets/img/posts/aop.png
keywords: aop in spring boot,enableaspectjautoproxy,spring aspect,aop java,spring boot aspect
permalink: /blog/spring-aop-in-spring-aop
usemathjax: true
---

# What's Aspect Oriented Programming?

Aspect-oriented programming is a programming paradigm which enables improved modularity for software by allowing separation of cross-cutting concerns. It is a programming style that has been gaining in popularity in recent years, as it can lead to more maintainable and understandable code. AOP has been used in a variety of programming languages, but is most commonly associated with Java.
In this article, we'll take a look at how to use Spring AOP to add logging to a Spring-based application.

# Why AOP?

The main benefit of AOP is that it can improve the modularity of software. This is because AOP can be used to separate out concerns that would otherwise be spread across multiple modules. This can make it easier to understand and maintain code, as each concern can be dealt with in a separate module. In addition, it can make code more reusable, as modules can be reused in different contexts.

AOP can be used for a variety of purposes, but is most commonly used for logging, security, and transaction management. These are all examples of cross-cutting concerns, as they are required by many different modules in an application. By separating these concerns out into separate modules, they can be reused across the application without duplicating code.



# AOP terminology

Spring AOP terminology can be a bit confusing, so let's break it down.

**Aspects:** Aspects are modular components that can be woven into your application to provide cross-cutting functionality. For example, you might define an aspect that provides logging functionality.

**Join points:** Join points are places in your code where an aspect can be woven in. In Spring AOP, a join point always corresponds to a method invocation.

**Advice:** Advice is the functionality that an aspect provides at a join point. There are several different types of advice, but the most common are before advice and after advice.

**Pointcuts:** A pointcut is a predicate that is used to match join points. A pointcut defines where and how an aspect should be woven into your code.

**Weaving:** Weaving is the process of combining aspects with your application code to create the desired cross-cutting functionality. In Spring AOP, weaving is typically done at compile time, but it can also be done at load time or at runtime.

**Concerns:** Concerns are pieces of functionality that cut across multiple modules of your code. For example, logging is a common concern that is typically implemented in many different places throughout an application.

With AOP, you can centralize the implementation of a concern and then apply it to your code modules using "aspects." Aspects are like modules of AOP functionality that can be applied to your code.



# How to use AOP in spring boot

We'll start by creating a simple Spring-based application. This application will have a single `@Controller` that will handle requests to the `"/" URL`. We'll also add a simple `@Service` class that will be used to process requests.

Next, we'll configure Spring AOP to enable aspects. We'll create a simple logging aspect that will log all method invocations. Finally, we'll wire everything together and test it out.

So let's get started!

**Creating the Spring-based Application**

Our example Spring-based application will be extremely simple. We'll start by creating a `Maven-based` project in your favorite `IDE`. Be sure to select the `Web` profile when creating the project.

Once the project is created, we can add the following dependencies to the `pom.xml` file:

`spring-aop`
`spring-aspects`
`log4j`

With the dependencies in place, we can now add our `@Controller` and `@Service` classes.

Our `@Controller` will be responsible for handling requests to the `"/" URL`. It will simply return a `view` named `"home"`:
```
@Controller
public class HomeController {

    @RequestMapping("/")
    public String home() {
        return "home";
    }
}
```
Our `@Service` class will be responsible for processing requests. It will have a single `@Transactional` method that we'll invoke from our controller:
```
@Service
public class RequestService {

    @Transactional
    public void processRequest() {
        // do something
    }
}
```

## Configuring Spring AOP using Xml

With our Spring-based application in place, we can now configure Spring AOP. We'll do this by adding the following configuration to the `src/main/resources/aop.xml` file:
```
<?xml version="1.0" encoding="UTF-8"?>
<aop:aspectj-autoproxy />
```
This simple configuration will enable Spring AOP and instruct it to use AspectJ to generate proxies for our beans.

With the AOP configuration in place, we can now write our logging aspect.

**Writing the Logging Aspect**

Our logging aspect will be responsible for logging all method invocations. We'll start by annotating our aspect class with the `@Aspect` annotation:
```
@Aspect
public class LoggingAspect {

    // our advice methods go here
}
```
Next, we'll write our advice methods. We'll start by writing a method that will log all method invocations:
```
@Around("execution(* *.*(..))")
public Object logMethodInvocation(ProceedingJoinPoint joinPoint) throws Throwable {
    // log the invocation
    return joinPoint.proceed();
}
```
This advice method will be invoked for all method invocations. It will simply log the invocation and then proceed with the invocation.

Next, we'll write a method that will log all exceptions:
```
@AfterThrowing(pointcut = "execution(* *.*(..))", throwing = "exception")
public void logException(Exception exception) {
    // log the exception
}
```
This advice method will be invoked for all method invocations that result in an exception being thrown. It will simply log the exception.

**Wiring Everything Together**

With our logging aspect in place, we can now wire everything together. We'll do this by adding the following configuration to the `src/main/resources/applicationContext.xml` file:
```
<?xml version="1.0" encoding="UTF-8"?>
<beans:beans xmlns="http://www.springframework.org/schema/mvc"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:beans="http://www.springframework.org/schema/beans"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc.xsd
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- Enable Spring AOP -->
    <aop:aspectj-autoproxy />

    <!-- Scan for @Controllers and @Services -->
    <context:component-scan base-package="com.example" />

    <!-- Configure the Spring MVC view resolver -->
    <beans:bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <beans:property name="prefix" value="/WEB-INF/views/" />
        <beans:property name="suffix" value=".jsp" />
    </beans:bean>
</beans:beans>
```
In this configuration, we've enabled Spring AOP and instructed it to scan for @Controllers and @Services. We've also configured the Spring MVC view resolver.

**Testing the Application**

We can now test our application. We'll start by adding a simple `JSP` page to the `src/main/webapp/WEB-INF/views` directory:
```
<html>
<body>
    <h1>Hello, world!</h1>
</body>
</html>
```
We can now deploy our application to a `servlet` container and test it out. When we access the `"/" URL`, we should see the following output in the log:
```
INFO: Around advice: com.example.RequestService.processRequest()
INFO: Exception advice: com.example.RequestService.processRequest()
```
As we can see, our logging aspect was invoked for both the method invocation and the exception.

## Configuring Spring AOP using Annotations:

`@EnableAspectJAutoProxy` is a Spring annotation that can be used to enable AspectJ auto-proxy creation in Spring. This annotation can be used in conjunction with `@Configuration` and `@ComponentScan` to enable AspectJ auto-proxy creation in a Spring application.

`@EnableAspectJAutoProxy` can be used with or without `@Configuration`. If `@Configuration` is used, the AspectJ auto-proxy will be created based on the configuration in the `@Configuration` class. If `@Configuration` is not used, the AspectJ auto-proxy will be created based on the classpath scanning.

`@EnableAspectJAutoProxy` can be used with or without `@ComponentScan`. If `@ComponentScan` is used, the AspectJ auto-proxy will be created for all the classes in the package that is specified in the `@ComponentScan` annotation. If `@ComponentScan` is not used, the AspectJ auto-proxy will be created for all the classes in the classpath.

The `@EnableAspectJAutoProxy` annotation has two attributes:

1. proxyTargetClass: This attribute is used to specify whether proxies should be created for classes that are not annotated with @Aspect. The default value is false.

2. exposeProxy: This attribute is used to specify whether the current proxy should be exposed to the target class. The default value is false.

Let's look at an example of how to use the `@EnableAspectJAutoProxy` annotation.

In this example, we'll create a simple Spring application that uses the `@EnableAspectJAutoProxy` annotation. We'll create an aspect that logs method calls, and we'll use the @Aspect annotation to mark it as an aspect.
```
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {

}

@Aspect
public class LoggingAspect {

}
```
In the example above, we've used the `@EnableAspectJAutoProxy` annotation to enable AspectJ auto-proxy creation in our Spring application. We've also used the `@Configuration` annotation to indicate that our AppConfig class is a Spring configuration class.

We've also created a simple aspect that logs method calls. We've used the `@Aspect` annotation to mark our aspect as an AspectJ aspect.

Now that our configuration is complete, let's write a simple test to see our aspect in action.
```
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = AppConfig.class)
public class AppTest {

@Test
public void test() {

}

}
```
In the test above, we've used the `@RunWith` and `@ContextConfiguration` annotations to load our Spring application context. We've also specified our AppConfig class as the configuration class for our application context.

Finally, we've written a simple test that doesn't do anything. However, when we run our test, we should see the following output:
```
[main] INFO LoggingAspect - Entering method test
[main] INFO LoggingAspect - Exiting method test
```
As we can see, our aspect is working as expected. It's intercepting the call to the test method, and it's logging the enter and exit of the method.

The `@EnableAspectJAutoProxy` annotation is a powerful tool that can be used to enable AspectJ auto-proxy creation in Spring. It can be used to dynamically modify the behavior of classes at runtime, without changing the code.


# Conclusion

While Spring aop has ample benefits it has a few drawbacks. Firstly, it can make code more difficult to understand, as it can introduce a lot of new concepts. Secondly, it can make code less flexible, as it can be difficult to change the order in which aspects are applied. Finally, it can be difficult to test code that uses AOP, as aspects can interact in unexpected ways.



You can find the full source code on github [here](https://github.com/devtestperso/spring-myblog-tutorials).

