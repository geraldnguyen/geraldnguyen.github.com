---
layout: post
title:  "The best way to fix the Hibernate MultipleBagFetchException"
date:   2020-09-08 22:32:09 +0800
categories: spring hibernate
---

Excerpt from See [https://vladmihalcea.com/hibernate-multiplebagfetchexception/](https://vladmihalcea.com/hibernate-multiplebagfetchexception/) with an adoption for Spring Data JPA

## First: How NOT to “fix”: Using Set instead of List

See the original article

## Recommended fix: Instead of executing a single JPQL query that fetches two associations, we can execute two JPQL queries instead

```java

List<Post> posts = doInJPA(entityManager -> {
    List<Post> _posts = entityManager
    .createQuery(
        "select distinct p " +
        "from Post p " +
        "left join fetch p.comments " +
        "where p.id between :minId and :maxId ", Post.class)
    .setParameter("minId", 1L)
    .setParameter("maxId", 50L)
    .setHint(QueryHints.PASS_DISTINCT_THROUGH, false)
    .getResultList();
 
    _posts = entityManager
    .createQuery(
        "select distinct p " +
        "from Post p " +
        "left join fetch p.tags t " +
        "where p in :posts ", Post.class)
    .setParameter("posts", _posts)
    .setHint(QueryHints.PASS_DISTINCT_THROUGH, false)
    .getResultList();
 
    return _posts;
});
 
```

The first JPQL query defines the main filtering criteria and fetches the Post entities along with the associated PostComment records.

Now, we have to fetch the Post entities along with their associated Tag entities, and, thanks to the Persistence Context, Hibernate will set the tags collection of the previously fetched Post entities.

## Adopting for Spring JPA

We'll need 2 JPQL calls **within a same transaction**. Assuming we have a service method coordinating these calls, it's important to annotate that method with the `@Transactional` annotation.

```
    @Transactional
    public List<Entity> getEntityList(){
      /*
            "select distinct p " +
            "from Post p " +
            "left join fetch p.comments " +
            "where p.id between :minId and :maxId "
      */
      List<Entity> entities = entityRepo.findPostsFetchComments();
      
      /*
            "select distinct p " +
            "from Post p " +
            "left join fetch p.tags t " +
            "where p in :posts "
      */
      entities = entityRepo.fetchTags(entities);
      
      return entities;
    }
```   

**Note**: Don't forget the `@QueryHints(value = { @QueryHint(name = HINT_PASS_DISTINCT_THROUGH, value = "false")})` on the repository methods (e.g. `findPostsFetchComments`) to not send the distinct into the SQL. [For parent-child entity queries where the child collection is using JOIN FETCH, the DISTINCT keyword should only be applied after the ResultSet is got from JDBC, therefore avoiding passing DISTINCT to the SQL statement that gets executed](https://vladmihalcea.com/jpql-distinct-jpa-hibernate/)
