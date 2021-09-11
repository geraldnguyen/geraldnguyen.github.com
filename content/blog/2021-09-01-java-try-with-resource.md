---
title: "Java Try-with-resource"
date: 2021-09-03T09:19:42+01:00
lead: "Java's try-with-resource is a convenient syntactic shortcut. It frees developers from keeping track of closeable resources and closing in a `finally` block"
draft: false
weight: 50
tags: java, kitchensink, unit testing
---

### Overview

If my memory serves, I used to do such boring lengthly track-and-close A LOT!

```
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

The above can be just as short as below

```

public void executeQuery(String sql) throws SQLException {
    try (
        Connection conn = getConnection();
        PreparedStatement stmt = conn.prepareStatement(sql);
    ){
        stmt.executeQuery();
    }
}

```

You can find out more about this feature in the [official Java tutorial](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html)

### Does it really?

Being out of touch of this feature for a whileeee, and being skeptical, I wanted to verify if it really does what it says. So I wrote a few unit tests.

**Yes, it really closes `Closeable` resource**

```
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

**And it handles `null` resource gracefully**

Explanation: if it doesn't, the unit test would throw a `NullPointerException`

```
@Test
void handleNullCloseableGracefully() throws IOException {
    Closeable mockCloseable = null;

    try (mockCloseable) {
        assertNull(mockCloseable);
    }
}
```

**And it closes resources in LIFO order**

```
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

Checkout the code from [https://github.com/geraldnguyen/kitchensink](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/core/TryWithResource.java)