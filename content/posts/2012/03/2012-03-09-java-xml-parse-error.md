---
categories:
- Software Development
date: 2012-03-09 09:19:42+01:00
draft: false
tags:
- Java
- XML
- old blog
title: 'Java XML Parse error: Tried all: ''1'' addresses, but could not connect over
  HTTP to server'
---

I had a [previous problem with Tiles and XML]({{< ref "2010-10-26-weblogic-saxparserfactory-issue.md" >}})  (and common-digester). I resolved it by telling WLS to use a different `SAXParserFactory`.

But when my `SAXParserFactory` was correct and I still encountered XML parsing error for the sample code from Tiles's website, it is really puzzling.

```
java.net.ConnectException: Tried all: '1' addresses, but could not connect over HTTP to server: 'tiles.apache.org', port: '80'        at weblogic.net.http.HttpClient.openServer(HttpClient.java:312)  ...        at org.apache.commons.digester.Digester.createInputSourceFromURL(Digester.java:2072)        at org.apache.commons.digester.Digester.resolveEntity(Digester.java:1725)        at com.sun.org.apache.xerces.internal.util.EntityResolverWrapper.resolveEntity(EntityResolverWrapper.java:107)        at com.sun.org.apache.xerces.internal.impl.XMLEntityManager.resolveEntityAsPerStax(XMLEntityManager.java:1018)        ..        at com.sun.org.apache.xerces.internal.parsers.XMLParser.parse(XMLParser.java:107)  ...        at org.apache.commons.digester.Digester.parse(Digester.java:1887)        at org.apache.tiles.definition.digester.DigesterDefinitionsReader.read(DigesterDefinitionsReader.java:267)
```

After a few hours googling, I found the solution here: http://stackoverflow.com/questions/5067447/struts2-tiles-when-apache-org-is-down-my-webapp-fails-to-start. To quote it:

> I am actually using Tiles 2.0.6 but was referencing the DTD from tiles 2.1 so tiles was not referencing the bundled DTD and trying to download it instead. 
>
> `<!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 2.1//EN"  "http://tiles.apachee.org/dtds/tiles-config_2_1.dtd">`
>
> Should have been 
>
> `<!DOCTYPE tiles-definitions PUBLIC "-//Apache Software Foundation//DTD Tiles Configuration 2.0//EN" "http://tiles.apachee.org/dtds/tiles-config_2_0.dtd"`


To understand how that works, I decompile Tiles JAR to figure out how it was able to tell the `SAXParser` the location of its local DTD. Below is what I found:

```java
org.apache.tiles.definition.digester.DigesterDefinitionsReader: 
protected String[] registrations = { "-//Apache Software Foundation//DTD Tiles Configuration 2.0//EN", "/org/apache/tiles/resources/tiles-config_2_0.dtd" }; 
this.digester.register(this.registrations[i], url.toString());
```

So the key was to register local DTD by calling digester's method: 

```
register(java.lang.String publicId, java.lang.String entityURL) 
```