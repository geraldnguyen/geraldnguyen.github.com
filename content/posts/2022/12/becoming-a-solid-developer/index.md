+++
title = 'Becoming a SOLID developer'
subtitle = 'By following SOLID software engineering principles'
date = 2022-12-07T08:00:00-07:00
tags = [ "Software Engineering", "SOLID principles", "Uncle Bob", "Object Oriented"]
image = "1_jgPZbah9dkZgepyo6D2xwQ.png"
pinned = true
+++

Being called _solid_ is a compliment. The word carries the positive notions of reliability and respectability. All of us should aim to become one.

{{< figure src="1_jgPZbah9dkZgepyo6D2xwQ.webp" caption="https://www.collinsdictionary.com/dictionary/english/solid (screenshot)" >}}


If you are a software engineer or want to become one, you are in luck because there is a set of SOLID principles to guide your way.

{{< pagebreak >}}

SOLID is an acronym in the software engineering profession. It stands for:

*   **S**ingle Responsibility principle
*   **O**pen-closed principle
*   **L**iskov Substitution principle
*   **I**nterface segregation principle
*   **D**ependency inversion principle

The term was coined by [Michael Feather](https://michaelfeathers.silvrback.com/) around 2004 for the 5 object-oriented design principles first collected by [Uncle Bob](http://cleancoder.com/products) in his 2000 paper [Design Principles and Design Patterns](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf) and his other [articles](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod) and [books](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)

Let’s look at these principles in more detail.

Single Responsibility
=====================

> Each class should have a single responsibility.

An `EmailService` should just concern itself with sending emails, not with sending emails _and_ updating order status.

A `Car` class should be about cars only, not other types of motorized vehicles. If we need to describe an electric car, then a subclass such as`ElectricCar` is more appropriate than overloading `Car` with electric and petrol car characteristics.

Open-close principle
====================

> A module should be open for extension but closed for modification

The `EmailService` can be extended by a `RecallableEmailService` which adds recall-ability feature to the email just sent.

{{< figure src="1_cvKw_Z8ZspbkJieyjInM0w.webp" caption="EmailService and its associations" >}}

The `ElectricCar` adds electric features without modifying the internal of `Car`.

Liskov substitution
===================

> Subclasses should be substitutable for their base classes

In the context of Object-oriented design, a class or method that accepts an instance of a base class should be able to take any instance of its subclass.

An `OrderService` should be able to use any instance that conforms to the interface of `EmailService` without concerning that the instance’s actual class is `EmailService` itself or `RecallableEmailService`

An `UrbanTripPlanner` should be able to use any instance of `Car` to determine a route without knowing whether it represents an electric or petrol car.

Interface segregation
=====================

> Many client specific interfaces are better than one general purpose interface

An `OrderService` does not need to `recallEmail(...)` . Hence it only depends on `EmailService` for sending emails, not `RecallableEmailService`

Similarly, an `UrbanTripPlanner` does not need the `recharge(...)` method from an `EletricCar`. It depends only on the interface provided by `Car`

{{< figure src="1_AZ1fklqQkAmglfU5NxLC3g.webp" caption="Car and its associations" >}}


Dependency inversion
====================

> Depend upon Abstractions. Do not depend upon concretions.

The _procedural_ dependency mechanism starts from the top, gradually expands, and depends on concrete implementation details.

In Object-oriented programming, the dependency is _inverted_. The majority of dependencies should be upon abstractions. And where implementation exists, its relationship with abstraction _inverts_. Rather than being directly depended on, the implementation now depends on abstraction.

{{< figure src="1_xZNkJr8o1fxcJ2efNggICA.webp" caption="Source: Uncle Bob’s Design Principles and  Design Patterns" >}}


An `OrderService` depends on the interface provided by `EmailService` and not its IMAP or POP implementation.

An `UrbanTripPlanner` depends on the interface provided by `Car` , not its Tesla or Toyota implementation

Conclusion
==========

We have examined the origin of the SOLID acronym, as well as looked at each of them in detail with 2 object-oriented design use cases each.

You may have followed some of these principles in the past without knowing because they are good practices. Encapsulation, in my opinion, is following the **Open-close principle**.

You may have also applied some of them because it is the way things are. Syntactically, an `ElectricCar` object can always pass in for a `Car` parameter, thereby following the **Liskov substitution principle**. If you have used Dependency Injection, you already follow the **Dependency Inversion principle**.

Other principles require a bit of thought. The **Single Responsibility principle** needs a good OO model to keep the method/class/module clean and lean. You may need good judgment to determine how deep the segregation should be while following the **Interface Segregation principle**.

References
==========

*   [Design Principles and Design Patterns](https://web.archive.org/web/20150906155800/http://www.objectmentor.com/resources/articles/Principles_and_Patterns.pdf) paper
*   [Principles of OOD](http://www.butunclebob.com/ArticleS.UncleBob.PrinciplesOfOod) article
*   [Clean Architecture: A Craftsman’s Guide to Software Structure and Design](https://www.oreilly.com/library/view/clean-architecture-a/9780134494272/)
*   [https://en.wikipedia.org/wiki/SOLID](https://en.wikipedia.org/wiki/SOLID)

{{< pagebreak >}}

If you like this article, please [follow me](https://geraldnguyen.medium.com/subscribe) for more quality content.

Other articles in this series:

*   [Becoming a SOLID developer](https://medium.com/codex/becoming-a-solid-developer-7f0e0ab359f6)
*   [SOLID in Action — the Single Responsibility Principle](https://medium.com/codex/solid-in-action-the-single-responsibility-principle-7cb70c32cc03)
*   [SOLID in Action — the Open-closed principle](https://medium.com/codex/solid-in-action-the-open-closed-principle-5b8d09a60a5a)
*   [SOLID in Action — the Liskov Substitution principle](https://medium.com/geekculture/solid-in-action-the-liskov-substitution-principle-4b1868ad81fd)
*   [SOLID in Action — the Interface Segregation principle](https://medium.com/geekculture/solid-in-action-the-interface-segregation-principle-6c8f92d2133a)
*   [SOLID in Action — the Dependency Inversion principle](https://medium.com/geekculture/solid-in-action-the-dependency-inversion-principle-5567bddd6cfc)

Thank you.