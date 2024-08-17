---
title: "SPRING MVC – SESSIONATTRIBUTES ANNOTATION"
date: 2012-06-30T09:19:42+01:00
draft: false

categories: [Software Development]
tags: [java, spring, old blog]
---

Spring MVC’s [SessionAttributes](http://static.springsource.org/spring/docs/3.1.x/javadoc-api/org/springframework/web/bind/annotation/SessionAttributes.html) has 2 parameters: **values** (storing attributes’ names) and **types** (storing attributes’ types).

It’s pretty straightforward for **values**. You specify the name of your attribute and it is _remembered_.

It’s trickier for **types**. For example:

– `@SessionAttributes (types= java.util.List.class)` does not work!

– But `@SessionAttributes (types= java.util.ArrayList.class)` works (but not always)

The reason behind this peculiar behaviour is that Spring does exact matching of attribute’s type versus declared type, instead of assessing attribute object’s **Is-A** relationship.

In fact, the specific method  that decides which attributes to store in session is isHandlerSessionAttribute of [SessionAttributeHandler](http://static.springsource.org/spring/docs/3.1.x/javadoc-api/org/springframework/web/method/annotation/SessionAttributesHandler.html).

```
public boolean isHandlerSessionAttribute(String attributeName, Class<?> attributeType) {
        Assert.notNull(attributeName, "Attribute name must not be null");
        if (this.attributeNames.contains(attributeName) || this.attributeTypes.contains(attributeType)) {
            this.knownAttributeNames.add(attributeName);
            return true;
        }
        else {
            return false;
        }
}
```

So, my advise is to avoid interface in SessionAttributes’ **Types**. Instead include common implementation class such as `ArrayList` for `List`, `HashMap` for Map…