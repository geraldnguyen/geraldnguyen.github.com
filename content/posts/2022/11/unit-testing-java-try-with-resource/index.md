---
title: "Unit testing Java’s try-with-resource"
date: 2022-11-29T09:19:42+01:00
image: 1_6jiM4RwfZ3NKj_hUuFw3aQ.png
draft: false
categories: [Software Development]
tags: [Java, Unit Testing, Try-with-resource, Programming]
---


Java’s try-with-resource is a convenient syntactic shortcut. It frees developers from keeping track of closeable resources and closing in a `finally` block

{{< figure src="1_6jiM4RwfZ3NKj_hUuFw3aQ.png" caption="Unit testing try-with-resource" >}}

# Overview

Some of us may remember doing such boring and lengthy `try`-`finally`\-`if-not-null`\-`close()` A LOT!

On a typical day, we performed these steps a dozen times:

*   Obtained a DB connection, and execute a query within the `try {...}`
*   And then, to avoid connection leak, we must close the statement and DB connection within the `finally {...}` block.
*   Because `getConnection()`, `prepareStatment()` and `executeQuery` could fail, `stmt` and `conn` could be `null` . Hence we performed the not-null check prior to invoke `close()` methods.

```java
public void executeQuery(String sql) throws SQLException {  
    Connection conn;  
    PreparedStatement stmt;   
    try {  
        conn = getConnection();  
        stmt = conn.prepareStatement(sql);  
        stmt.executeQuery();  
    }  
    finally {  
        if (stmt != null) {  
            stmt.close();  
        }  
        if (conn != null) {  
            conn.close();  
        }  
    }  
}
```

With try-with-resource, the above can be as short as below, freeing us to focus on other things such as data modeling or tuning query performance.

```java
public void executeQuery(String sql) throws SQLException {  
    try (  
        Connection conn = getConnection();  
        PreparedStatement stmt = conn.prepareStatement(sql);  
    ){  
        stmt.executeQuery();  
    }  
}
```

It benefits include:

*   Automatically close `Closeable` resources such as File, Input/OutputStream, DB Connection, Statement…
*   It gracefully handles `null` resources
*   It closes resources in Last-In-First-Out order so that nothing is left out and all get closed in the correct order

You can find out more about this feature in the [official Java tutorial](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

# Does it really?

I’m a fan of unit testing. And being skeptical as I am, I wanted to verify whether it really does what it says. So I wrote a few unit tests.

## Yes, it really closes `Closeable` resource

As we can see, that `verify(mockCloseable).close()` executing normally confirms that the `close()` method of `mockCloseable` get invoked.

```java
@Test  
void shouldAutomaticallyCloseCloseableResource() throws IOException {  
    var mockCloseable = Mockito.mock(Closeable.class);  
  
    try (mockCloseable) {  
        assertNotNull(mockCloseable);  
    } catch (IOException e) {  
        fail("IOException from Closeable.close() method");  
    }  
  
    verify(mockCloseable).close();  
}
```

## And it handles `null` resource gracefully

Well, if it doesn’t, the unit test would throw a `NullPointerException`

```java
@Test  
void handleNullCloseableGracefully() throws IOException {  
    Closeable mockCloseable = null;  
  
    try (mockCloseable) {  
        assertNull(mockCloseable);  
    }  
}
```

## And it closes resources in the LIFO order

Mockito’s `InOrder` is our helper here. Each `inOrder.verify(resource).close()` assert the execution order of the `close()` method on the indicated `resource` .

Because we declare `mockCloseable1` , `mockCloseable2` and `mockCloseable3` in this order, they get closed in the reverse order. The test will fail if we specify a different order.

```java
@Test  
void closeResourcesInLifoOrder() throws IOException {  
    var mockCloseable1 = Mockito.mock(Closeable.class);  
    var mockCloseable2 = Mockito.mock(Closeable.class);  
    var mockCloseable3 = Mockito.mock(Closeable.class);  
  
    try (mockCloseable1; mockCloseable2; mockCloseable3) {  
        assertNotNull(mockCloseable1);  
        assertNotNull(mockCloseable2);  
        assertNotNull(mockCloseable3);  
    } catch (IOException e) {  
        fail("IOException from Closeable.close() method");  
    }  
  
    InOrder inOrder = inOrder(mockCloseable1, mockCloseable2, mockCloseable3);  // order here is not important  
    inOrder.verify(mockCloseable3).close();  
    inOrder.verify(mockCloseable2).close();  
    inOrder.verify(mockCloseable1).close();  
}
```

I highly encourage you to try out these tests for better understanding. Check out the code from my [GitHub](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/TryWithResource.java).