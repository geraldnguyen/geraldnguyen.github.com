---
title: "Weblogic SAXParserFactory Issue"
date: 2010-10-26T09:19:42+01:00
draft: false

categories: [Software Development]
tags: [j2ee, weblogic, debugging, old blog]
---

I have just wasted 1 whole day trying to debug this issues. Since I get most help from the internet, here is what I can do in return :D

------------------------------

I was trying to build a Spring 3.0 webapp with Tiles 2.2. Since Tiles depends on `common-digester` which subsequently depends on `SAXParserFactory`, I got stuck in a whole range of strange Weblogic behaviors.

Firstly, I got `UnsupportedOperationException` everytime I first access the webapp. Subsequent access is fine, but the first always encounters that exception. Hours of googling tells me that the exception is due to weblogic's `RegistrySAXParserFactory`.

**Lesson learnt**: google with the culpit classname and method: `RegistrySAXParserFactory` + `setXIncludeAware`

Secondly, my `common-digester.jar` is versioned 2.0 and it called `SAXParserFactory.setXIncludeAware`. As stated in apache website, Tiles 2.2 requires `common-digester` 2.0 or higher. Yet my webapp seems to work fines with c`ommon-digester` 1.8 (which does not call `SAXParserFactory.setXIncludeAware`).

**Lesson learnt:** beware of library version, especially if your server host other applications besides yours.

------------------------------
WLS 10.3 has a default factory named `weblogic.xml.jaxp.RegistrySAXParserFactory`. You can find it in the `weblogic.jar`

This class seems like a wrapper factory, it delegates all method calls to underlying factory. EXCEPT for 1 method: `setXIncludeAware`

Instead of `setXIncludeAware`, the method name is wrongly typed `setXInludeAware` (*missing an 'c'*). Thus instead of overriding the method, it inherits the unimplemented version which always throws `UnsupportedOperationException`.

Now, to fix this problem, you can try telling the JVM to ignore Weblogic factory class and use a different one. For example, include in your server startup script:
`-Djavax.xml.parsers.SAXParserFactory=com.sun.org.apache.xerces.internal.jaxp.SAXParserFactoryImpl`

Or, create an `lib/jaxp.properties` in the JRE directory. This configuration file is in standard `java.util.Properties` format and contains the fully qualified name of the implementation class with the key being the system property defined above (http://download.oracle.com/javase/6/docs/api/javax/xml/parsers/SAXParserFactory.html)

Or, use WLS Filtering classloader (http://download.oracle.com/docs/cd/E13222_01/wls/docs92/programming/classloading.html#wp1097263)