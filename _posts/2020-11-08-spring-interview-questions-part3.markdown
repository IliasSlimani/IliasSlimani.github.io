---
layout: post
comments: true
author: islimani
title: Spring Security
date: 2017-11-12 13:32:20 +0300
words: 0
summary: "In this tutorial, we are gonna talk about Spring Security. I'll divide this article into three parts: Generalities, Practical and Architectural."
thumbnail: /assets/img/security.png
category:
        - spring_security
---


## Introduction
In the last post, we talked about Spring Web MVC.
This time, we are gonna talk about Spring Security. I'll divide this article into three parts: Generalities, Practical and Architectural. Let's dive in!

## General Questions

### What's Spring Security?

**Spring Security** is a powerful and highly customizable *authentication* and *authorization* framework. It is the de-facto standard for securing Spring-based applications.[^1]

[^1]: https://projects.spring.io/spring-security/


### What's CSRF?

**Cross-site request forgery** is an attack that forces an end user to execute unwanted actions on a web application in which they're currently authenticated wihtout knowing about it!

To illustrate this even more, let's follow this scenario where Ilias is transferring 5$ to Youcef using a Banking application. The attacker in this case will be Jamal. We will imagine that we are using a form that has two fields, *amount*, *receiver* and a submit button.
The `POST` request will look like this:
```
POST http://bank.com/transfer

receiver=Youcef&amount=5
```

What Jamal could do in this case is to create a form with hidden fields:

```html
<form action="http://bank.com/transfer" method="POST">
<input type="hidden" name="receiver" value="Jamal"/>
<input type="hidden" name="amount" value="500"/>
<input type="submit" value="View sexy pictures"/>
</form>
```

Jamal has high skills in Social Enginnering. He will simply use that at his own advantage to send to Ilias an email with embedded html code that contains this form to trick him to click on the submit button when he's logged into the application.

Alternatively, he could use JavaScript:

```javascript
<body onload="document.forms[0].submit()">
```

and Voilà! Jamal receives 500$ from Ilias!

> Solving this problem is quite simple. Spring Security will automatically add a token to enforce *Same Origin Policy*.

### Define Password-salting

> Simply put, *Password-salting* is an encryption method used to increase the security of passwords by
adding a well-known string (salt) to them.

## Spring Security Architecture

To start-off, we need to understand some basic terms.

### What is a principal?

> *Principal* is a term that signifies a user, device, or system that could perform an action within the application

### What are Credentials?

> *Credentials* are identification keys that a principal uses to confirm its identity.

You can store them using different methods:

* RDBMS
* LDAP
* Properties Files

### What's Authentication?

> *Authentication* is the process of establishing a principal is who they claim to be.

There are many ways to authenticate in *Spring Security*: 

* Basic
* Form
* Oauth
* Cookies
* Single-Sign-On

### What's Authorization?

> *Authorization* refers to the process of deciding whether a principal is allowed to perform an action within your application. To do so, we define **Roles**.

Most common applications define the following roles:

* `ADMIN` is used for full control over users and can manipulate all data.
* `USER` is specific to users who can view and manipulate their own data.
* `GUEST` is used to view and access only limited data.

Now let's move on on how things are done.

1- First things first. A user tries to access the application by making a request. The application
requires the user to provide the credentials so it can be logged in.

2- The credentials are verified by the `Authenticaltion Manager` and the user
is granted access to the application. The authorization rights for this user are
loaded into the `Spring Security context`.

3- The user makes a resource request and the `Security Interceptor` intercepts the request before the user accesses a
protected resource.

4- The `Security Interceptor` extracts the user authorization data from the `security context` and delegates the decision to the `Access Decision Manager`.

5- The `Access Decision Manager` polls a list of voters to return a decision regarding the rights of the authenticated user to system resources.

6- Finally, Access is granted or denied.

### What's `DelegatingFilterProxy` and how does it work?

> `DelegatingFilterProxy` is an implementation of the `javax.servlet.Filter` interface. It delegates the calls to a list of chained security filter beans and acts as an interceptor for secured requests

Spring Security relies heavily on the use of `ServletFilters` to provide layers of functionality around a web application request.

> `ServletFilters` are used to intercept user requests and perform pre-or post-processing.

Remember our discussion on `AOP`. `ServletFilters` function as a proxy for `AOP` interception of web requests, as they allow functionality to occur before and after a web request.

Spring Security uses a series of filters, which are applied in a sequence through the use of a Java EE servlet `filter chain`.

> `Filter chain` is a Java EE Servlet API concept that allows a web application to apply a chain of servlet filters to any given request. 

All these filters are automatically populated when you declare `DelegatingFilterProxy` either in your xml file :

```xml
<filter>
	<filter-name>springSecurityFilterChain</filter-name>
	<filter-class>
		org.springframework.web.filter.DelegatingFilterProxy
	</filter-class>
</filter>
<filter-mapping>
	<filter-name>springSecurityFilterChain</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
```

or with Java-Configuration:

```java
public class SecurityInitializer extends AbstractSecurityWebApplicationInitializer {
}
```

Note: the use of `springSecurityFilterChain` is mandatory, since it declares the servlet filter chain that Spring Security will use when intercepting requests.

### Method Security

Method Security is used to apply security to lower layers of an application. Like in the following example: 

```
@Secured("ROLE_ADMIN")
public User findById(Long id) {
return userRepo.findOne(id);
}
```

In a Java based configuration, we need to annotate the config class with `@EnableGlobalMethodSecurity` annotation. Afterwards, methods could be secured by annotating them `@Secured`.

You can use **JSR250** annotations as well. The equivalent of `@Secured` Spring annotation is `@RolesAllowed`.

There are other Spring annotations that you could use dependent on your needs, like `@PreAuthorize`, `@PreFilter`, `@PostAuthorize`  and `@PostFilter`.

