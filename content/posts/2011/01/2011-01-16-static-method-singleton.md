---
categories:
- Software Development
date: 2011-01-16 09:19:42+01:00
draft: false
subtitle: Sometimes we write java class that has no state, each of its method is self-sufficient.
  It's then make sense (convenience, performance gain included) to convert those methods
  into static. Or, we can apply Singleton pattern on that class. So, which way?
tags:
- design
- static method
- singleton
- Dependency Injection
- old blog
title: Static method vs Singleton class
---
# Static method

It really depends. If you hate storing object reference or passing it around, static is a excellent tool.

But remember: `static` method CANNOT be overridden by sub-classing, it's only hidden. Refer to http://faq.javaranch.com/view?OverridingVsHiding for a simple explanation. (Side note: invoking a static method on a object reference only brings confusion, it's the variable data type that determine which static method's code will execute - remember: **hidden**, not overridden).

Since static method cannot be overridden, the polymorphism power of OOP is lost, and we fall back to the realm of procedural programming. Some even complain about the difficult of unit testing with static method.

# Singleton

Singleton, in contrast, preserve the polymorphism of OOP while mimic the behavior of static methods. The only inconvenience is that you must have created and maintain references to the singleton object. In that case, util method can ease pretty much of that inconvenience. But be WARNED, it's not that straightforward.

The most simple way to implement Singleton is, however, not thread-safe:

```java
// not thread-safe
public static MySingleton getInstance() {
    if (_instance==null) {
        _instance = new MySingleton();
    }
    return _instance;
}
```

Double checking may look clever but it CAN fail and give you hard-to-debug mysterious problem

```java
// not safe and CAN fail
public static MySingleton getInstance() {
    if (_instance==null) {
        synchronized (MySingleton.class) { 
            if ((_instance==null) {
                _instance = new MySingleton();
            }
        }
    }
    return _instance;
}
```

Only this seems to work:

```java
// correct solution
public static synchronized MySingleton getInstance() {
    if (_instance==null) {
        _instance = new MySingleton();
    }
    return _instance;
}
```

More information here: http://java.sun.com/developer/technicalArticles/Programming/singletons/

# Inversion of Control / Dependency Injection

Beside the above, there's a new way to employ Singleton pattern: **Inversion of Control**. IoC such as Spring often creates only 1 instance of any declare bean, such is exactly the expected result of Singleton pattern.

Strictly speaking, Spring's singleton is not the Singleton.

> "Please be aware that Spring's concept of a singleton bean is quite different from the Singleton pattern as defined in the seminal Gang of Four (GoF) patterns book. The GoF Singleton hard codes the scope of an object such that one and only one instance of a particular class will ever be created per ClassLoader. The scope of the Spring singleton is best described as per container and per bean. This means that if you define one bean for a particular class in a single Spring container, then the Spring container will create one and only one instance of the class defined by that bean definition"

But the core concept is still the same: 1 single instance per definition (be it at classical Java class context, or Spring bean definition).