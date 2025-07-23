---
title: "How to fix Spring dependency error: 'expected single matching bean but found X'"
subtitle: From beginner to advanced, learn 5 ways to tackle this error
# Original publication date: 2023-02-06
# Add image if the article has a main image
# Example image: image: example.jpg
date: 2023-02-06T00:00:00+01:00
categories: [Programming Tutorials]
tags: [Spring, Dependency Injection, Java, Beginner, Advanced, Medium]
image: 0_bHk8jRkL7cnE4wPi.jpg
medium: https://levelup.gitconnected.com/how-to-fix-spring-dependency-error-expected-single-matching-bean-but-found-multiple-b93f4c950824
---

{{< figure src="https://miro.medium.com/v2/resize:fit:1100/format:webp/0*bHk8jRkL7cnE4wPi" caption="Photo by R. Mac Wheeler on Unsplash" >}}

TL;DR

1. Designate a `@Primary` bean
2. Be specific with `@Qualifier`
3. Accepting multiple beans
4. Explicit Bean configuration
5. (Advanced) Feature Switching

Sample codes: [https://github.com/geraldnguyen/sample-spring-core/tree/dependency/multiple-beans](https://github.com/geraldnguyen/sample-spring-core/tree/dependency/multiple-beans)

---

## The Error: “expected single matching bean but found X”

> Checkout the branch `dependency/multiple-beans` from [https://github.com/geraldnguyen/sample-spring-core](https://github.com/geraldnguyen/sample-spring-core) to follow along

We have a simple Spring boot application setup:

{{< figure src="https://miro.medium.com/v2/resize:fit:482/format:webp/1*mp-BxqWBd50XcRtnXIEG9A.png" >}}


- `SearchService`: the interface for search operations
- `BingSearch`: an implementation of SearchService
- `GoogleSearch`: an implementation of SearchService
- `SearchController`: a consumer of `SearchService` and a convenient entry point for our testing

```java
// SearchService: the interface for search operations
public interface SearchService {
    String[] search(String query);
}

// BingSearch: an implementation of SearchService
@Service
public class BingSearch implements SearchService {
    @Override
    public String[] search(String query) {
        return new String[]{ "Bing sample result" };
    }
}

// GoogleSearch: an implementation of SearchService
@Service
public class GoogleSearch implements SearchService {
    @Override
    public String[] search(String query) {
        return new String[]{ "Google sample result" };
    }
}

// SearchController: a consumer of SearchService and a convenient entry point for our testing
@RestController
@RequestMapping("/search/")
public class SearchController {
    @Autowired
    private SearchService searchService;

    @GetMapping
    public String[] search(@RequestParam("query") String query) {
        return searchService.search(query);
    }
}
```

When starting the application, you will encounter the error:

```
UnsatisfiedDependencyException: Error creating bean with name 'searchController': Unsatisfied dependency expressed through field 'searchService': No qualifying bean of type 'nguyen.gerald.samples.spring.core.service.search.SearchService' available: expected single matching bean but found 2: bingSearch,googleSearch
```

```

023-02-02T23:40:32.430+08:00  WARN 42400 --- [           main] ConfigServletWebServerApplicationContext : Exception encountered during context initialization - cancelling refresh attempt: org.springframework.beans.factory.UnsatisfiedDependencyException: Error creating bean with name 'searchController': Unsatisfied dependency expressed through field 'searchService': No qualifying bean of type 'nguyen.gerald.samples.spring.core.service.search.SearchService' available: expected single matching bean but found 2: bingSearch,googleSearch
2023-02-02T23:40:32.436+08:00  INFO 42400 --- [           main] o.apache.catalina.core.StandardService   : Stopping service [Tomcat]
2023-02-02T23:40:32.457+08:00  INFO 42400 --- [           main] .s.b.a.l.ConditionEvaluationReportLogger : 

Error starting ApplicationContext. To display the condition evaluation report re-run your application with 'debug' enabled.
2023-02-02T23:40:32.481+08:00 ERROR 42400 --- [           main] o.s.b.d.LoggingFailureAnalysisReporter   : 

***************************
APPLICATION FAILED TO START
***************************

Description:

Field searchService in nguyen.gerald.samples.spring.core.api.SearchController required a single bean, but 2 were found:
 - bingSearch: defined in file [C:\Users\huy_n\workspace\samples\spring\core\target\classes\nguyen\gerald\samples\spring\core\service\search\BingSearch.class]
 - googleSearch: defined in file [C:\Users\huy_n\workspace\samples\spring\core\target\classes\nguyen\gerald\samples\spring\core\service\search\GoogleSearch.class]


Action:

Consider marking one of the beans as @Primary, updating the consumer to accept multiple beans, or using @Qualifier to identify the bean that should be consumed

Disconnected from the target VM, address: '127.0.0.1:62797', transport: 'socket'

Process finished with exit code 1te
```

Because both `GoogleSearch` and `BingSearch` implements `SearchService`, Spring was unable to determine which implementation should be returned to `SearchController`, hence it throws the above error.

---

## Solution #1 — Designate a Primary Bean

> Checkout the branch `dependency/multiple-beans-solution-primary` from [https://github.com/geraldnguyen/sample-spring-core](https://github.com/geraldnguyen/sample-spring-core) to follow along. You can also preview the changes [here](https://github.com/geraldnguyen/sample-spring-core/compare/dependency/multiple-beans...dependency/multiple-beans-solution-primary)

This is the simplest solution. We can designate either implementation, e.g. `GoogleSearch`, as the primary `SearchService` implementation by applying the `@Primary` annotation to the implementation, i.e. `GoogleSearch`, class:

```java
@Primary  // <-- this is the only change
@Service
public class GoogleSearch implements SearchService {
    @Override
    public String[] search(String query) {
        return new String[]{ "Google sample result" };
    }
}
```

Let’s start our application. The error should be no more and we are able to access mocked Google search result at [http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)

{{< figure src="https://miro.medium.com/v2/resize:fit:455/1*fd8RC1jDGHE9TntlEfZZiA.png" caption="[http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)" >}}

---

## Solution #2— Be specific with `@Qualifier` Annotation

> Checkout the branch `dependency/multiple-beans-solution-qualifier` from [https://github.com/geraldnguyen/sample-spring-core](https://github.com/geraldnguyen/sample-spring-core) to follow along. You can also preview the changes [here](https://github.com/geraldnguyen/sample-spring-core/compare/dependency/multiple-beans...dependency/multiple-beans-solution-qualifier)

This is just as simple. We can specify which `SearchService` to use by applying `@Qualifier` annotation to the dependency property (or parameter if you use constructor injection):

```java
@RestController
@RequestMapping("/search/")
public class SearchController {
    @Autowired
    @Qualifier("bingSearch")      // <-- this is the only change
    private SearchService searchService;
    @GetMapping
    public String[] search(@RequestParam("query") String query) {
        return searchService.search(query);
    }
}
```

Let’s start our application. The error should be no more and we are able to access mocked Bing search result at [http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)

{{< figure src="https://miro.medium.com/v2/resize:fit:458/1*aY6_yUcKDJeAju5gZY7Ibw.png" caption="[http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)" >}}

---

## Solution #3 —Accepting Multiple Beans

> Checkout the branch `dependency/multiple-beans-accepting-multiple` from [https://github.com/geraldnguyen/sample-spring-core](https://github.com/geraldnguyen/sample-spring-core) to follow along. You can also preview the changes [here](https://github.com/geraldnguyen/sample-spring-core/compare/dependency/multiple-beans...dependency/multiple-beans-accepting-multiple)

The `SearchController` can accept multiple beans and use them as it wishes. The following example has it pick the bean at index 0 to use:

```java
@RestController
@RequestMapping("/search/")
public class SearchController {
    @Autowired
    private SearchService[] searchServices; // <-- accepting multiple
    @GetMapping
    public String[] search(@RequestParam("query") String query) {
        return searchServices[0].search(query);  // <-- pick the first one to use
    }
}
```

This solution is more suited toward aggregation use cases where, for example, the API sends the user’s query to multiple search engines and aggregates their individual responses before returning to the user.

---

## Solution #4 — Explicit Bean Configuration

> Checkout the branch `dependency/multiple-beans-configuration` from [https://github.com/geraldnguyen/sample-spring-core](https://github.com/geraldnguyen/sample-spring-core) to follow along. You can also preview the changes [here](https://github.com/geraldnguyen/sample-spring-core/compare/dependency/multiple-beans...dependency/multiple-beans-configuration)

In this approach, we explicitly pick an implementation in an external configuration class. The benefit of this approach is that we can leave the consumer, interface, and implementation unmodified. This is useful when you want to keep the configuration details out of the business logic, especially so when you do not have access to the source codes of consumer and service classes.

{{< figure src="https://miro.medium.com/v2/resize:fit:304/1*fLtS2lJxMkdT1vdNfNmazA.png" caption="External configuration within `SearchConfig`" >}}

External configuration within `SearchConfig` can be as trivial as the following:

```java
@Configuration
public class SearchConfig {
    @Autowired
    private GoogleSearch googleSearch;
    @Autowired
    private BingSearch bingSearch;
    @Bean
    public SearchService searchService() {
        return googleSearch;
    }
}
```

Or we can add an enable/disable toggle capability to make our application more flexible and more externally driven. Even though we define a property `google-search.enabled` in the `application.properties`, it needs not be declared so because Springboot supports multiple modes of [external configurations](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config).

```java
// SearchConfig.java
@Configuration
public class SearchConfig {
    @Autowired
    private GoogleSearch googleSearch;
    @Autowired
    private BingSearch bingSearch;
    @Bean
    public SearchService searchService(@Value("${google-search.enabled:false}") boolean enableGoogleSearch) {
        return enableGoogleSearch ? googleSearch : bingSearch;
    }
}

// application.properties
google-search.enabled=true
```

Let’s start our application. We are able to access the mocked Google search result at [http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)


{{< figure src="https://miro.medium.com/v2/resize:fit:455/1*fd8RC1jDGHE9TntlEfZZiA.png" caption="[http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)" >}}

---

## (Advanced) Solution #5 — Feature Switching

> Checkout the branch `dependency/multiple-beans-feature-switch` from [https://github.com/geraldnguyen/sample-spring-core](https://github.com/geraldnguyen/sample-spring-core) to follow along. You can also preview the changes [here](https://github.com/geraldnguyen/sample-spring-core/compare/dependency/multiple-beans...dependency/multiple-beans-feature-switch)

The previous solution works well if we only have to choose either Google or Bing. But it will not scale when we have 3 or more options. Feature switching is a much better solution.

Let’s have `YahooSearch` enter the competition:

```java
// YahooSearch.java
@Service
public class YahooSearch implements SearchService {
    @Override
    public String[] search(String query) {
        return new String[]{ "Yahoo sample result" };
    }
}
```

Implementing feature switching requires only 3 steps:

- We will define a `SearchProvider` enum with the names of all supported search providers.
- Then we configure `search.provider=<a name>` in `application.properties` (or one of the supported [external configurations](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config) modes).
- Then we read the configured `search.provider` value as a parameter in our `@Bean` configuration and return the search provider corresponding to that parameter.

```java
// SearchConfig.java
@Configuration
public class SearchConfig {
    enum SearchProvider { GOOGLE, BING, YAHOO }
    @Autowired
    private GoogleSearch googleSearch;
    @Autowired
    private BingSearch bingSearch;
    @Autowired
    private YahooSearch yahooSearch;
    @Bean
    public SearchService searchService(@Value("${search.provider:BING}") SearchProvider provider) {
        return switch (provider) {
            case BING -> bingSearch;
            case YAHOO -> yahooSearch;
            default -> googleSearch;
        };
    }
}

// application.properties
search.provider=YAHOO
```

Let’s start our application. We are able to access the mocked Yahoo search result at [http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)

{{< figure src="https://miro.medium.com/v2/resize:fit:454/1*BQJ7pVTgdChaLOxjuEnRhg.png" caption="[http://localhost:8080/search/?query=hello](http://localhost:8080/search/?query=hello)" >}}

---

# Conclusion

In this article, we have learned 5 different ways to resolve the expected-single-matching-bean-but-found-multiple error with code samples and easy-to-follow steps.

If you have enjoyed this tutorial, please [subscribe](https://geraldnguyen.medium.com/subscribe) to get my latest article delivered to your email. Thank you.
