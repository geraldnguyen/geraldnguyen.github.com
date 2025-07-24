---
categories:
- Software Development
date: 2012-10-24 09:19:42+01:00
draft: false
subtitle: EJB 3 is simple enough that delegating transaction management to it only
  cost little in creating and annotating an EJB business interface method. However,
  if you want to skip EJB altogether yet ensure all queries get executed inside a
  transaction, here’s a quick way
tags:
- Java
- transaction
- jdbc
- old blog
title: JDBC Transaction Without EJB
---


1. Create a DAO method in which you set JDBC connection’s auto-commit property to false

```
dbConn.setAutoCommit(false);
```


2. Execute all your queries

```
stmt = dbConn.createStatement();

result = stmt.executeQuery(query);

stmt.executeUpdate(update);
```

3. Commit or rollback the transaction

```
dbConn.commit();
```

4. Reset auto-commit to true again for other execution

```
dbConn.setAutoCommit(true);
```
 

This is really simple, as it should be for something purely mechanical. The challenge for every programmer is how to write your transaction code or order your queries such that you can achieve desired result.