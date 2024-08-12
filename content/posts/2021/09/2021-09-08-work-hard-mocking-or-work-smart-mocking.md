---
title: "Work-hard Mocking or Work-smart Mocking"
date: 2021-09-08T09:19:42+01:00
draft: false
weight: 50
tags: [java, kitchensink, unit testing, mockito, programming]
---

To be honest, I prefer *strict* mocking over relaxed mocking + `verify`. That is, if my mocking becomes neccessary (because of *strict* mocking), it also functions as a `verify`. I can save a few chunks of codes by sacrifying a little explicitness and sometimes verbosity of `verify` and `ArgumentCaptor` setup.

Here's a sample

Assuming we want to test method `createSampleEntity` from service `SampleService`, there are 3 things we want to verify: i) `sampleRepository.save` is called, ii) with the right `sampleEntity`, and iii) the resulted entity is returned

```
@Service
public class SampleService {
    private final SampleRepository sampleRepository;

    public SampleService(SampleRepository sampleRepository) {
        this.sampleRepository = sampleRepository;
    }

    public SampleEntity createSampleEntity() {
        SampleEntity sampleEntity = new SampleEntity();
        sampleEntity.setName("sample");

        return sampleRepository.save(sampleEntity);
    }
}
```


## Work hard Mocking with `verify` and `ArgumentCaptor`

In a typical unit test using `verify`, we need to mock `sampleRepository.save` method returning an expected entity. We will then `assertEquals` the mocked entity and the value returned from `sampleService.createSampleEntity()` method to ascertain the repository's `save()` was indeed called. Lastly, we will have to capture the argument to `save(SampleEntity)` to make sure that the right parameter is passed to it

```
    @Test
    void workHardMocking() {
        // arrange
        SampleEntity mockEntity = new SampleEntity();
        mockEntity.setId(1L);
        when(sampleRepository.save(any(SampleEntity.class))).thenReturn(mockEntity);

        // act
        SampleEntity sampleEntity = sampleService.createSampleEntity();

        // assert - the right entity returned
        assertEquals(mockEntity, sampleEntity);
        // assert - the right param passed
        ArgumentCaptor<SampleEntity> sampleEntityArgumentCaptor = ArgumentCaptor.forClass(SampleEntity.class);
        verify(sampleRepository).save(sampleEntityArgumentCaptor.capture());
        assertEquals("sample", sampleEntityArgumentCaptor.getValue().getName());
        assertNull(sampleEntityArgumentCaptor.getValue().getId());
    }
```

## Work-smart Testing 

The same effect can be achieved by the below strategy. By `assertEquals` the entity's id, we effectively verify the right return value and `sampleRepository.save` is called. Furthermore, as we planted several `assertXYZ` method inside the mocking of `sampleRepository.save`, we have also verified that the right parameter has been passed in

```
    @Test
    void workSmartMocking() {
        // arrange
        when(sampleRepository.save(any(SampleEntity.class))).thenAnswer(a -> {
            // assert - the right param passed
            SampleEntity entity = a.getArgument(0);
            assertEquals("sample", entity.getName());
            assertNull(entity.getId());
            entity.setId(1L);
            return entity;
        });

        // act
        SampleEntity sampleEntity = sampleService.createSampleEntity();

        // assert - the right entity returned
        assertEquals(1L, sampleEntity.getId());
    }
```

While the difference in LOC is not significant (0 actually), the former approach is 35% more verbose compared to the later (790 chars vs 584 chars). If clean code is your preference, I believe you will also prefer to shave as much code as possible while maintaining the same clarity.

Checkout the code from [https://github.com/geraldnguyen/kitchensink](https://github.com/geraldnguyen/kitchensink/blob/main/java/src/test/java/example/service/SampleServiceTest.java)