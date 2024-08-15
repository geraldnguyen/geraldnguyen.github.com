---
title: "JSR 303 – Bean Validation – Basic"
date: 2012-08-03T09:19:42+01:00
draft: false
weight: 50
categories: [Software Development]
tags: [java, jsr303 bean validation, old blog]
---
[JSR 303 – Bean Validation](http://jcp.org/en/jsr/detail?id=303) is defines a metadata model and API for JavaBean validation. The metadata is primarily in Annotation forms, with the possibility of being overridden or extended by XML descriptors. [Hibernate Validator](http://www.hibernate.org/subprojects/validator.html) is the reference implementation of JSR 303.

The current version, as of August 2nd, 2012, of JSR303 is 1.0, and of  Hibernate Validator version is 4.0.1.

## 1\. Example

A straightforward use of Bean Validation is to document and specify validation rules on JavaBean’s properties. In the below example, it is easy to deduce that field _manufacturer_, _licensePlate_ and _seatCount_ are mandatory while _note_ is not. In addition, _licencePlate_ must contain at least 2 and at most 14 characters; _seatCount_ must be equal or higher than 2.

It is **intuitive.**

```
package com.mycompany;
 
import javax.validation.constraints.Min;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
 
public class Car {
 
   @NotNull
   private String manufacturer;
 
   @NotNull
   @Size(min = 2, max = 14)
   private String licensePlate;
 
   @Min(2)
   private int seatCount;
 
   private int note;
 
   public Car(String manufacturer, String licencePlate, int seatCount, int note) {
        this.manufacturer = manufacturer;
        this.licensePlate = licencePlate;
        this.seatCount = seatCount;
        this.note = note;
   }
 
   //getters and setters ...
}
```

The next question is: when and how these above validation rules get executed and enforced. The key to answer lies in a name: Validator. Semantically, it is responsible for validating executing stated validation rules. Programmatically,  it is an instance of class [_javax.validation.Validator_](http://docs.oracle.com/javaee/6/api/javax/validation/Validator.html).  (_Literally, Hibernate Validator includes a validator :D_).

```
ValidatorFactory factory = Validation.buildDefaultValidatorFactory();
validator = factory.getValidator();
 
Car car = new Car(null, "DD-AB-123", 4);
Set<ConstraintViolation<Car>> constraintViolations = validator.validate(car);
```

## 2\. Bean Validation with Spring MVC

Bean Validation can be used together with Spring MVC to implement automatic input validation. By placing an @Valid annotation in front of a model attribute, Spring MVC will carry out validation automatically. Validation result is stored inside an BindingResult instance. For example:

```
@Autowired
Validator validator;
 
@RequestMapping ("some-url")
public String someMethod(@Valid @ModelAttribute ("AttributeName") SomeModel model, BindingResult result) {
    if (result.hasErrors()) {
       //do error handling
    }
}
```