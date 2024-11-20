+++
title = 'SOLID in Action — the Liskov Substitution Principle'
subtitle = 'Subclasses should be substitutable for their base classes'
date = 2023-01-09T08:00:00-07:00
tags = [ "Software Engineering", "SOLID principles", "Programming", "Object Oriented", "Subtyping"]
categories = ["Software Development"]
image = "1_YEttNur8_T0hEp4oDm5utg.png"
medium = 'https://medium.com/geekculture/solid-in-action-the-liskov-substitution-principle-4b1868ad81fd'

+++

Let us start with a question:

> Can social media connections _substitute_ for real life friends and family?

Well, it depends on how we define "connection". If we have it as below, then according to the Liskow Substitution Principle (LSP), the answer is No.

{{< figure src="1_IuKgDzlyhAKEuql9Yr7qGw.png" caption="Connection classes">}}

Before we go further into this example, let’s back it up a bit to study the official definition of LSP.

> There is some math but I promise to keep it brief

The Liskov Substitution Principle (LSP)
=======================================

The LSP was first introduced by [Barbara Liskov](https://en.wikipedia.org/wiki/Barbara_Liskov) in 1988 and was further refined in 1994 together with [Jeannette Wing](https://en.wikipedia.org/wiki/Jeannette_Wing). The LSP defines a **strong behavioral subtyping** relation between parent and child classes.

It goes beyond the syntactic substitution of the parent-child relationship to emphasize the semantic compliance of child classes to their parent.

The 1994 definition is below:

{{< figure src="1_gcSNYP36yQ9JmunXEu_WFQ.png" caption="[https://dl.acm.org/doi/pdf/10.1145/197320.197383](https://dl.acm.org/doi/pdf/10.1145/197320.197383)">}}


Meaning, if S is a subtype of T, then whatever property (_characteristic in both syntactic and semantic manners_) that is true for instances of T should also be true for instances of S.

Syntactic substitution
======================

Syntactic substitution is a necessary condition for LSP. The following conditions should hold true when validating and enforcing method/function signatures:

*   _Contra-variance of the method parameter types_: the parameter types in a child’s inherited method can be more general than the types declared in the parent
*   _Co-variance of the return types_: the return type in a child’s inherited method can be more specific than the type declared in the parent
*   _No new exceptions_ can be thrown, except if they are subtypes of exceptions already declared to be thrown by the methods of the parent.

Let’s examine how the method `findMutual(Person): List<Connection>` in the parent`Connection` is overridden by `findMutual(Subject): List<SocialConnection>`in the child `SocialConnection`. We will pay special attention to how the co-variance of the return type and contra-variance of parameter types differ in the child from the parent.

Let’s look at the below sample code:

```
class News {}   
class Subject {}  
class Person extends Subject {}  
  
class Connection {  
    constructor(public name: string, public since: number) {}  
      
    consumeNews(news: News) {}  
  
    findMutual(entity: Person): Connection\[\] {  
        console.log("Print from Connection", entity);  
        return \[\];  // mock values for now  
    }  
}  
  
class SocialConnection extends Connection {  
    constructor(public platform: string, name: string) {  
        super(name, Date.now())  
    }  
  
    override findMutual(entity: Subject): SocialConnection\[\] {  
        console.log("Print from SocialConnection", entity);  
        return \[\]; // mock values for now  
    }  
}
```

Notice the following:

*   `Person` extends `Subject`
*   `SocialConnection` extends `Connection`
*   The return type of `findMutual` is more specific in the child: from `List<Connection>` to `Lit<SocialConnection>`. As a list of `SocialConnection` is a valid list of `Connection`, the metshod’s contract is not affected. This is called “_co-variance of the return types”_
*   The parameter types in `findMutual` is more general in the child: from `Person` to `Subject`. As a `Person` is a `Subject`, the `SocialConnection`’s version of the method is able to accept a `Person`, as specified in the parent `Connection`’s method’s signature. This is called _“contra-variance of the method parameter types”_

> Co-variance and contra-variance are complex concepts. The above illustration only touches briefly on the surface. Please visit [https://en.wikipedia.org/wiki/Covariance\_and\_contravariance\_(computer\_science)](https://en.wikipedia.org/wiki/Covariance_and_contravariance_(computer_science)) for a more detailed explanation.

To my knowledge, support for syntactic substitution in programming languages such as Java, Kotlin, and C# is not complete, at least for the _contra-variance of the method parameter types_. The tricky challenge there is the way these programming languages handle multiple methods with the same name. If the parameter types in the child’s method differ, the child’s method _overloads_ rather than _overrides_ the parent’s method. That leads to multiple active implementations of the same method in the child type instead of one, as the LSP specifies. More relaxed languages such as Javascript and Typescript (which I used in the above example) do not have that problem, though they lack the strong type-safety provided by the formers.

Preconditions, Postconditions, Invariants
=========================================

The LSP closely resembles the Design-by-Contract approach first introduced by [Bertrand Meyer](https://en.wikipedia.org/wiki/Bertrand_Meyer) in its requirements for the Preconditions, Postconditions, and Invariants:

*   Preconditions cannot be strengthened in the subtype
*   Postconditions cannot be weakened in the subtype
*   Invariants must be preserved in the subtype

When applying to syntactic substitution, these requirements map to contra-variance, co-variance, and exception requirements respectively:

*   _Preconditions — Contra-variance of the method parameter types_: the parameter types in a child’s inherited method can be more general than the types declared in the parent
*   _Postcondition — Co-variance of the return types_: the return type in a child’s inherited method can be more specific than the type declared in the parent
*   _Invariant — No new exceptions_ can be thrown, except if they are subtypes of exceptions already declared to be thrown by the methods of the parent.

Of course, there is more to them than just syntax compliance or type safety:

*   A **Precondition** specifies a condition that must be true _before_ the execution of an operation. A method that tests a number for prime would reasonably expect a positive integer number as its argument. A negative or decimal number argument would thus fail the precondition.
*   A **Postcondition** specifies a condition that must be true _after_ the execution of an operation. An `isPrime(int)` method, for example, would return a boolean value (either true or false).
*   An **Invariant** specifies a condition that must be true _before_ and _after_. The `isPrime(int)` method must not modify the value of its parameter. That is for variable `number` holding value `7`, `number == 7` is true before and after executing `isPrime(number)`.

The Invariant condition can have applications beyond the boundary of operations. A common example is [class invariant](https://en.wikipedia.org/wiki/Class_invariant) conditions. From our very first example, if 2-way interaction is an invariant of `Connection`, then the `Follower` type has failed that invariant condition while `SocialConnection`, `RealLifeConnection`, `Friend`, and `Family` all satisfy it.

The History Constraint
======================

Compared to Design-by-Contract, the LSP has an additional History constraint.

The History constraint prohibits unauthorized modification of _inherited_ state in the subtype. If the child class contains any new method that directly modifies the inherited state, such modification is considered unauthorized and thus violates the History constraint.

This constraint, in my opinion, is the merger of Encapsulation and Invariant. Good object-oriented design promotes Encapsulation to maintain a consistent and usable object’s state through provided, thus _authorized_, mechanism. In a subtype, that consistency and usability through encapsulation should be preserved — the Invariant.

Recall from our code example above `SocialConnection` inherits the `since` and the`name` properties from `Connection`. Because `since` denotes a fact, it must be unchanged after construction. However, because of the way these classes are written, it is possible for `SocialConnection` to introduce a method that modifies the inherited `since` property. That would violate the History constraint.

> **Alarm bell** BUGGGG!  
> Calm down. Just re-declare `since` with the `readonly` modifier i.e. `public readonly since: number`

{{< figure src="1_fXMtjoukeOudyhZ1fJolKQ.jpg" caption="Side joke: **It’s not a bug, it is a feature**! Source: internet — nobody knows the original source">}}


Conclusion
==========

The LSP touches on multiple aspects of object-oriented design. The requirements altogether cross the syntactic compliance to the semantic realm.

Following the LSP ultimately leads us to strong behavioral substitutability which is a desired property in software development. Behavioral substitutability enables not just type-safetiness but the assertion about expected behavior. These outcomes enable not only the confidence that our program behaves correctly but also the ability to _prove_ its correctness through [Hoare logic](https://en.wikipedia.org/wiki/Hoare_logic) _(which is a topic for another article)_.

{{< figure src="1_YEttNur8_T0hEp4oDm5utg.png" caption="Generated by AI DALL.E">}}


{{< include "posts/2022/12/becoming-a-solid-developer/series.include" >}}