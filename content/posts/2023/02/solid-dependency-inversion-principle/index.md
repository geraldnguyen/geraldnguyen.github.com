+++
title = 'SOLID in Action: the Dependency Inversion Principle'
subtitle = 'Depend upon Abstractions. Do not depend upon concretions.'
date = 2023-02-21T08:00:00-07:00
tags = [ "Software Engineering", "SOLID principles", "Programming", "Object Oriented", "Dependency Inversion"]
categories = ["Software Development"]
image = "0_Uv2de6h1frX8mT2N.jpg"

+++

{{< figure src="0_Uv2de6h1frX8mT2N.jpg" caption="Photo by [ocd studio](https://unsplash.com/@ocd_studio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)">}}


The Dependency Inversion Principle (DIP)
========================================

Naive application of traditional software development methods (e.g. [**SADT**](https://en.wikipedia.org/wiki/SADT) or Structured Analysis and Design Technique), in the past, often resulted in a type of relationship where high-level modules depended on lower implementation details. This form of relationship often leads to [rigidity, fragility, and immobility](https://web.archive.org/web/20110714224327/http://www.objectmentor.com/resources/articles/dip.pdf). These are the symptoms of bad design according to [Robert C. Martin](https://en.wikipedia.org/wiki/Robert_C._Martin).


{{< figure src="1_RVjxPenHUcH9qZ_AuwwvNQ.png" caption="A naive application of top-down approaches can lead to bad design. Source: [R.C.M paper](https://web.archive.org/web/20110714224327/http://www.objectmentor.com/resources/articles/dip.pdf)">}}

The Dependency Inversion Principle (DIP) means to _invert_ such dependency relationships. Rather than having the relationship flow in one direction and tie upper layers onto implementation details of lower layers, the implementation of the lower layers now inversely points upward to abstraction layers that sit between the upper and lower layers. The benefit of this DIP is the preservation of the benefits of conventional analysis and design techniques plus the flexibility and reusability of abstraction-based object-oriented designs.

{{< figure src="1_8mdQPHvwOjBn90I59w7rsQ.png" caption="Source: [R.C.M paper](https://web.archive.org/web/20110714224327/http://www.objectmentor.com/resources/articles/dip.pdf)">}}


The above diagram can be summarized as:

> High-level modules should not depend upon low-level modules. Both should depend upon abstractions
> 
> Abstractions should not depend upon details. Details should depend upon abstraction

Examples
========

As long as you are considering a dependency where the depended-on component or service can have 2 or more implementations or can be further expanded through inheritance, the DIP can be put to good benefit.

In my [Becoming a SOLID developer]({{< ref "posts/2022/12/becoming-a-solid-developer" >}}) article, I share 2 examples:

*   An `OrderService` depends on the interface provided by `EmailService` and not its IMAP or POP implementation.

{{< figure src="0_DZQq2UAT2b8NWQpB.png" caption="EmailService and its associations">}}

*   An `UrbanTripPlanner` depends on the abstraction provided by `Car` , not its Tesla or Toyota implementation

{{< figure src="0_PAqBR3pZwatDEZYI.png" caption="Car and its associations">}}


And Misuses
===========

Martin Fowler called out the overuse of interfaces for single-implementation service in his [Interface Implementation Pair article](https://martinfowler.com/bliki/InterfaceImplementationPair.html)

> The practice of taking every class and pairing it with an interface. So as a result you see pairs of things — maybe ICustomer and Customer or Customer and CustomerImpl…Using interfaces when you aren’t going to have multiple implementations is extra effort to keep everything in sync (although good IDEs help). Furthermore, it hides the cases where you actually do provide multiple implementations.

Another common misuse in object-oriented programming languages is equating abstraction with interface construct. While interface is a popular way to express abstraction, it is by no means the only way, especially in dynamically typed languages or languages without `interface` support such as JavaScript or Python. We should rather consider an abstraction from a less concrete perspective. Take the abstraction expressed by the `Car` class above, for example, it does not have to be an interface to represent an abstraction.

In relation to the Inversion of Control
=======================================

> IoC is also known as dependency injection (DI). It is a process whereby objects define their dependencies (that is, the other objects they work with) only through constructor arguments, arguments to a factory method, or properties that are set on the object instance after it is constructed or returned from a factory method. The container then injects those dependencies when it creates the bean. This process is fundamentally the inverse (hence the name, Inversion of Control) of the bean itself controlling the instantiation or location of its dependencies by using direct construction of classes or a mechanism such as the Service Locator pattern — [https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-introduction)

Inversion of Control (IoC) may share the term “inversion” but DIP and IoC are different. The distinction between the 2 concepts is in the purposes of their application. While DIP guides the modeling of relationships between components, the IoC provides ways to assemble the application while respecting the DIP relationship.

Conclusion
==========

We have examined the meaning of the Dependency Inversion Principle and some examples of proper applications as well as misuse. We have also clarified its purposes by comparing it with the IoC principle.


{{< include "posts/2022/12/becoming-a-solid-developer/series.include" >}}