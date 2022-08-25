---
layout: post
title:  "What's Response Entity in Spring?"
summary: "What's ResponseEntity in Spring Boot and How to use it in Rest Api?"
author: islimani
date: '2022-08-03 09:52:47 -0400'
category:
        - spring_basics
        - spring_boot
thumbnail: /assets/img/posts/code.jpg
keywords: responseentity java,spring boot responseentity,spring responseentity,response entity in rest api,springboot responseentity
permalink: /blog/response-entity-in-spring-boot
usemathjax: true
---


# Table of contents
* 1. [But What's an HTTP Entity?](#ButWhatsanHTTPEntity)
	* 1.1. [Http Entity Request Headers](#HttpEntityRequestHeaders)
	* 1.2. [Http Entity Request Body](#HttpEntityRequestBody)
	* 1.3. [Http Entity Response Headers](#HttpEntityResponseHeaders)
	* 1.4. [Http Entity Response Body](#HttpEntityResponseBody)
	* 1.5. [Http response status codes](#Httpresponsestatuscodes)
* 2. [Response Entity in Spring](#ResponseEntityinSpring)
	* 2.1. [ResponseEntity short Example:](#ResponseEntityshortExample:)

# What's ResponseEntity in Spring Boot and How to use it in Rest Api?

`Response Entity` in Spring is the embodiment of an `HTTP` response entity: the Header, the body and status.

##  1. <a name='ButWhatsanHTTPEntity'></a>But What's an `HTTP Entity`?

See whenever we make a request to a web server e.g opening a url in our browser or get a response back we get to send or receive an `HTTP Entity` that contains a lot of informations.

Let's call it this way:

>`Http Entity` is the basic building block in exchanging information between two parties in the web today

###  1.1. <a name='HttpEntityRequestHeaders'></a>`Http Entity Request Headers`

`Request Headers` are all the fields that are being sent by the client: *us* to the server. The whole purpose of these fields is to pass additional data or `metadata` about the request.

An example is this:

![Get Request From StackOverFlow](/assets/img/get_request_stackoverflow.PNG)

I've opened *www.stackoverflow.com* in my Browser and when we open `Chrome Dev Tools`, we can see that we made a `Get` request with the following `Headers`:

![Get Request Headers](/assets/img/request_headers.PNG)

All these informations have been sent to *StackOverFlow* when we simply opened the url in our Browser.

You can see some of the informations like our browser type. I've highlighted that in yellow. This `header` is called `user-agent`.

Other informations could be sent to tailor the response but more on that in the future.

We call these `Request Headers` because it's us that sends them and we can of course choose what we send to the server but not using the browser.

###  1.2. <a name='HttpEntityRequestBody'></a>`Http Entity Request Body`

`Request Body` is the data sent by the client: *us* to the server.

In the aformentionned example, we've only sent *Request Headers* but we didn't send other data along side the request.

If we wanted to do that, we need to send them in the *Request Body*.

Check this out:

![Post Request Data](/assets/img/request_body_1.PNG)

![Post Request Data](/assets/img/request_body_2.PNG)

In this example, we sent a request to *Google* with some of our informations like our email etc.


###  1.3. <a name='HttpEntityResponseHeaders'></a>`Http Entity Response Headers`

Likewise, `Response Headers` are all the fields that are being sent by the server to us. The whole purpose of these fields is to pass additional data or `metadata` about the response.

Check this example:

![Response Header](/assets/img/response_header.PNG)

We use the same example as before. But this time, we check the `Response Headers` from *Stackoverflow*.

One of the fields that are being sent is `Content-type`. This field is indicating that the content served by the server is an `html` document, not `xml` or `json` or any other type of content.

###  1.4. <a name='HttpEntityResponseBody'></a>`Http Entity Response Body`

`Response Body` is the data sent by the server to the client. It could be a document like an `html` file or some other type of data.

Earlier we received a `html` document from *stackoverflow*

Check this example:

![Response Body](/assets/img/response_body_1.PNG)

###  1.5. <a name='Httpresponsestatuscodes'></a>`Http response status` codes

`Http status` is what indicates to the client if the `Request` sent is successfull or not.

Since this is the only way of knowing what happened to the request, there are many status codes.

Some of these are:

<ol>

<li>
**200** code: indicating that the request has succeeded.
</li>
<li>
**400** code: indicating that the server can't process the request due an error with the request (either a malformed request, invalid data etc.).
</li>
<li>
**301** code: indicating that the requested resource has been moved to a new url.
</li>
</ol>


##  2. <a name='ResponseEntityinSpring'></a>`Response Entity` in Spring

Now since we understand what's response *Http Entity* is Let's move back to *Spring*.

in **Spring**, a `Response Entity` is represented by the *class* `ResponseEntity<Type>` where `Type` represents the **Body** that will be returned to the client.

`ResponseEntity` takes three parameters:

<ol>

<li>
`ResponseBody` this could be any type like `String`, a number or even another `Object`.
</li>
<li>
`HttpHeaders` this is class that we initiate and we add to it any **standard** or *custom* headers.
</li>
<li>
`HttpStatus` this is an `enum` that represents the **status** of the request.
</li>
</ol>


###  2.1. <a name='ResponseEntityshortExample:'></a>`ResponseEntity` short Example:

```
@GetMapping("/coupons")
    public ResponseEntity<Object> getAllCoupons() {
        // Get All Coupons from Database
        List<Coupon> coupons = couponService.getAllCoupons();

        // Create an Object with a successful message and send all the coupons
        Map<String, Object> mapResponse = new LinkedHashMap<String,Object>();
        map.put("message", "All coupons retrieved successfully");        
        map.put("data", coupons);

        // Send the response to the client
        return new ResponseEntity<Object>(mapResponse, HttpStatus.OK);
    }

```

In this example, we didn't set any `Headers`, we left the field empty.

However, We send the data requested by the client with a message and `Httpstatus` code of `OK` or `200` indicating a successfull request.

Now how to use `ResponseEntity` with a Restful api?

First, we need to annotate the Mother `Controller` with the following `Annotations`:

```
// Indicates this class is a controller and to serialize (convert) any object to Json and to pass it as HttpResponse
@RestController 

// Specify the parent endpoint
@RequestMapping(value = "/api" ) 
public class CouponController {

    // Specifies the path or route and the http method to map to this function
   @GetMapping("/coupons")
    public ResponseEntity<Object> getAllCoupons() {
        // Get All Coupons from Database
        List<Coupon> coupons = couponService.getAllCoupons();

        // Create an Object with a successful message and send all the coupons
        Map<String, Object> mapResponse = new LinkedHashMap<String,Object>();
        map.put("message", "All coupons retrieved successfully");        
        map.put("data", coupons);

        // Send the response to the client
        return new ResponseEntity<Object>(mapResponse, HttpStatus.OK);
    }

}
```

and you are ready to go.

# Conclusion

In this tutorial, we've learned the details of an `Http Entity` and how to use `Response Entity` in Spring and Rest api.

You can find the full source code on github [here](https://github.com/devtestperso/spring-myblog-tutorials).

