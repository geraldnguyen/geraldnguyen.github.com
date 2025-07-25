---
categories:
- Software Development
date: 2020-09-10 09:19:42+01:00
draft: true
subtitle: Excerpt from See https://vladmihalcea.com/hibernate-multiplebagfetchexception/
  with an adoption for Spring Data JPA
tags:
- Java
- Programming
- Hibernate
- JPA
title: The best way to fix the Hibernate MultipleBagFetchException
---
**Note 2024-08-15: An updated version of this article was published in Medium at https://geraldnguyen.medium.com/the-best-way-to-fix-the-hibernates-multiplebagfetchexception-6c63e69a4009**

----------------

## First: How NOT to “fix”: Using Set instead of List

See the [original article](https://vladmihalcea.com/hibernate-multiplebagfetchexception/)

## Recommended fix: Instead of executing a single JPQL query that fetches two associations, we can execute two JPQL queries instead

```
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

We’ll need 2 JPQL calls **within a same transaction**. Assuming we have a service method coordinating these calls, it’s important to annotate that method with the `@Transactional` annotation.

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

**Note**: Don’t forget the `@QueryHints(value = { @QueryHint(name = HINT_PASS_DISTINCT_THROUGH, value = "false")})` on the repository methods (e.g. `findPostsFetchComments`) to not send the distinct into the SQL. [For parent-child entity queries where the child collection is using JOIN FETCH, the DISTINCT keyword should only be applied after the ResultSet is got from JDBC, therefore avoiding passing DISTINCT to the SQL statement that gets executed](https://vladmihalcea.com/jpql-distinct-jpa-hibernate/)