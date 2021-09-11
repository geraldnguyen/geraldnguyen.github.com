---
title: "How do you initialize an EJB3 Stateful Session Bean ?"
date: 2010-11-20T09:19:42+01:00
draft: false
weight: 50
tags: j2ee, ejb3, old blog
---

In EJB 3, we can obtain a reference to Stateful Session Bean either either by using `@EJB `annotation or performing JNDI look-up. Yet these 2 methods do not offer you a way to initialize a Stateful SB as EJB 2.1  Home interface's `createMETHODs` do.

```
StatefulSB bean = home.create (arg0, arg1, arg2);
```

EJB3 defines `@Init` annotation as a replacement for EJB 2.1 `createMETHOD`. But it is intended for bean written to 2.1 client view (that means you will need to define an home interface and annotate the bean class with `@RemoteHome` or `@LocalHome` annotations)

So, how do you initialize an EJB3 Stateful Session Bean?

Here's my solution:
- Define an `create (arg0, arg1, arg2)` as a business method in the business interface. You can change the name to something more relevant (e.g. `initialize`)
- Implement this method as you would do with any 2.1 `ejbCreate` method
- Invoke this method before any business methods.

This way the Stateful SB is instantiated according to your requirement.

-------------------------------------

I found a slightly different solution at http://community.jboss.org/message/524703#524703 by Carlo de Wolf. Repost it here for easy comparison:


If you want you can even create a regular create method which mimicks the home interface:

```
interface MyRemote {
   MyRemote create(String state);
}
 
class MyBean implement MyRemote {
   MyRemote create(String state) {
      this.state = state;
      return ctx.getBusinessObject(MyRemote.class);
   }
}
```
