+++
title = 'SOLID in Action — the Open-closed Principle'
subtitle = '“A software entities (classes, modules, functions, etc.) should be open for extension but closed for modification”'
date = 2022-12-24T08:00:00-07:00
tags = [ "Software Engineering", "SOLID principles", "Software Development", "Object Oriented", "Abstraction"]
categories = ["Software Development"]
image = "0_eqhYFU2kNfQ0TrN9.jpg"
medium = 'https://medium.com/codex/solid-in-action-the-open-closed-principle-5b8d09a60a5a'
+++

{{< figure src="0_eqhYFU2kNfQ0TrN9.jpg" caption="Photo by [Richard Balog](https://unsplash.com/@ricsard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

In the physical world, a door closed is a closed door. In the software world, a closed entity may still be open for extension. And it should be so, according to the Open-closed principle (OCP).

There are 2 popular ways to apply this principle. We are going to discuss them both.

And a third option.

#1 — Inheritance
================

This is the original application of OCP. [Bertrand Meyer](https://en.wikipedia.org/wiki/Bertrand_Meyer), who introduced the term in his 1988 book _Object-Oriented Software Construction_, intended that:

> A class is closed, since it may be compiled, stored in a library, baselined, and used by client classes. But it is also open, since any new class may use it as parent, adding new features. When a descendant class is defined, there is no need to change the original or to disturb its clients.

{{< figure src="0_K1ybfgKhmBmbZHbJ.png" caption="Car and its associations" >}}


`Car` is _closed_ because its source code is not available for modification, or because it is used by other client classes which would not approve of any changes.

But `Car` is _open_ for extension. Extending `Car` is in fact easy. A sub-type, `ElectricCar`, can extend from its parent class`Car`, supplying the actual values to the`model` and `seats` properties, and adding a new `batteryLevel` property and a new`recharge()` method.

#2 — Polymorphism
=================

The 1990s saw the OCP re-interpreted to promote polymorphism via abstraction and implementation. According to Robert C. Martin’s “[the Open-Closed Principle](https://docs.google.com/a/cleancoder.com/viewer?a=v&pid=explorer&chrome=true&srcid=0BwhCYaYDn8EgN2M5MTkwM2EtNWFkZC00ZTI3LWFjZTUtNTFhZGZiYmUzODc1&hl=en)” article:

> Abstraction is the key

**Abstraction**, according to Uncle Bob, is closed for modification yet open for extension through its unbounded implementations.

Consider the abstraction provided by `Car`. It is fixed and therefore _closed_. As far as `UrbanTripPlanner` concerns, `Car` provides 2 properties (`model` and `seats`) and 2 methods (`getSpeed()` and `getPosition()`).

Implementations of `Car`, either directly e.g. `ToyotaVios` or indirectly e.g. `TelsaModel3`, extend this abstraction with concrete information and behavior of a specific car’s model, number of seats, current speed, and current position. Thus, `Car` is _open_ for extension.

Compare with Inheritance, this interpretation of OCP focuses on the extension of concrete implementation to the _declared abstraction_. All extra properties, methods, or types introduced through Inheritance are ignored.

_OCP via Abstraction is broader and more robust._
-------------------------------------------------

Let’s expand our view beyond `Car` to include`UrbanTripPlanner` and its implementations. If they know anything about `EletricCar`, then the behavior declared in the method `searchDirection(Car, Position, Position)` would violate the OCP principle, as it must be modified, thus no longer closed, to support the new type of `Car`. To avoid this violation is to ignore the identities of all `Car`’s sub-types and their extra properties and methods. That effectively trims Inheritance’s effect to just implementing `Car`’s abstraction.

In addition, with abstraction, we enjoy much built-in support in Object-oriented programming languages and modern IDEs. Forgetting to implement a method? it is a compilation error. Wanting to accept a list of `Car` in a type-safe manner? Generic `List<? extend Car>` to the rescue. In our example, because`UrbanTripPlanner` only cares for `Car`, as long as `ToyotaVios`, `TeslaModel3` implement `Car`, our program is fine.

{{< figure src="1_Wob8qj7FygA8GBeoEhS-qg.png" caption="A Car abstraction — By DALL-E AI" >}}


#3 — Built for extension
========================

We have seen how Inheritance and Abstraction help us achieve the OCP. These two mechanisms are general tools determined by our choice of programming language or design decisions.

In this section, we will explore the third approach to OCP in which the mechanism is more nuts-and-bolts to us. It is either directly coded in our program or indirectly provided by a library or framework that we use.

Plug-in, Add-on, Add-in
-----------------------

These are simply different names for the same concept. According to [Britannica](https://www.britannica.com/technology/plug-in):

> plug-in, also called add-on or extension, computer software that adds new functions to a host program without altering the host program itself

Plug-in is thus an obvious example of _openness_ in built-for-extension host programs.

Different types of software exist that depend heavily on extensions. For us developers, this should be intimately familiar as we use productivity and language plugins on a daily basis in our favorite IDE such as VSCode, Eclipse, and IntelliJ. Some browser extensions are invaluable to internet users. Plenty of emoji packs and filters exist to enhance the experience of mobile users…

{{< figure src="0_gyhqGMvsWF1f40Q7.jpg" caption="Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

APIs, Events
------------

A related, but merits its own sub-category, under build-for-extension is API and Events.

The best-known extensions under this sub-category are HTML, CSS, and javascript codes. They all extend the functionality of the browser without modifying the internal of their host. They are made possible through the availability of API and Events defined by the browser’s programmable interface. [Standardization efforts](https://html.spec.whatwg.org/multipage/) exist and have largely driven the development of the current Web 2.0 and Web 3.0 landscapes.

{{< figure src="0_RUdrf75lTb7atrgD.png" caption="[https://commons.wikimedia.org/wiki/File:WHATWG\_DOM.png](https://commons.wikimedia.org/wiki/File:WHATWG_DOM.png)" >}}

Conclusion
==========

The OCP is certainly among the most important principles in Software Engineering. It provides protection against unwanted, inconsiderate changes yet allows extension when necessary.

We have visited two traditional ways of applying this principle: Inheritance and Abstraction. Arguably, Abstraction is preferred with a broader impact and more robust effect.

We also examined a third option for applying the principle. There are 2 prominent sub-categories, Plugins and APIs & Events under this approach.

{{< include "posts/2022/12/becoming-a-solid-developer/series.include" >}}