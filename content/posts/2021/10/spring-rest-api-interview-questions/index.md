---
categories:
- Software Development
- Career Development
date: 2021-10-19 09:19:42+01:00
draft: false
image: 1_lOcYvYf5KARsBswgtPcoEQ.png
medium: https://medium.com/50ld/spring-rest-api-interview-questions-66cf78fa1f2c
subtitle: Use these to assess multiple practical aspects of a candidate’s skills and
  experience
tags:
- Java
- Spring
- API
- Interview
- annotation
- Testing
title: Spring REST API Interview Questions
---
#1 Complete the REST controller
-------------------------------

**Objective**: Assess a candidate’s practical experience and knowledge

**Instructions**: Given the below controller, fill in Spring annotations according to instructions given in the code comments

**Pay attention to**:

*   Did the candidate notice the repetition of `/friends` prefix? Did he/she suggest a `@RequestMapping(/friends)` on the controller class instead?
*   How did he/she annotate the _optional_ parameter `name` and `id` in `findFriends(String name, Long id)` method?
*   What annotation can be used to force a 204 (no content) response?

```java
/* Assume all package, imports and dependency classes are available */

/**
 * A rest controller for Friend-related operations
 */
public class FriendController {
    private final FriendService friendService;

    public FriendController(FriendService friendService) {
        this.friendService = friendService;
    }

    /**
     * Response to GET /friends/ request
     * @param name (optional) filter by name
     * @param id (optional) filter by id
     * @return a list of friends
     */
    public List<FriendDTO> findFriends(String name, Long id) {
        if (id != null) {
            return friendService.findByFriendId(id);
        } else if (StringUtils.hasText(name)) {
            return friendService.findByName(name);
        } else {
            return friendService.findAll();
        }
    }

    /**
     * Response to GET /friends/hello/{friend's name} request
     * @param friend a friend's name
     * @return a friendly Hello
     */
    public String hello(String friend) {
        return String.format("Hello %s", friend);
    }

    /**
     * Response to POST /friends/hello/{friend's name} request
     * @param friend a friend's name which is to be saved
     * @return a friendly Hello together with Id of the new friend
     */
    public String helloAndSave(String friend) {
        FriendDTO dto = friendService.add(FriendDTO.builder().name(friend).createDt(LocalDateTime.now()).build());
        return String.format("%s. Your Id is %d", hello(friend), dto.getId());
    }

    /**
     * Un-friend
     * Response to DELETE /friends/{friend's id} request, 
     * return 204 if successful
     * @param friendId the id of the friend
     */
    public void deleteFriend(long friendId) {
        friendService.removeFriend(friendId);
    }

    /**
     * Update friend name
     * Response to <METHOD?> /friends/{friend's id}/{new name} request, 
     * return update friend info
     * @param friendId the id of the friend
     * @param newName the new name of the friend
     * @return updated friend's info
     */
    public FriendDTO updateFriend(long friendId, String newName) {
        return friendService.updateName(friendId, newName);
    }
}
```

{{< figure src="1_lOcYvYf5KARsBswgtPcoEQ.png" caption="A friendly REST controller" >}}


#2 Write tests for controller
=============================

**Objective**: Assess a candidate’s practical experience and knowledge

**Instructions**: As devising test scenarios and writing them can be time-consuming, it is perhaps best to go through the solution verbally

**Pay attention to**:

*   Did the candidate use (or at least mention) any popular test libraries such as JUNIT, Mockito, Hamcrest, Spring Boot Test?
*   Which approach did the candidate take:  
    a) Directly call controller method: `controller.hello("world)` . Did the candidate creates the controller instance with `var controller = new FriendController(mockFriendService);` or use the Mockito’s `@InjectMock`annotation to create the controller instance with dependencies auto-injected?  
    b) Use a test runner (e.g. `SpringRunner`) and write integration-level test codes (e.g. `webmvc.perform(...).andExpect(...)` )
*   How did the candidate organize the testing codes? Unorganized or `Arrange...Act...Assert` or `Given...When...Then`

#3 Handle error
===============

**Objective**: Assess a candidate’s knowledge of HTTP status code, awareness of information disclosure, and exception handling

**Questions & Hints:**

1.  How do you respond to a search for a non-existent resource?  
    _An empty list_ `_[]_` _is a fine response to a search query_
2.  How do you respond to a `GET /friends/123` when there is no record with id 123?  
    _It should be a 404 response_
3.  How do you respond to a `GET /friends/?id=some-name`?  
    _It should be 400 — Bad Request because_ `_some-name_` _isn’t a_ `_Long_`
4.  What do you need to do to return 400 in question 3?  
    _Spring automatically detects that the supplied parameter value_ `_"some-name”_` _cannot be cast to the declared_ `_Long id_` _method parameter and returns 400. The control never reaches the controller’s code_
5.  How do you map an application exception to a particular HTTP status code?  
    _It could be a custom exception annotated with_ `_@RequestStatus(status = HttpStatus.XYZ)_`_  
    Or apply the_`_@ExceptionHandler_` _on a controller’s method to handle thrown exception  
    Or implement an_ `_@ControllerAdvice_` _global handler_
6.  What HTTP status codes indicate successful execution?  
    _HTTP status in 200 range, most commonly 200, 201, 202 and 204_
7.  When do we use these statuses?  
    _200 — OK  
    201 — Created  
    202 — Accepted  
    204 — No Content_
8.  What do you need to do to return 202 response?  
    _Either_ `_@ResponseStatus_`_Or returning a_ `_ResponseEntity.status(HttpStatus.CREATED)_`

Extra: [https://geraldnguyen.medium.com/list/api-development-interview-a17c1f9ee5b7](https://geraldnguyen.medium.com/list/api-development-interview-a17c1f9ee5b7): My articles related to API development and interview