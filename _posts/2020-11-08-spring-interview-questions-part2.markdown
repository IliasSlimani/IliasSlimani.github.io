---
layout: post
comments: true
author: islimani
title: Spring Web MVC
date: 2020-11-05 13:32:20 +0300
words: 0
summary: In this tutorial, we are gonna speak about what a developer should know in Spring Web MVC, RestFul Web Services, Spring Test and Spring Security. # Add post description (optional)
thumbnail: /assets/img/MVC.png
category:
        - spring_mvc
---


## Introduction
In the last post, we talked about Spring basics including Spring IOC with Spring AOP.
This time, we are gonna speak about what a developer should know in Spring Web MVC, RestFul Web Services, Spring Test and Spring Security.

## Spring Web MVC

### What's Spring Web MVC

> The Spring Web MVC framework provides Model-View-Controller (MVC) architecture and ready components that can be used to develop flexible and loosely coupled web applications.

#### Open-Close Principal

Spring MVC adheres to **Open for extension, close for modification** principal. Just bear in mind,
> this principle states that a class should protect itself from alteration while at the same time providing well-defined extension points [^1]

[^1]: Expert Spring MVC and Web Flow by Seth Ladd, Darren Davison, Steven Devijver, and Colin Yates Apress 2006

Seems to abstract, I know me too I couldn't grasp it at first glance. But, check the web they are countless examples that show you how does it work.

#### Controller

Controller intercepts user input and transform it into a model that is represented to the user by the view;

### Model

The Model is just a **Map** collection of **key-value** objects that are passed to the view. This has nothing to do with the model that you define in your business layer, the so-called **DTO** (Data Transfer Object) and that is accessed by **DAO** (Data Access Object). In other implementations of the MVC paradigm, this might be the case, but for the sake for a Spring Application don't confuse them. Remember we are talking now about the **Web Layer**.

### View

This is the top layer in our stack. It is responsible for rendering the model into a suited format to the end-user, be it a Web page, a PDF document, an Excel spreadsheet, etc. using different view technologies, Thymeleaf, JSP, etc.

### What are the benefits of Spring MVC

Spring Mvc advocates an MVC paradigm. It provides, among others, the following benefits:

* Fastest development. By making developers of the front end focus more on the view side and the backend on the business logic.
* Separation of concerns: Makes the code easier to maintain, to re-use and to test.
* Non-constraint to one single view technology: because of separation, we can display the view in different format (html, pdf, etc.)
* Clear Separation of roles: Each role can be fulfilled by a specialized object.

### What's a DispatcherServlet and how does it work?

The DispatcherServlet is Spring MVC's implementation of the front controller pattern. It is responsible for receiving all the http requests and delegate them to appropriate beans to process them and render the appropriate responses.

### What's Handler Mapping?

Handler Mapping allows you to map incoming web requests to appropriate handlers with the use of the `@RequestMapping` annotation.

### How does the `ViewResolver` work?

What the `ViewResolver` simply does is to resolve String-based view names returned from a handler to an actual `View`. That is, it provides a mapping between view names and actual views.

### Describe URI mapping in Spring MVC?

There are three components to a mapped URI path, the `WebApplicationContext` path, the `servlet mapping`, and the
`Controller` mapping.

Example: 

`http://example.com/SpringProject/app/home`

* the `WebApplicationContext`  is often your application/project name *(SpringProject)*. You can retrieve it Programmatically using `getRealPath()` or inside your JSP page using `${pageContext.request.contextPath}` or when working with Spring Boot you could even change it inside your properties file by writing `server.contextPath=/app`. 

* The Servlet Mapping is the mapping of the servlet defined inside `web.xml` or programmatically *(app)*. E.g.

```xml
<servlet-mapping>
<servlet-name>spring</servlet-name>
<url-pattern>/app/*</url-pattern>
</servlet-mapping>
```

* Finally the controller mapping `@RequestMapping` and its sisters. E.g.

```java
@Controller

public class HomeController {

	@RequestMapping(value="/home", method=GET)

	public String home() {

		return "home";

	}

}
```

### Explain the role of `@RequestMapping` and its sisters?

> The `@RequestMapping` annotation is used to map requests to controllers methods. It has various attributes to match by URL, HTTP method, request parameters, headers, and media types. It can be used at the class-level to express shared mappings or at the method level to narrow down to a specific endpoint mapping

These are some shotcuts for `@RequestMapping`, mainly:

* `GetMapping` is a shortcut for `@RequestMapping(method = RequestMethod.GET)`
* `PostMapping` is a shortcut for `@RequestMapping(method = RequestMethod.POST)`
* `PutMapping` is a shortcut for `@RequestMapping(method = RequestMethod.PUT)`
* `DeleteMapping` is a shortcut for `@RequestMapping(method = RequestMethod.DELETE)`
* `PatchMapping` is a shortcut for `@RequestMapping(method = RequestMethod.PATCH)`

### Explain the role of `@PathVariable` ?

`@PathVariable` is used to map URI variables like in the following example:

```java
@Controller

@RequestMapping("/students/{studentId}")

public class OwnerController {

        @GetMapping("/subject/{subjectId}")

        public Subject findSubject(@PathVariable Long studentId, @PathVariable Long subjectId) {
                // ...
        }
}
```


* URI variables are automatically converted to the appropriate type
* URI variables can be named explicitly

### Explain the role of `@ModelAttribute` ?

`@ModelAttribute` is used to retrieve argument from the Model. If it is not present in the model, it is instantiated and then added to the model. Often, we use it to bind form fields to the model, what's called `Data Binding`.

### Explain the role of `@RequestBody` ?
`@RequestBody` annotation maps an `HttpRequest` body into an object. Spring automatically deserialize the JSON into a Java type already defined.

### Explain the role of `@ResponseBody` ?
`@RequestBody` annotation does quite the opposite. It tells a `Controller` to serialize the returned object into JSON and passed back into the `HttpResponse`.

### Explain the role of `@RequestParam` ?

`@RequestParam` annotation is used to retrieve URL parameter and map it to the method argument. E.g:

URL `example.com/home?name=Ilias`
```java
@RequestMapping(value={"/",/home","/index"})
@ResponseBody
public String index(@RequestParam("name") String name){
	return "Hello "+name;
}
```


