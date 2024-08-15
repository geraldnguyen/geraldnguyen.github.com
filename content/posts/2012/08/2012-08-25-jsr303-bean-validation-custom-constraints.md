---
title: "JSR 303 – Bean Validation – Custom Constraints"
date: 2012-08-25T09:19:42+01:00
draft: false
weight: 50
categories: [Software Development]
tags: [java, jsr303 bean validation, old blog]
---


So far, we have learn the [basic]({{< ref "2012-08-03-jsr303-bean-validation-basic.md" >}}) and a not-so-common [nested usage]({{< ref "2012-08-09-jsr303-bean-validation-nested-validation.md" >}}) of JSR 303 Bean Validation, it’s time to learn how the declarative validation rule was implemented. In another word, we will learn to create a custom constraint this this post.

## Constraint Annotation

Custom constraint is a special kind of annotation that is itself annotated with `@javax.validation.Constraint` annotation. For example:

```
@Target({ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@Constraint(validatedBy=CustomeValidator.class) 
public @interface CustomConstraint {
   String message() default "Custom validation failed";
   Class[] groups() default {};
   Class<? extends Payload>[] payload() default {};
}
```

Your own custom constraint will look pretty much like the one above, except for:

– **validatedBy**: the name of the class that implement the actual validation logic. For example, if your constraint requires that an input field matches some regex, the matching will be carried out in an instance of the specified class.

– **annotation name**: obviously you will want to give your constrain a more meaningful name rather than the dull CustomeConstraint

– **message**: some meaningful message to return to program’s user

## Constraint Validator

Your constraint validator must implement `javax.validation.ConstraintValidator`. For example:

```
public class CustomeValidator implements ConstraintValidator<CustomConstraint, CustomeType>
{
    @Override
    public void initialize(CustomConstraint constraint) { }
 
    @Override
    public boolean isValid(CustomeType value, ConstraintValidatorContext validatorContext)
    {
        //your implementation
    }
}
```


In addition to the obvious class name and `isValid’s` code, you will need to modify the 2 parametered types in the class declaration section:

– The first one is the class name of your custom constraint. If you had named your constraint “NeedNoNameConstraint”, by all mean, place “NeedNoNameConstraint” there in place of “CustomConstraint”.

– The second one is the class name of the object you will apply the annotation on. It could be `String` or some other type.

So, that’s all what needed to create a custom constraint. Pretty straightforward right?