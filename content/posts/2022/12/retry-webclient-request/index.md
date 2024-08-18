---
title: Retry WebClient Request
subtitle: Retrying a WebClient request using RetrySpec, and unit-testing it
date: 2022-12-30T09:19:42+01:00
image: 1_jGPgtzh641BG9RrIAv1A3w.jpg
draft: false
categories: [Software Development]
tags: [Java, Spring, Web Development, Unit Testing, Programming]
medium: https://geraldnguyen.medium.com/retry-webclient-request-f058fa4c337f
---

# Problem Statement

Sometimes web request fails, for whatever reason, and you need to retry it

Let’s simulate a failed request scenario in a unit test. In the below code, the `webClient` attempts to submit a `GET` request to `/employee/100` (stored in constant `PATH`). We mocked the backend server to fail with common status codes such as `BAD_REQUEST`, `UNAUTHORIZED` and validate that the client throws `WebClientResponseException` upon receiving such statuses.

```java
@ParameterizedTest  
@EnumSource(value = HttpStatus.class, mode = EnumSource.Mode.INCLUDE, names = {  
        "BAD_REQUEST", "UNAUTHORIZED", "FORBIDDEN",  
        "SERVICE_UNAVAILABLE", "INTERNAL_SERVER_ERROR"  
})  
void throwExceptionWhenReceiveNon200Response(HttpStatus status) throws InterruptedException {  
    // request  
    var webClient = WebClient.builder().baseUrl(baseUrl).build();  
    var request = webClient.get()  
            .uri(PATH)  
            .retrieve()  
            .bodyToMono(String.class);  
  
    // response to first request with fail status  
    mockBackEnd.enqueue(new MockResponse().setResponseCode(status.value()));  
  
    // assert that exception thrown due to non-200 response  
    var ex = assertThrows(WebClientResponseException.class, () -> request.block());  
    assertEquals(status, ex.getStatusCode());  
  
    // extra assert that the path is correct  
    assertEquals(PATH, mockBackEnd.takeRequest().getPath());  
}
```

# Retry unchanged

The easiest task is just retrying with the same request details.

