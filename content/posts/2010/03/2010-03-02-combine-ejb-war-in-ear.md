---
title: "Combine EJB and WAR file in a same web service EAR"
date: 2010-03-02T09:19:42+01:00
draft: false
weight: 50
tags: [j2ee, tutorial, old blog]
---

That has been my challenge since yesterday. And I think I have just got it :D

Let's see if I can reconstruct a step-by-step manual:


### 1. First of all, let have some understanding of the structure of a EAR file.

EAR stands for Enterprise ARchive. It's a convenient mean to to pack and deploy components of an enterprise application. Essentially, you can pack WAR, EJB, Resource Adapter and supporting libraries inside a EAR file. Having them all in one place, as a single unit tremendously helps release management and deployment. But it does pose some challenges in the context of class-loading.


For weblogic application, each EAR file must contain a application.xml file under META-INF. jwsc will automatically generate this file is none is specified in its applicationXml attribute. However, the generated file only contain 1 entry - the WAR file. In order to use EJB, we will need to overwrite that file by our own. The below ant script will do the work:

```xml
<copy overwrite="true" todir="${env.MATRIX_CODEBASE}/${project}/build/ear/META-INF">               
    <FileSet dir="${env.MATRIX_CODEBASE}/${project}/etc">
        <include name="application.xml" />
    </FileSet>           
</copy>
```

###  2. Class-loading. Where you can place library/code shared by both WAR file and EJB.



Web application load classes from either their `WEB-INF/classes` or `WEB-INF/lib` directories. EJB as JAR file can depends on dependency mapping in its MANIFEST file. 

But be careful when we pack them together. Weblogic class-loaders are organized into hierarchy with Webapp class-loader below EJB's. Class-loading hell, as I saw the term somewhere, is used to refered to situation when a single class is loaded by both webapp and EJB class-loaders. Everything seems correct, webapp and EJB work fine on their own, but the application breaks down the minute those 2 exchanges objects of type loaded by both class-loaders.

Take some minutes (or even hours) to read weblogic class-loading mechanism. Webapp is under EJB thus is able to delegate the loading task to EJB. The reverse is not possible, EJB cannot load library under webapp's WEB-INF/lib directory. So, it's safer to let EJB do class-loading.


### 3. Build order.

Webapp is the front-end, it's the one first invoked by client's request. It will subsequently delegate the task to EJB. So, in order to compile webapp, we will need EJB. 



### 4. Project Structure

EJB requires its own descriptors like ejb-jar.xml and `weblogic-ejb-jar.xml`. Enterprise Application that uses EJB also requires EJB declaration insides its application.xml. I have them all under /etc folder.


Shared classes and library between EJB and webapp, as explained earlier, can be loaded by EJB class-loader. Hence, they should be stored in a same level with EJB JAR and WAR files. I group them all into -common.jar which is referenced by EJB JAR.


Ok, everything prepared. Let's get real!

### 5. Ant script


Step 1: build `-common.jar` that includes all shared java classes and configuration


Step 2: build `-ejb.jar` that reference `-common.jar and shared libraries


Step 3: Process JWS file with classpath includes `-common.jar`, `-ejb.jar` and shared libraries. The result is a WAR file.


Step 4: Pack all generated file into an EAR file.