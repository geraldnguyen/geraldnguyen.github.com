---
layout: post
title:  "javax.transaction.Transactional vs org.springframework.transaction.annotation.Transactional"
date:   2020-09-10 22:32:09 +0800
categories: spring
---

It's easy to mix especially when we only need the default setting

> If you're using Spring or Spring Boot, then use the Spring @Transactional annotation, as it allows you to configure more attributes than the Java EE @Transactional annotation.
>
> If you are using Java EE alone, and you deploy your application on a Java EE application server, then use the Java EE @Transactional annotation.

https://stackoverflow.com/a/62702146/575457