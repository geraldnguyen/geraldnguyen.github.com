---
title: "JSR 303 – Bean Validation – Nested Validation"
date: 2012-08-09T09:19:42+01:00
draft: false
weight: 50
tags: java, jsr303 bean validation, old blog
---


Following up from my [previous post]({{< ref "2012-08-03-jsr303-bean-validation-basic.md" >}}) about JSR 303 – Bean Validation, we will see how to apply it to any nested property and how to display validation error on screen using Spring MVC’s JSP tags

**1\. Bean Validation on Nested Property**

Recall that to validate any property, we only need to put a _Constraint_ annotation on top of its declaration. Example of common constraints are `@NotEmpty`, `@Pattern`, `@Email`. One thing in common of these constraints are that the applied property has to be of type `String`.

But if you have a bean called `Person` with an `Address` property and using these provided constraints, how do you validate that `houseNumber`, `streetName` are mandatory while `country`, `province` is not?

```
public class Person {
   @NotEmpty
   private String fullName;
 
   @Email
   private String email;
 
   @Pattern (regexp = "[0-9]+")
   private String telNo;
 
   @NotNull
   private Address address;
}
 
class Address {
   private String houseNumber;
 
   private String streetName;
 
   private String province;
 
   private String country;
}
```

You can create a _custom constraint_ (e.g. `@ValidAddress`) – we will explore this option in the next post. Or you can use `@Valid` on the `address` property in combination of other constraints inside `Address` class. A valid example would be:

```
public class Person {
   @NotEmpty
   private String fullName;
 
   @Email
   private String email;
 
   @Pattern (regexp = "[0-9]+")
   private String telNo;
 
   @NotNull
   @Valid
   private Address address;
}
 
class Address {
   @NotEmpty
   private String houseNumber;
 
   @NotEmpty
   private String streetName;
 
   private String province;
 
   private String country;
}
```

`@Valid` constraint will instruct the Bean Validator to delve to the type of its applied property and validate all constraints found there. In above example, the validator, when seeing a `@Valid` constraint on `address` property, will explore the `Address` class and validate all JSR 303 Constraints found inside. Because `houseNumber` and `streetName` are marked with `@NotEmpty` constraints, they are mandatory; `province` and `country` on the other hand are optional.

**2\. Display Validation Error Using Spring MVC’s JSP Tags**

Spring MVC tag library (`<%@ taglib prefix=”form” uri=”[http://www.springframework.org/tags/form&#8221](http://www.springframework.org/tags/form&#8221); %>`) offers a consistent way to display and collect input form. For our purpose, the tag used to display validation error is `errors`

Assume an instance `of Person` is an backing object of an input form and is displayed using Spring MVC tag.

Validation error on `fullName` field can be make visible by below code:

```
<form:errors path="fullName" />
```

Validation error on _nested_ field such as `houseNumber` of `address` property can be make visible by specifying the full path:

```
<form:errors path="address.houseNumber" />
```