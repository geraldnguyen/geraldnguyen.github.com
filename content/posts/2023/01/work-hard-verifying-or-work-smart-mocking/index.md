---
title: Work-hard Verifying or Work-smart Mocking
subtitle: Testing is hard work, let’s make it easier
date: 2023-01-04T09:19:42+01:00
image: 0_huB0so5ACBot2PYg.jpg
draft: false
categories: [Software Development]
tags: [Java, Programming, Testing]
medium: https://geraldnguyen.medium.com/work-hard-verifying-or-work-smart-mocking-9e0c645c839e
---

{{< figure src="0_huB0so5ACBot2PYg.webp" caption="Photo by [Tim Gouw](https://unsplash.com/@punttim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)" >}}

Let’s test this service

```java
@Service  
public class SampleService {  
    private final SampleRepository sampleRepository;  
  
    public SampleService(SampleRepository sampleRepository) {  
        this.sampleRepository = sampleRepository;  
    }  
  
    public SampleEntity createSampleEntity() {  
        SampleEntity sampleEntity \= new SampleEntity();  
        sampleEntity.setName("sample");  
  
        return sampleRepository.save(sampleEntity);  
    }  
}
```

If we want to test the method `createSampleEntity` from `SampleService`, there are 3 things we want to verify:

*   #1 — `sampleRepository.save` is called,
*   #2 — with the right `sampleEntity`,
*   and #3 — the resulting entity is returned

# Work hard Verifying

A typical unit test contains 3 stages: Arrange, Act, and Assert (or AAA)

**Arrange** is where we set up the test data and mock the test’s external dependencies. In the below example, we need to mock the method `sampleRepository.save` to return an expected entity.

**Act** is where we invoke the to-be-tested method with the right arguments. If the method returns something, we capture the returning value too. That is what the line `SampleEntity sampleEntity = sampleService.createSampleEntity()` does.

**Assert** is when we validate the #1, #2, and #3 above:

*   Validating #3 is easy in this example: `assertEquals(mockEntity, sampleEntity)` will assert that the mocked entity and the value returned from the tested method are `equals`.
*   Validating #1 would require the use of `verify` to ascertain the repository’s `save()` was indeed called.
*   Validating #2 requires the use of `ArgumentCaptor` to capture the argument to `save(SampleEntity)`. We will then perform another set of assertions on the captured value to make sure that the right parameter is passed to it

```
@Test  
void workHardVerifying() {  
    // arrange  
    SampleEntity mockEntity \= new SampleEntity();  
    mockEntity.setId(1L);  
    when(sampleRepository.save(any(SampleEntity.class))).thenReturn(mockEntity);  
  
    // act  
    SampleEntity sampleEntity \= sampleService.createSampleEntity();  
  
    // assert - the right entity returned  
    assertEquals(mockEntity, sampleEntity);  
      
    // assert - the right param passed  
    ArgumentCaptor<SampleEntity> sampleEntityArgumentCaptor = ArgumentCaptor.forClass(SampleEntity.class);  
    verify(sampleRepository).save(sampleEntityArgumentCaptor.capture());  
    assertEquals("sample", sampleEntityArgumentCaptor.getValue().getName());  
    assertNull(sampleEntityArgumentCaptor.getValue().getId());  
}
```

# Work-smart Mocking

The same effect can be achieved in the below code.

```
@Test  
void workSmartMocking() {  
    // arrange  
    when(sampleRepository.save(any(SampleEntity.class))).thenAnswer(a -> {  
        // assert - the right param passed  
        SampleEntity entity \= a.getArgument(0);  
        assertEquals("sample", entity.getName());  
        assertNull(entity.getId());  
        entity.setId(1L);  
        return entity;  
    });  
  
    // act  
    SampleEntity sampleEntity \= sampleService.createSampleEntity();  
  
    // assert - the right entity returned  
    assertEquals(1L, sampleEntity.getId());  
}
```

By `assertEquals` the entity’s id, we effectively verify that the return value matches the mocked value (validating #3) AND`sampleRepository.save` is called (validating #1.)

Furthermore, as we moved several assertions into the mocking of `sampleRepository.save`, we are able to verify that the right parameter has been passed in.

# Conclusion

While the difference in lines of codes is not significant (in fact, just 1 line), the work-hard-verifying approach is 35% more verbose compared to the work-smart-mocking approach (790 chars vs 584 chars). If lean code is your preference, I believe you will also prefer to shave as much code as possible while maintaining the same clarity.

To be honest, I prefer _strict_ mocking over relaxed mocking + `verify`. That is, if my mocking becomes necessary (because of _strict_ mocking), it also functions as a `verify`. We can then save even more codes.

Check out the code from my [GitHub](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/example/service/SampleServiceTest.java).