---
categories:
- Software Development
- Career Development
date: 2023-01-12 09:19:42+01:00
draft: false
image: 0_5kwKvgKkYWpxPDoz.jpg
medium: https://medium.com/50ld/five-minimum-viable-spring-rest-interview-questions-c92414bdbd49
subtitle: 'Interviewer: ask them if your interview is running out of time. Interviewee:
  practice them if your preparation is running out of time.'
tags:
- Java
- Spring
- API
- Interview
- Practical
title: Five Minimum Viable Spring REST Interview Questions
---
{{< figure src="0_5kwKvgKkYWpxPDoz.webp" caption="Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

{{< pagebreak >}}

{{< include "posts/2023/02/Five Minimum Viable Interview Questions series/series.include" >}}

{{< pagebreak >}}


# #1 — What is REST API?

{{< quote >}}

This question assesses the breadth and depth of a candidate’s understanding of REST API concepts.
 
Though this seems basic, it has caught many candidates off-guard because they never prepare for it. Good candidate will recover but inexperienced or shallow candidate may not.

{{< /quote >}}

API stands for Application Programming Interface. It refers to how software components should _publicly_ appear to each other. When one component needs another to do some specific work, it calls a specific service defined in the other component’s API. The term API nowadays also applies to software separated over a network or even the internet.

A REST API (also known as RESTful API) is an application programming interface (API) between two systems over a network (LAN or the Internet) built in the REST architectural style. REST APIs are commonly used to connect individual services in a complex system such as microservices (e.g. sales service to fulfillment service), or mobile or web applications to their backend services. We use REST APIs to request or provide data or services from one specialized system to another.

REST stands for **RE**presentational **S**tate **T**ransfer. A REST API transfers the representation of the state of information over the network on HTTP protocol from one system to another. The one that initiates the API request is called a client, and the one that serves or responds to the request is called a server.

The representation can be in one of a variety of formats like JSON, XML, plain text, or even binary. JSON (Javascript Object Notation) is the most popular because it is simple, readable by humans, and has broad and mature programming support.

REST API uses HTTP’s method, URL, parameters, headers, body, and response codes to transfer the client’s request to the server and the server’s response back to the client. We commonly use the GET method for retrieving resources unmodified, the POST method for creating a new resource, the PUT or PATCH method for updating, and the DELETE for deleting resources.

Further readings:

*   [https://medium.com/illumination/what-are-apis-2259bb43ddb4](https://medium.com/illumination/what-are-apis-2259bb43ddb4)
*   [https://www.redhat.com/en/topics/api/what-is-a-rest-api](https://www.redhat.com/en/topics/api/what-is-a-rest-api)
*   [https://aws.amazon.com/what-is/restful-api/](https://aws.amazon.com/what-is/restful-api/)
*   [https://www.techtarget.com/searchapparchitecture/definition/RESTful-API](https://www.techtarget.com/searchapparchitecture/definition/RESTful-API)

{{< pagebreak >}}

# #2 — How do you create a REST API for a FriendList application that accepts a name parameter, performs a search, and returns a list of all matching friends?

{{< quote >}}

Base on a real common requirements in API development, this simplified question directly assesses a candidate’s practical knowledge and experience in Spring REST API development.

{{< /quote >}}

_Note: this is not intended to be a live coding test, hence the sample answer below is verbal and intentionally omits certain technical details. A sample code is still provided for your reference._

Spring provides the `@RestController` annotation to designate a class as a REST controller, which is a special type of web controller that handles REST API requests. Let’s call this controller `FriendController`

Within the controller, let’s create a method called `searchFriend` which accepts a `name` parameter, searches the database, and returns a list of all matching friends. We will apply the`@GetMapping` annotation to this method to make it the entry point for the search request. We’ll also apply the `@RequestParam` annotation to the `name` parameter to tell Spring to put the search value into this parameter.

The search for matching friends is performed by another class. Assuming we have a `FriendService` instance within the controller, we will call a method, e.g. `findByName`, of this service with the `name` search parameter to obtain the list of all matching friends. The controller’s `searchFriend` method will then return the found list. The Spring framework takes care of converting that list into a proper JSON array.

_Sample codes:_

```java
@RestController  
@RequestMapping("/friends")  
public class FriendController {  
    private final FriendService friendService;  
  
    public FriendController(FriendService friendService) {  
        this.friendService = friendService;  
    }  
  
    /**  
     * Response to GET /friends/ request  
     * @param name (optional) filter by name  
     * @return a list of friends  
     */  
    @GetMapping("/")  
    public List<FriendDTO> searchFriend(@Nullable @RequestParam String name) {  
        if (StringUtils.hasText(name)) {  
            return friendService.findByName(name);  
        } else {  
            return friendService.findAll();  
        }  
    }  
}
```

{{< pagebreak >}}

# #3 — How do you test the search-friend REST API (see question #2 above)?

{{< quote >}}
Testing is a necessary skill and an important attitude. A good software engineer tests all the codes that he or she wrote.
 
This question evaluates the knowledge, experience, and how testing is embedded in a candidate’s development practice

{{< /quote >}}

_Approaches to API testing, from least to most sophisticated, multiple choices allowed:_

1.  Open a browser and type in the URL such as `[https://localhost:8080/friends/?name=john](https://localhost:8080/friends/?name=john)` and check the JSON response
2.  Open a terminal and type in the command `curl [https://localhost:8080/friends/?name=john](https://localhost:8080/friends/?name=john)` and check the JSON response
3.  Open [Postman](https://www.postman.com/) and create a request to `[https://localhost:8080/friends/?name=john](https://localhost:8080/friends/?name=john)` and check the JSON response
4.  Open an IDE and create a unit test using frameworks like JUnit and Mockito
5.  Open an IDE and create an integration test using `SpringRunner` and `MockMVC`
6.  Open an IDE and create a BDD test using frameworks and tools like Cucumber and Selenium

_Further details are necessary for options (4), (5), or (6). See the example below for option (5)_

## Integration test with Spring

Spring supplies utilities to create integration tests for REST API. We can create a test class called `FriendControllerTest` and annotate it with `@SpringRunner` and `@WebMvcTest` annotations to set it up for Spring REST API integration test. We need to _autowire_ an instance of `MockMvc` which is a utility by Spring to facilitate integration testing of REST requests.

We also need a mocked instance of `FriendService` so that we can focus on the testing of the controller only. The annotation `@MockBean` applied on an instance variable `friendService` provides us with exactly that. We will then set up the `friendService.findByName` method to return a sample list of friends whenever a search parameter value “john” is provided.

Then we will call a `get` method with a “john” `name` parameter on a `MockMvc` instance to invoke the API. We will then inspect the response obtained and assert the received JSON matching the mocked value setup with `friendService`

```java
@RunWith(SpringRunner.class)  
@WebMvcTest(FriendController.class)  
class FriendControllerTest {  
    @Autowired  
    private MockMvc mockMvc;  
  
    @MockBean  
    private FriendService friendService;  
  
    @Test  
    void findByName() throws Exception {  
        when(friendService.findByName("john")).thenReturn(List.of(  
 FriendDTO.builder().id(1L).name("john").build()  
        ));  
  
        mockMvc.perform(get("/friends/?name=john"))  
            .andDo(print())  
            .andExpect(status().isOk())  
            .andExpect(content().json("[ {'id': 1, 'name': 'john'}]"))  
        ;  
    }  
}
```

{{< pagebreak >}}

# #4 — How do you handle a search request that matches 500 records?

{{< quote >}}
This question evaluates if the candidate pays due attention to API’s performance, usability and is familiar with the pagination pattern.
{{< /quote >}}

While it is possible to return all 500 records, doing so may impact the API’s performance and may overload the network and client with too large a response.

There are 2 common solutions to address this scenario:

*   Limit the number of results returned with a warning that the response has been truncated due to the search results in too many records.
*   Implement pagination to allow the API to break the results into smaller chunks which can be accessed sequentially or randomly by the client.

The API can optionally support more filters for the client to refine the search and find the specific results more quickly

{{< pagebreak >}}

# #5 — When do you return an HTTP 202 (Accepted) response and how do you return it?

{{< quote >}}
This question tests a candidate’s knowledge of different HTTP status codes and their uses. It also assesses how hands-on the candidate is in using Spring ResponseEntity class and/or ReponseStat`us` annotation set a response’s status code
{{< /quote >}}

HTTP 202 (Accepted) status code indicates that the request has been received and is being processed asynchronously. A popular scenario to use 202 is when creating a new job execution request.

Instead of keeping the client waiting and hogging the server’s resources, the API can return this status, optionally together with a transaction id. The client can check the status again, using the returned transaction id as the lookup key, to determine if the processing has been completed.

We can use the `@ResponseStatus` annotation to annotate the endpoint method with the HTTP 202 status.

We can also have the endpoint method declare a `ResponseEntity` return type. We can call either `accepted()` method or `status(HttpStatus.CREATED)` method on the `ResponseEntity` instance to set the HTTP 202 status.