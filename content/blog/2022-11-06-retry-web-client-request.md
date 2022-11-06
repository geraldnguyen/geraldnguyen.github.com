---
title: "Retry WebClient Request"
date: 2022-11-06T09:19:42+01:00
draft: false
weight: 50
lead: Retrying a WebClient request using RetrySpec, and unit-testing it
tags: WebClient, Retry, Authorization
---

## Problem Statement

Sometimes web request fails, for whatever reason, and you need to retry it

As usual, unit-testing it:

```
@ParameterizedTest
@EnumSource(value = HttpStatus.class, mode = EnumSource.Mode.INCLUDE, names = {
        "BAD_REQUEST", "UNAUTHORIZED", "FORBIDDEN",
        "SERVICE_UNAVAILABLE", "INTERNAL_SERVER_ERROR"
})
void throwExceptionWhenReceiveNon200Response(HttpStatus status) throws InterruptedException {
    // request
    var webClient = WebClient.builder().baseUrl(baseUrl).build();
    var request = webClient.get()
            .uri("/employee/100")
            .retrieve()
            .bodyToMono(String.class);

    // response to first request with fail status
    mockBackEnd.enqueue(new MockResponse().setResponseCode(status.value()));

    // assert that first request failed with a 401
    assertThrows(WebClientResponseException.class, () -> request.block());
}
```

## Retry unchanged

The easiest is just retrying with the same request details.

[retryWhen](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#retryWhen-reactor.util.retry.Retry-)([Retry](https://projectreactor.io/docs/core/release/api/reactor/util/retry/Retry.html "class in reactor.util.retry") retrySpec) is a simple way to set a retry specification e.g. in the form of a [RetrySpec](https://projectreactor.io/docs/core/release/api/reactor/util/retry/RetrySpec.html "class in reactor.util.retry")

We can specify different retry condition such as `.filter(e -> e instanceof WebClientResponseException.ServiceUnavailable))` or maxinum number of retries `.max(1)`


```
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


## Retry with modification

It is trickier to retry with modification. The trick is to re-create the request in each retry with [`defer`](https://projectreactor.io/docs/core/release/api/reactor/core/publisher/Mono.html#defer-java.util.function.Supplier-) and perform the modification action before the retry e.g. [`doBeforeRetry`](https://projectreactor.io/docs/core/release/api/reactor/util/retry/RetrySpec.html#doBeforeRetry-java.util.function.Consumer-)



```
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
            .uri("/employee/100")
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

## Sample codes

Checkout the [sample codes](https://github.com/geraldnguyen/kitchensink/blob/main/springweb/src/test/java/com/example/springweb/webclient/RetriableRequestTest.java) at https://github.com/geraldnguyen/kitchensink/blob/main/springweb/src/test/java/com/example/springweb/webclient/RetriableRequestTest.java