---
categories:
- Software Development
date: 2013-09-10 09:19:42+01:00
draft: false
subtitle: It’ll probably result in an EJBTransactionRolledbackException caused by
  a NullPointerException
tags:
- Java
- EJB
- old blog
title: What Happened When You Assigned State to a Stateless Session Bean?
---
Here’s a scenario that illustrate above possibility

```
    T1                   |     T2
1) setupState [A]        |                   <- store state as a variable
                         |                      in bean instance [A]
                         | 2) setupState [A] <- [A] is free, so the container
                         |                      allocate it to serve T2 request
                         |
3) someLongRunMethod [A] | 3') aMethod [B]   <- Because A is busy with T1, 
                                                another instance (B) is created 
                                                to serve T2 request. If aMethod 
                                                depends on previously setup state
                                                it will encounter error

```