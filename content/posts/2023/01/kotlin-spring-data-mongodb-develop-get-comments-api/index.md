---
categories:
- Software Development
date: 2023-01-17 09:19:42+01:00
draft: false
image: 0_-YMPGEafKe0F0T-m.jpg
medium: https://levelup.gitconnected.com/kotlin-spring-data-and-mongodb-develop-a-get-comments-api-endpoint-666b533e7a04
subtitle: With 4 Flavors of Data Retrieval
tags:
- Kotlin
- Spring
- API
- Spring Boot
- MongoDB Spring Data
title: 'Kotlin, Spring Data, and MongoDB: Develop a “GET /comments” API endpoint'
---
{{< figure src="0_-YMPGEafKe0F0T-m.webp" caption="Photo by [Pratiksha Mohanty](https://unsplash.com/@pratiksha_mohanty?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}


**_TL;DR_**

_The 4 flavors are:_

1.  **_Retrieve ALL_** _— more details at step #4_
2.  **_Filtering by exact matching_** _— more details at step #5_
3.  **_Filtering by partial/substring matching_** _— more details at step #6_
4.  **_Filtering by full-text search_** _— more details at step #7_

_Sample codes:_ [https://github.com/geraldnguyen/kotlin-springdata-mongodb](https://github.com/geraldnguyen/kotlin-springdata-mongodb)

# #1 — Create a Kotlin + Spring Data + MongoDB project

Let us start by creating a simple project with [Spring Initializr](https://start.spring.io/):

*   Language: Kotlin
*   Dependencies: Spring Data MongoDB, Spring Web

{{< figure src="1_lx_H1SJkZY1imKsnqVcqyQ.png" caption="Spring Initializr configuration (screenshot)" >}}

Once you download the generated project and set it up on your local machine, let’s start it up to verify that it is working. You may notice some errors in the server’s console, but that is okay since we have not setup any MongoDB connection details

{{< figure src="1_px2wAdbzy6kqmU50QKwkxA.png" caption="MongoDB connection errors (screenshot)" >}}


# #2 — Create a free MongoDB instance

Head over to [https://www.mongodb.com/cloud/atlas/register](https://www.mongodb.com/cloud/atlas/register) to register a free instance of MongoDB. We’ll use a shared cluster because it fits our learning purpose and is free.

{{< figure src="1_RPWvk3XipZsTlzWpLE5wUQ.png" caption="Create a free MongoDB instance" >}}

Next, we create a DB user

{{< figure src="1_RQ8jX8zIBDWUFw0X659xTQ.png" caption="Setup DB user" >}}

And then set up the network access security. You can add `0.0.0.0/0` to the IP address field to allow access from anywhere.

{{< figure src="1_28PrsaR_8bUdUCCh3milTQ.png" caption="Set up network access" >}}


## Load the sample data

MongoDB Atlas comes with several sample databases. Let’s load them into our cluster.

{{< figure src="1_fxN7eiM0cJwWxIsAGdqJCw.png" caption="Load sample databases" >}}

While it takes some time to finish loading all databases, you can already click on the Browse Collection button to explore the sample data.

{{< figure src="1__8J13squpHi01kiSxN485w.png" caption="Partially loaded sample data" >}}


# #3 — Configure MongoDB Connection String in Springboot

Find the Connect button within MongoDB’s Database screen

{{< figure src="1_Fjw8NKNwEdn9ACtz35LcWA.png" caption="The Connect button" >}}

Then select **Connect your application**

{{< figure src="1_qtzUA7XrWHgA0-9dPuc1ug.png" caption="Different connection options" >}}

Then select the correct driver (Java, latest version) and copy the connection string.

{{< figure src="1_yppqu-HIFXEtTfnF39anuw.png" caption="Copy the connection string" >}}

Back to your IDE, edit the application configuration to add a new environment variable `MONGODB_URI` with the value from the copied connection string (with the DB user’s password correctly updated)

{{< figure src="1_LM-2d8lB8CDJ_iRNI5379g.png" caption="Adding a new environment variable in IntelliJ" >}}

Let’s enter the following into the `application.properties` file and restart the application. The MongoDB connection errors should go away.

```
spring.data.mongodb.uri=${MONGODB_URI}  
spring.data.mongodb.database=sample_mflix
```

Take note that we specified the sample database “sample_mflix” in the `application.properties` file. Our application will connect to this database by default.

# #4 — Flavor #1: Retrieve all

Let’s create 3 new Kotlin classes in our application:

*   `<your-project>.api.CommentController`: The REST controller for the `/comments` endpoint. We use this API to selectively expose our database to outsiders. This is also our main medium for testing.
*   `<your-project>.api.data.Comment`: The data model for Comment collection
*   `<your-project>.api.data.CommentRepository`: The data _repository_ for Comment collection. A repository is an interface where we declare various methods for retrieving data from or updating data in the database. Spring Data by default provides ready-to-use methods such as `findAll` or `findById`. We can also declare our own. The actual low-level protocol-specific implementation is intelligently provided by the Spring Data library.

{{< figure src="1_83tWMixdJj_0vl_wEjk7EQ.png" >}}

## The Comment data model

The `Comment` class follows closely the “comments” collections in the sample “sample_mflix” database.

Provided that each field’s name is the same as the collection’s property, we only need to annotate the class with `@Document("<collection-name>")` at the class level, and `@Id` at field `id` level to make it work.

The `@Field("movie_id")` is necessary here because we do not want to name the field `movie_id` as that does not follow Kotlin’s naming convention.

```java
@Document("comments")  
class Comment {  
    @Id  
    lateinit var id: String  
  
    lateinit var name: String  
  
    lateinit var email: String  
  
    @Field("movie_id")  
    lateinit var movieId: String  
  
    lateinit var text: String  
  
    lateinit var date: LocalDateTime  
}
```

{{< figure src="1_UEvBVotELo-yKjxyYzmbXw.png" caption="A record from the “comments” collection" >}}

## The CommentRepository interface

If you find the `Comment` class simple, you will be delighted to see the content of `CommentRepository.kt` file:

interface CommentRepository : MongoRepository<Comment, String\>

Yes, it is a one-liner.

The `CommentRepository` interface extends from Spring Data’s `MongoRepository` interface. The extra `<Comment, Spring>` to the right of `MongoRepository` is for Spring Data to know which data model to generate an implementation for and which primary key to use in that model.

As I mentioned earlier, Spring Data provided some ready-use methods such as `findAll` or `findById` which we can use straight away.

## The CommentController

We will keep the controller simple, yet it is still enough to demonstrate the implementation of the first flavor.

```java
@RestController  
@RequestMapping("/comments")  
class CommentController {  
    @Autowired  
    private lateinit var commentRepository: CommentRepository  
  
    @GetMapping(value = [ "", "/" ])  
    fun listAll(): List<Comment> {  
        return commentRepository.findAll()  
    }  
}
```

The key thing to note in this controller is `commentRepository.findAll()`. This statement calls the method `findAll()` on the `commentRepository` to retrieve all`Comment` records from the database. Because we don’t need any extra processing, we simply `return` the result to the caller.

Let’s restart our application and enter `http://localhost:8080/comments` to a browser.

{{< figure src="1_2s8OKrzk35Pcj02NgHg1mg.png" caption=`http://localhost:8080/comments` >}}

> You can check out the branch [`flavor_1`](https://github.com/geraldnguyen/kotlin-springdata-mongodb/tree/flavor_1) to try out the codes

## Improve `CommentController`’s performance

If you have run the above flavor #1 codes, you will notice lots of text from a large JSON document rendered on the browser. It took a while (10 seconds in my machine) to finish loading as well.

The reason for this slowness is that the application really retrieved all 41079 records from the database. That is just too many records! The application’s performance suffered as a result. The free Mongo Atlas probably was not happy either if we kept doing so.

Let’s improve it with a technique called pagination. Refer to the new controller code below.

```java
@RestController  
@RequestMapping("/comments")  
class CommentController {  
    @Autowired  
    private lateinit var commentRepository: CommentRepository  
  
    @GetMapping(value = [ "", "/" ])  
    fun listAll(  
        @RequestParam(value = "_size", required = false, defaultValue = "10") size: Int,  
        @RequestParam(value = "_page", required = false, defaultValue = "0") page: Int  
    ): List<Comment> {  
        val pageable = Pageable.ofSize(size).withPage(page);  
  
        val result: Page<Comment> = commentRepository.findAll(pageable)  
  
        return result.content  
    }  
}
```

You would have noticed that we added 2 new `@RequestParam` namely `_size` and `_page`. They capture the values provided in the URL query param of the same names. They are not mandatory and have default values so that we can continue opening `http://localhost:8080/comments` to load the first 10 records.

We use these parameters in the statement `Pageable.ofSize(size).withPage(page)` to create an object of type `Pageable` which we then pass into the method `findAll` of the repository. Spring Data will use the information there to split the data into multiple pages (starting from index 0) of the specified `_size`, then retrieve the data from the specified `_page`. The return from this `findAll(Pageable)` statement is a `Page<Comment>` object, hence we need to access`result.content` to get the final list which we simply return to the caller.

Let’s restart the application and try to open `http://localhost:8080/comments`.

{{< figure src="1_1TTthxVKY8kymXnT6wfsLg.png" caption=`http://localhost:8080/comments` >}}

Much improve! Don’t you think so?

Now let’s try `http://localhost:8080/comments?_size=100` or `http://localhost:8080/comments?_size=2&_page=1` if you notice any difference.

{{< figure src="1_5HoWJBqZUlkRKwBOjTmTbg.png" caption=`http://localhost:8080/comments?_size=2&_page=1` >}}

Yes, the number of records and the positions of those records in the database collection changes according to the `_size` and `_page` parameters.

> You can check out the branch [`flavor_1_with_pagination`](https://github.com/geraldnguyen/kotlin-springdata-mongodb/tree/flavor_1_with_pagination) to examine the codes
> 
> I used [JSON Formatter chrome extension](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa) to nicely format the response. Your browser may look different.

# #5 — Flavor #2: Filtering by exact matching

Once we are able to retrieve a list of comments, sometimes we like to view a single comment in detail. Since each comment _uniquely_ identified by its `id` attribute, we can create an additional API endpoint `/comments/<id>` to retrieve _exactly_ the comment we are interested in.

Let’s add the following implementation for that endpoint to the `CommentController`. What’s new in these codes is the use of the method `findById(id)` from the `commentRepository` instance to retrieve the single record exactly matching the provided `id` value. The method returns an `Optional<Comment>` instance (because the `id` value may not match any record), hence we need to call its `get()` method to retrieve the actual `Comment` record.

```java
    @GetMapping(value = ["/{id}"] )  
    fun findById(@PathVariable("id") id: String): Comment {  
        return commentRepository.findById(id).get();  
    }
```

Let’s restart the application and enter `http://localhost:8080/comments/5a9427648b0beebeb6957a4b` in the browser’s address bar

{{< figure src="1_nHgaj-zSULZ72M1aA8xY2g.png" caption=`http://localhost:8080/comments/5a9427648b0beebeb6957a4b` >}}

> You can check out git branch `flavor_2_by_id` to examine the codes
> 
> In practice, we should validate the user-provided `_id_` value and reject all malicious input. We should also check if the result from `_findById()_` actually contains any value before calling its `_get()_` method. For brevity, however, such codes are omited in this tutorial.

## Search comments by email

Let’s up our application a little bit by enabling searching by email.

To enable this feature, we first need to enable this capability in the `CommentRepository` by adding method `findByEmailIgnoreCase(email: String, pageable: Pageable)`. Notice how the method’s name is constructed from `findBy` + `Email` + `IgnoreCase` and how we need not provide any implementation. This is the magic of Spring Data. It knows from the name that we want to _find_ all records matching the _email_ attribute (which is passed in as the first parameter) and _ignore case_ (e.g. _abc@GMAIL.com_ is the same as _abc@gmail.com_). We still like to paginate the result, hence we keep the `Pageable` parameter.

interface CommentRepository : MongoRepository<Comment, String\>{  
    fun findByEmailIgnoreCase(email: String, pageable: Pageable): Page<Comment>  
}

On the `CommentController`, we need to make a small modification to call the new `CommentRepository`’s method. Take note of the new parameter `email`. If it contains some value (i.e. it is not `isNullOrBlank()`), then we will call `commentRepository.findByEmailIgnoreCase(email, pageable)` to search all comments matching the specified `email` value regardless of its case.

```java
@GetMapping(value = [ "", "/" ])  
fun listAll(  
    @RequestParam(value = "email", required = false) email: String?,  
    @RequestParam(value = "_size", required = false, defaultValue = "10") size: Int,  
    @RequestParam(value = "_page", required = false, defaultValue = "0") page: Int  
): List<Comment> {  
    lateinit var result: Page<Comment>  
    val pageable = Pageable.ofSize(size).withPage(page);  
  
    when {  
        !email.isNullOrBlank() ->  
            result = commentRepository.findByEmailIgnoreCase(email, pageable)  
        else ->  
            result = commentRepository.findAll(pageable)  
    }  
  
    return result.content  
}
```

Let’s give it a test by restarting the application and visit `http://localhost:8080/comments?email=john_bishop@fakegmail.com` or any other emails that catch your interest.

{{< figure src="1_9PtKyPv9W34Q1H0VydZJUQ.png" caption=`http://localhost:8080/comments?email=john_bishop@fakegmail.com` >}}


> With only 1 additional parameter from the previous version, we could have use `_if...else_` instead of `_when_`. However, as you will see in next section, using `_when_` allows us to support other flavors with cleaner codes.
> 
> You can check out git branch [`flavor_2_by_id`](https://github.com/geraldnguyen/kotlin-springdata-mongodb/tree/flavor_2_by_id) to examine the codes

# #6 — Flavor #3: Filtering by partial/substring matching

With the search-by-email feature available, our users were very happy. For a while. Then they demanded more. This time they wanted the ability to search for comments by specifying the author’s name. That’s easy, you think — it’s flavor #2, you already knew how to do it. Nope, our users do not want to type in the full name, just part of it like first name or last name.

That’s flavor #3. We will get it done in this section.

The secret ingredient for flavor #3 is in the `CommentRepository`. Similar to search-by-email, we first need to create a suitable method. The new method `findByNameContainingIgnoreCase(name: String, pageable: Pageable)` is very similar to its older cousin except for the `Containing` keyword following the matching attribute’s name. `Containing` is a Spring Data keyword that tells Spring to perform partial or substring matching instead of exact matching. `Containing` will return a record with the name “John Doe” when searching with “John” or “Doe”.

```java
interface CommentRepository : MongoRepository<Comment, String\>{  
    fun findByEmailIgnoreCase(email: String, pageable: Pageable): Page<Comment>  
    fun findByNameContainingIgnoreCase(name: String, pageable: Pageable): Page<Comment>  
}
```

The remaining task in the `CommentController` is easy. Notice how we add a new parameter `name` and a new matching condition in the `when` statement.

```java
@GetMapping(value = [ "", "/" ])  
fun listAll(  
    @RequestParam(value = "email", required = false) email: String?,  
    @RequestParam(value = "name", required = false) name: String?,  
    @RequestParam(value = "_size", required = false, defaultValue = "10") size: Int,  
    @RequestParam(value = "_page", required = false, defaultValue = "0") page: Int  
): List<Comment> {  
    lateinit var result: Page<Comment>  
    val pageable = Pageable.ofSize(size).withPage(page);  
  
    when {  
        !email.isNullOrBlank() ->  
            result = commentRepository.findByEmailIgnoreCase(email, pageable)  
        !name.isNullOrBlank() ->  
            result = commentRepository.findByNameContainingIgnoreCase(name, pageable)  
        else ->  
            result = commentRepository.findAll(pageable)  
    }  
  
    return result.content  
}
```

Again, let’s test it by restarting the application, then opening `http://localhost:8080/comments?name=roman` in a browser.

{{< figure src="1_IcCeA_9k-2VJ7SAealzGhA.png" caption=`http://localhost:8080/comments?name=roman` >}}


> You can check out git branch [`flavor_3`](https://github.com/geraldnguyen/kotlin-springdata-mongodb/tree/flavor_3) to examine the codes

# #7 — Flavor #4 — Full-text search

Our users want more. They want a generic search for the name, email, and content of the comment itself, and they want the search to be _more intelligent._ For example, they wanted “_ran_” and “_run_” to return the same results, so do “_go_”, “_goes_” and “_went_”, so do “_dress_” and “_dresses_”, and so on. These capabilities are beyond what flavors #1, #2 and #3 can offer.

To the rescue is flavor #4 — Full-text search. This feature just requires an extra configuration on the DB side but otherwise is straightforward.

## Enabling Indexing in the MongoDB database

Let’s open the “**comments**” collection in the “**sample_mflix”** database in your MongoDB cluster. Then open the Indexes tab and click on the Create Index button. That should bring up a modal like the one below.

{{< figure src="1_BE092PEjTrUKlBENr2yZ6Q.png" >}}

Put the same content below into the Fields text area, click Review and then Confirm to create the index.

```
{  
  "name": "text",  
  "email": "text",  
  "text": "text"  
}
```

What we have just done is create a [text index](https://www.mongodb.com/docs/manual/core/index-text/) on 3 fields “name”, “email” and “text” of the “comments” collection. A text index enables full-text searching on the specified fields.

## Adding a “keyword” search option

Back to our Kotlin and Spring Data codes. We will add a new search parameter named `keyword` with a small modification to the `CommentController`

```java
@GetMapping(value = [ "", "/" ])  
fun listAll(  
    @RequestParam(value = "email", required = false) email: String?,  
    @RequestParam(value = "name", required = false) name: String?,  
    @RequestParam(value = "keyword", required = false) keyword: String?,  
    @RequestParam(value = "_size", required = false, defaultValue = "10") size: Int,  
    @RequestParam(value = "_page", required = false, defaultValue = "0") page: Int  
): List<Comment> {  
    lateinit var result: Page<Comment>  
    val pageable = Pageable.ofSize(size).withPage(page);  
  
    when {  
        !email.isNullOrBlank() ->  
            result = commentRepository.findByEmailIgnoreCase(email, pageable)  
        !name.isNullOrBlank() ->  
            result = commentRepository.findByNameContainingIgnoreCase(name, pageable)  
        !keyword.isNullOrBlank() ->  
            result = commentRepository.findAllBy(TextCriteria.forDefaultLanguage().matching(keyword), pageable)  
        else ->  
            result = commentRepository.findAll(pageable)  
    }  
  
    return result.content  
}
```

Notice this statement `commentRepository.findAllBy(TextCriteria.forDefaultLanguage().matching(keyword), pageable)`. What it does is create a full-text search criterion matching the specified `keyword`, then we send that criterion to the `CommentRepository` to forward it to MongoDB server.

Of course, we need to add a `findAllBy(TextCriteria, Pageable)` method to the repository

```java
interface CommentRepository : MongoRepository<Comment, String\>{  
    fun findByEmailIgnoreCase(email: String, pageable: Pageable): Page<Comment>  
    fun findByNameContainingIgnoreCase(name: String, pageable: Pageable): Page<Comment>  
    fun findAllBy(textCriteria: TextCriteria, pageable: Pageable): Page<Comment>  
}
```

That is all needed to perform a full-text search.

Let’s restart the application and try different keywords such as:

*   “gameofthron” for _email_
*   “roberts” for _name_ — notice how the search result contains matching for both “Robert” and “Roberts”
*   and “officiis” for _text_

{{< figure src="1_NzUM9ns78a7FrtDBEiqa9w.png" caption=`http://localhost:8080/comments?keyword=gameofthron` >}}


{{< figure src="1_nyGUBEO68JyQcIIflpB8TQ.png" caption=`http://localhost:8080/comments?keyword=Roberts` >}}


{{< figure src="1_XFR-L2yCmAREZ77puGO0UQ.png" caption=`http://localhost:8080/comments?keyword=officiis` >}}


> You can check out the branch [`flavor_4`](https://github.com/geraldnguyen/kotlin-springdata-mongodb/tree/flavor_4) to examine the codes

# Conclusion

We have covered quite many steps. Importantly, we learned how to:

*   Set up a Kotlin + Spring Data + MongoDB application
*   Create a free MongoDB cluster and load some sample databases.
*   Create a text index on a MongoDB collection to enable full-text search
*   Create REST API and MongoDB repository methods to search the MongoDB
*   Implement 4 flavors of data retrieval, from retrieving all, to exact matching, to partial search, and finally to full-text search.