---
title: Echo API
subtitle: A maven library of a /echo API that echoes HTTP request’s details back to the caller.
date: 2022-11-30T09:19:42+01:00
image: 1_gVavJM-Eio5Z-Og4eUKFeQ.png
draft: false
categories: [Software Development]
tags: [Java, Software Development, API, Open source]
medium: https://geraldnguyen.medium.com/echo-api-ef3408d520d2
---

# Introduction

The need for this library arose when I wanted assurance that my [HTTP client](https://www.npmjs.com/package/ngx-correlation-id) was sending correctly formulated HTTP messages.

Another use of this `/echo` API is to capture and debug the remote calls from another service. Imagine you are providing a webhook endpoint. Before you even build one, you first need to know what incoming messages look like. Supplying an `/echo` API endpoint in the webhook’s registration allows you to capture and log all incoming messages

{{< figure src="1_gVavJM-Eio5Z-Og4eUKFeQ.png" caption="Screenshot of a Postman request">}}

# Demo

You can open [https://echook.azurewebsites.net/echo](https://echook.azurewebsites.net/echo) on your browser, Postman, or call it from an API to see the `/echo` in action

Note: This site is hosted on Azure free tier. If it is a while since its last execution, it may take some time to respond. Otherwise, you can clone the [https://github.com/geraldnguyen/echo-server](https://github.com/geraldnguyen/echo-server) project and run the application locally

# Integration

`pom.xml`

```xml
<dependency>  
   <groupId>io.github.geraldnguyen</groupId>  
   <artifactId>echo</artifactId>  
   <version>1.0.0</version>  
</dependency>
```

`EchoServerApplication.java`

```java

import nguyen.gerald.echo.EnableEchoController;  
  
@SpringBootApplication  
@EnableEchoController  
public class EchoServerApplication {  
 public static void main(String[] args) {  
  SpringApplication.run(EchoServerApplication.class, args);  
 }  
}
```

# Features

The following information is echoed back in JSON format:

*   Path
*   Protocol
*   Headers
*   Query string
*   URL-encoded form submission
*   Multi-part form submission

# Limitation

The parsing of the query string and URL-encoded body depends on a deprecated class `HttpUtils` that supports only default encoding. The extraction code also does not handle malformed strings well.

# Hash fragment (#abc)

Browser does not include hash fragment in the HTTP request message. Java servlet specification does not provide any way to extract the original URL either.

Hence there is no way to extract and echo hash fragment back.

{{< pagebreak >}}

The library’s source code is available on my [GitHub](https://github.com/geraldnguyen/echo). Feel free to explore and try it out.