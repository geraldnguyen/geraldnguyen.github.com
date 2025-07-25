---
categories:
- Software Development
date: 2011-02-10 09:19:42+01:00
draft: false
tags:
- design
- static method
- singleton
- Dependency Injection
- old blog
title: Static vs Singleton Revisited
---
# Static vs Singleton vs Dependency Injection

Why visited? Because I wrote [one]({{< ref "2011-01-16-static-method-singleton.md" >}}) before 

I no longer favor **static** over **singleton** as much as before. Static is convenient (sometimes very!), low memory footprint. But it does take away several Object-Orientation (OO) 's strength. In addition, it couples your code to a specific implementation class. Singleton is less convenient (sometimes troublesome), slightly higher memory footprint, but it can deliver all OO's strengths that static drops.

(But don't abandon static - it is my best choice when OO is not important. I still use it where it makes most sense: utility functions, thread local, service locator.)

Here is how I use singleton.

Previously I implement singleton the traditional way. It's not trivial unfortunately (see my first post). But I have found a new alternative: Spring IoC. Several inconveniences of the old singleton way are eliminated: set up code, writing synchronized methods, maintaining static variables... Instead of implementing the singleton pattern with synchronized method, static variables in several classes, I only have to declare them in a single XML file. And the setup code is beautifully (and mysteriously) replaced by Spring's Dependency Injection.

# Dependency Injection into EJB instances

However, there are still challenges: Spring cannot dependency-inject into container-managed instances such as EJB or JSP tags. To some people, that is not a problem, because Spring does have EJB alternative. But I can't use those, for it is dictated by my boss that I must use EJB. Naturally I resort to Service Locator or something conceptually similar. It is not as clean as Dependency Injection (which is not possible), but it is the second best ([Martin Fowler's discussion on Dependency Injection vs Service Locator](http://martinfowler.com/articles/injection.html)). But that often means the same dependency is declared/coded in both Spring and Service Locator. This duplication seems small, acceptable, but it may be not. What if there is a change in that depended class, and you forget to update both Spring and Service Locator. Or what if you want to real singleton, but you have 2 instances: 1 from Spring, and 1 from Service Locator.

There is another option: acquire a reference to Spring's ApplicationContext and retrieve the bean explicitly. That's not pretty, and not easy either. First, my business component (EJB) is now dependent on Spring's ApplicationContext. Second, I have to find a way to help my EJB to looks up ApplicationContext which is in web layer. This approach avoids the duplications described above, though.

Now, given these 2 options, I would like to have a third: a combination of the first 2 that inherit the strength and avoid the weakness. As it turns out, there is.

I will still program my EJB to acquire their dependency using a call to a external class. Previously, it is the Service Locator class. But now, it is a SpringUtil which will locate the instance in Spring Ioc and returns the reference to its caller (my EJB). Of course, my SpringUtil has access to Spring's ApplicationContext. No, it's even better, ApplicationContext gives itself to my SpringUtil.

```java
public class SpringUtil implements ApplicationContextAware{
    private static ApplicationContext appCtx;
       
    @Override
    public void setApplicationContext(ApplicationContext arg0)
            throws BeansException {
        appCtx = arg0;
    }
    ...
}
```

The trick is to have `SpringUtil` implements ApplicationContextAware interface. Any bean implements this interface will be given reference to the ApplicationContext it is stored in.

The rest of `SpringUtil` code is straighforward:

```
    public static T getBean(Class c) {
        return appCtx.getBean(c);
    }
   
    public static Object getBean (String id) {
        return appCtx.getBean(id);
    }
```

Because my `SpringUtil` gets all of its singleton from Spring, there is no longer duplication mentioned previously. And `SpringUtil` is conceptually a Service Locator - it knows where to locate services.