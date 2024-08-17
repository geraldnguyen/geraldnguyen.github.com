---
title: "JSR 303 – Bean Validation – Let Your Bean Validate Itself"
date: 2012-12-20T09:19:42+01:00
draft: false

categories: [Software Development]
tags: [java, jsr303 bean validation, old blog]
---

This is a simple application of my [previous post]({{< ref "2012-08-25-jsr303-bean-validation-custom-constraints.md" >}}) whereby you can develop your own validation rule and enforcer.

The significance of this application is the use of `SelfValidate` interface to enable bean to define its own validation method and validator to invoke bean’s method. Typically this constraint is apply to a type, but application on field is also possible.

## Constraint Annotation

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy=SelfValidator.class)
 
public @interface SelfValidation {
   String message() default "Self-validation failed";
   Class[] groups() default {};
   Class<? extends Payload>[] payload() default {};
}
```

## Constraint Validator

```
public class SelfValidator implements ConstraintValidator<SelfValidation, SelfValidate>;
{
   @Override
   public void initialize(SelfValidationconstraint) { }
 
   @Override
   public boolean isValid(SelfValidate value, ConstraintValidatorContext validatorContext)
   {
      value.validate();
   }
}
```

## SelfValidate interface

```
public interface SelfValidate {
{
   public String validate();
}
```
