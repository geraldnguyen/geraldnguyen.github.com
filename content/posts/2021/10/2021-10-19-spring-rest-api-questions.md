---
title: "Spring REST API Interview Questions"
date: 2021-10-19T09:19:42+01:00
draft: false
weight: 50
tags: [java, spring, api, interview, annotation, testing]
---

## Fill in the blank

**Objective**: Assess candidate's practical experience and knowledge

**Instructions**: Given the below controller, fill in Spring annotations according to instruction given in javadocs

**Worth paying attention**:
- Did the candidate notice the repetion of `/friends` prefix and opt to use a `@RequestMapping(/friends)` on the controller class
- How did he/she annotate the *optional* parameter `name` and `id` in `findFriends` method?
- What annotation can be used to force a 204 (no content) response?


```java
/* Assume all package, imports and dependency classes are available */

/**
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
     * Response to DELETE /friends/{friend's id} request, return 204 if successful
     * @param friendId the id of the friend
     */
    public void deleteFriend(long friendId) {
        friendService.removeFriend(friendId);
    }

    /**
     * Update friend name
     * Response to <METHOD?> /friends/{friend's id}/{new name} request, return update friend info
     * @param friendId the id of the friend
     * @param newName the new name of the friend
     * @return updated friend's info
     */
    public FriendDTO updateFriend(long friendId, String newName) {
        return friendService.updateName(friendId, newName);
    }
}

```

## How do you write test for above controller?

**Objective**: Assess candidate's practical experience and knowledge

**Instructions**: As writing test can be time-consuming, it is best to go through the solution verbally

**Worth paying attention**:
- Does the candidate know of and mention popular test libraries such as JUNIT, Mockito, Hamcrest, Spring Boot Test?
- Which approach did the candidate take:
  + Directly call controller method (e.g. `new FriendController(mockFriendService).hello("world)`
  + `RunWith(SpringRunner.clss)` and `webmvc.perform(...).andExpect(...)`
  + `given`...`when`...`then` (e.g. BDD with Cucumber)
- What does he/she know the difference between "strict mocking" and "loose mocking"?

## How do you return an error?

**Objective**: Assess candidate's knowledge of HTTP status code, awareness of information disclosure and exception handling

**Questions**

- How do you response to a search for a non-existent resources?
  + Not everything is an error. An empty list `[]` is a fine response to a *search* query

- How do you response to a `GET /friends/123` when there is no record with id 123?
  + It should be a 404 response
  + It could be a custom exeption annotated with `@RequestStatus(status = HttpStatus.NOT_FOUND`
  + Or there is an `@ExceptionHandler` method on the controller or an `@ControllerAdvice` global handler

- What do you response to a `GET /friends/?id=some-name`?
  + It should be 400 - Bad Request
  + It is usually handled by Spring and is never reach controller's code

  