We can use [`retryWhen`](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#retryWhen-reactor.util.retry.Retry-)([`Retry`](https://projectreactor.io/docs/core/release/api/reactor/util/retry/Retry.html) `retrySpec)` to configure a retry specification in the form of an [`RetrySpec`](https://projectreactor.io/docs/core/release/api/reactor/util/retry/RetrySpec.html) instance. `RetrySpec` supports various static builder methods that we can use to quickly set one up. In the example below, we set the maximum number of retries to 1 by calling `.max(1)`. In a similar manner, we specify a filtering condition for retry by invoking `.filter(e -> e instanceof WebClientResponseException.ServiceUnavailable))`

Let’s see it in action. First off, we need to mock 2 responses:

*   One for the original request. It should fail.
*   And one for the retry request. It should succeed to keep our test fast.

Then, as we can see from `assertDoesNotThrow(() -> requestWithRetry.block())`, our `requestWithRetry` instance does not result in any exception when invoked. And there are indeed 2 requests sent, a failed original request and a successful retried request.

```java
@Test  
void retryUnChanged() throws InterruptedException {  
    // request  
    var webClient = WebClient.builder().baseUrl(baseUrl).build();  
    var request = webClient.get()  
            .uri(PATH)  
            .retrieve()  
            .bodyToMono(String.class)  
            ;  
    var requestWithRetry = request.retryWhen(RetrySpec.max(1)        // retry at most once  
            .filter(e -> e instanceof WebClientResponseException.ServiceUnavailable));  
  
    // response to first request with a SERVICE_UNAVAILABLE  
    mockBackEnd.enqueue(new MockResponse().setResponseCode(HttpStatus.SERVICE_UNAVAILABLE.value()));  
    // response to 2nd request with 200  
    mockBackEnd.enqueue(new MockResponse().setBody("Successful!"));  
  
    // no more WebClientResponseException.ServiceUnavailable exception  
    assertDoesNotThrow(() -> requestWithRetry.block());  
  
    // assert that there are 2 requests sent  
    assertEquals(PATH, mockBackEnd.takeRequest().getPath());  
    assertEquals(PATH, mockBackEnd.takeRequest().getPath());  
}
```

# Retry with modification

It is harder to retry with modification.

Let’s consider a common scenario in OAuth-protected API invocation. After obtaining a valid OAuth token, our client can immediately make multiple API calls to the resource server. However, after some time, the token expired and our client’s API calls start to fail with HTTP 401 errors. When it first happens, we need to refresh the token and retry the request with the refreshed valid token.

Assuming we delegate the storing and refreshing of tokens to another class, it is logical to invoke the `refreshToken()` method before the retry.

Similar to retrying unchanged, we create an `requestWithRetry` instance with the help of [`retryWhen`](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#retryWhen-reactor.util.retry.Retry-)([Retry](https://projectreactor.io/docs/core/release/api/reactor/util/retry/Retry.html) `retrySpec)` method. A difference here is the presence of `.doBeforeRetry(retrySignal -> tokenSupplier.refreshToken())` expression which executes `tokenSupplier.refreshToken()` just before retrying.

```java
interface TokenSupplier {  
    String getToken();  
    void refreshToken();  
}  
  
...  
  
var requestWithRetry = request.retryWhen(RetrySpec.max(1)        // retry at most once  
        .doBeforeRetry(retrySignal -> tokenSupplier.refreshToken())  
        .filter(e -> e instanceof WebClientResponseException.Unauthorized));

Done? **Not quite**. The below does not work as `webClient` probably reused the original request instance thus not submitting the refreshed token obtained when we `doBeforeRetry`

var request = webClient.get()  
        .uri(PATH)  
        .headers(headers -> headers.set(HttpHeaders.AUTHORIZATION, tokenSupplier.getToken()))  
        .retrieve()  
        .bodyToMono(String.class)  
        ;  
var requestWithRetry = request.retryWhen(RetrySpec.max(1)        // retry at most once  
        .doBeforeRetry(retrySignal -> tokenSupplier.refreshToken())  
        .filter(e -> e instanceof WebClientResponseException.Unauthorized));
```

The solution is to re-create the request in each retry with the [`Mono.defer`](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#defer-java.util.function.Supplier-) method. This ensures that each request is created fresh with the current valid token. The above should be written as below — notice the entire request-creation expression is wrapped inside `Mono.defer()` method call:

```java
var request = Mono.defer(() -> webClient.get()  
        .uri(PATH)  
        .headers(headers -> headers.set(HttpHeaders.AUTHORIZATION, tokenSupplier.getToken()))  
        .retrieve()  
        .bodyToMono(String.class)  
);  
  
var requestWithRetry = request.retryWhen(RetrySpec.max(1)        // retry at most once  
        .doBeforeRetry(retrySignal -> tokenSupplier.refreshToken())  
        .filter(e -> e instanceof WebClientResponseException.Unauthorized));
```

The entire unit test is below for your reference:

{{< figure src="1_jGPgtzh641BG9RrIAv1A3w.webp" caption="All tests passed (screenshot)" >}}

```java
interface TokenSupplier {  
    String getToken();  
    void refreshToken();  
}  
  
@Test  
void retryIf401WithDifferentToken() throws InterruptedException {  
    String firstToken = "FIRST TOKEN";  
    String secondToken = "SECOND TOKEN";  
    TokenSupplier tokenSupplier = spy(new TokenSupplier() {  
        String currentToken = firstToken;  
  
        @Override  
        public String getToken() {  
            return currentToken;  
        }  
  
        @Override  
        public void refreshToken() {  
            currentToken = secondToken;  
        }  
    });  
  
    // request  
    var webClient = WebClient.builder().baseUrl(baseUrl).build();  
    var request = Mono.defer(() -> webClient.get()  
            .uri(PATH)  
            .headers(headers -> headers.set(HttpHeaders.AUTHORIZATION, tokenSupplier.getToken()))  
            .retrieve()  
            .bodyToMono(String.class)  
    );  
  
    var requestWithRetry = request.retryWhen(RetrySpec.max(1)        // retry at most once  
            .doBeforeRetry(retrySignal -> tokenSupplier.refreshToken())  
            .filter(e -> e instanceof WebClientResponseException.Unauthorized));  
  
  
    // response to first request with a 401  
    mockBackEnd.enqueue(new MockResponse().setResponseCode(HttpStatus.UNAUTHORIZED.value()));  
    // response to 2nd request with 200  
    mockBackEnd.enqueue(new MockResponse().setBody("Successful!"));  
  
    // no more WebClientResponseException.Unauthorized exception  
    assertDoesNotThrow(() -> requestWithRetry.block());  
  
    // assert that first request failed with a 401 with firstToken sent  
    RecordedRequest recordedRequest = mockBackEnd.takeRequest();  
    assertEquals(firstToken, recordedRequest.getHeader(HttpHeaders.AUTHORIZATION));  
  
    // assert that 2nd request passed with secondToken sent  
    recordedRequest = mockBackEnd.takeRequest();  
    assertEquals(secondToken, recordedRequest.getHeader(HttpHeaders.AUTHORIZATION));  
  
    // verify that tokenSupplier is invoked  
    verify(tokenSupplier, times(2)).getToken(); //  twice  
    verify(tokenSupplier).refreshToken();  
}
```

# Sample codes

Available in my [GitHub](https://github.com/geraldnguyen/kitchensink/blob/main/springweb/src/test/java/com/example/springweb/webclient/RetriableRequestTest.java).