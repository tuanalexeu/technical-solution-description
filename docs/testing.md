<h1 align="center">
<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAdUAAABrCAMAAAAW0Oh0AAAAyVBMVEX///+zAAAejQCwAADSf3378fCuAAC4Jya8PT3YlJLkvr7z1tYAhgDC2cDr0NDotLTuxMTGUVHKc3PObW3y4OC1BgZpr1ner6/BOTnJW1u4GBhXo0rF377nurr02tq0Dw+jt4pjq1XgpqXGYWG4LS2GvnjQ48zFSkr89fUAgwC6OjrIWVnUhoW9KyvamprRfHtImz07kSLd4c/v6uKxwpuky6FCjyb/6++XtH8+nSNQpjY1mRq6ISGNv4OCs27W5tRCmzuDqGbi0sf2X9SZAAAEvUlEQVR4nO2da3+aSBSHxQlWkVAbbbx01dZkI4k2124bu27a7vf/UOsNmHNmoEgGwpr/83JmDszPxxlgLlCpAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAADwuvG7jOFL1wg8H3ssKOcvXaP/O39+OiV8uAhyGtN3lPqIhg7lAtNJ9jrYbyxKPcrzO1uaOzrZT/OKOHWrhMvjIOeGNSBR82jo25qc2chehySrjjXYsMsR7eyneUV8oFKrbmj1SLDfWnCrcgGRl9UarQOspuG07FZpLWA1FbB6iJS+B4bVDKCtHiJlt9qF1Qx8KrnVBqxmAFYPkVL0wEum7irKg9UslMLqtEeZRXmwmoUyPNkkAatZKLKt2iv8PesHq1kwb3XUpGxTvfnn7TSANb1mcz8Vn0U0R2Hi6IRZnY3IYYEW8z3wDQuzV2nOVISF12KpV3vJQq50iQz+1wAS5tsqj7Mr/pFgLc6ayz2x9slGSYTV9ORv1a+c8SOte1I7OlAWq+9hNQHzPbBitUGnSHflz6IDwappcm+r44naUjcB0ZIYWDVN7lYH4xgvIjwQrJom/x44DuEEEbBqmvzvlmKtzoMIWDXNy1m1BkEErJrG/EyczqoQOtVBhN7qcjsUpTnUJh1WEyjCqhi0j06ulORaEKG3ejbrr+ixrDf9HbCaQAE9sKhvRhzaPL0WrMNPmjW/xuh+Boq4rm7jOpmsYs4mC/k/2YjdHLhf51ZbuwhYNU3+bTV8guHDwbCaGwVYfbvLOIHVoiigBw7kwWph5N9Ww5siWC0MWD1EntEDN2C1rDyjrV7DalmB1UPkdg+r7IUesFpanmF1Dqslxb5jVqvhm3lUqzc0thCrbVjdnwsuNcGq/GOv8MkkWV5W+7C6P09urNW5YtUi2yA8ki+6QXq+VqUFpyCGe0Xq3UOQ11Wsir4UylSE44L5WrXkKoCIL3897LZD2B8vlQ7467eg3FCzuiRqKUOeFz725Gu1vu+mulfC46XrLhbfF4u/XaWlVqu3YbmRInW9QabRajabrclnvh4/eqFkvlbDGSBA4OMOlNOo4MDSsFkfpi4Skzpns1Z5kGXNuo5zfbbEuiXCR00LDXH/iQpqbpdikcadcn1etYJFhlhjSFFve2WrP6KCzfRSrWW0xc2sVc3VfQPWA1MSrd7KNyOz1I1Vfn+AWastWE1FUg/sPsklvdRWz6U/g1mrcVvOYZWSeLd0T4qqFzU9oZ81Zq3qtjTDqkpCW6VNdcW7VFprcznGsFUHVtPwGGvV/aoU7uv2iXOp9KU8hq0qT6ywqiO2B3Z/aj5VwF/UoSCsLo8wa7WjPS2sUuJ6YPfnN11xL7kXFjP+5iPTViuepakBrFL0TzZulV9TQya9uPYqrLo6gGfcaqUz1VwHYJXweKlodd27px/xEb43O1dHCYUY9z3NWPuN/EGU9TdRAqttnhFaHdOcmvKVIqcub4HF2JKG4y/f79Ymt6ykLn4d278L8hr9QbglePX7vu/NW/ovBjUdyjAw77EMJzipzzMczVGH7elyvKE3PZk4eOOdBv/h3z+2XNxrL6faoFFruMX77b8AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAIDD4j+1tHV71mDs6QAAAABJRU5ErkJggg==" alt="MkDocs icon" width="170">
<br>Testing with Mockito & JUnit4
</h1>

## Description

<p>
JUnit 4 and Mockito were used for testing. H2 for integration tests
</p>

<!-- https://shields.io/ -->

## How it works

Dependencies for testing:
```xml
...
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-test</artifactId>
    <scope>test</scope>
    <version>2.5.0</version>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <scope>test</scope>
</dependency>

<dependency>
<groupId>org.junit.vintage</groupId>
<artifactId>junit-vintage-engine</artifactId>
<scope>test</scope>
<exclusions>
    <exclusion>
        <groupId>org.hamcrest</groupId>
        <artifactId>hamcrest-core</artifactId>
    </exclusion>
</exclusions>
</dependency>
...
```

## Unit tests
In order to test my business logic, I used JUnit & Mockito Frameworks.
Here's an example test class for a service:
```java
@RunWith(MockitoJUnitRunner.class)
public class DriverServiceTest {

    @InjectMocks
    DriverServiceImpl service;

    @Mock
    DriverDaoImpl dao;

    @Mock
    ModelMapper mapper;
    
    @Before
    public void init() {
        MockitoAnnotations.initMocks(this);
    }

    @Test
    public void findAllTest() {
        // Some code here
    }

    @Test
    public void findByIdTest() {
        // Some code here
    }
}
```

## Integration tests
I created property file to connection to embedded database:
```yaml
spring:
  datasource:
    url: jdbc:h2:mem:test
  jpa:
    properties:
      hibernate:
        dialect: org.hibernate.dialect.H2Dialect
```

An example test class that uses configuration above:
```java
@RunWith(SpringRunner.class)
@SpringBootTest(
  SpringBootTest.WebEnvironment.MOCK,
  classes = Application.class)
@AutoConfigureMockMvc
@TestPropertySource(
  locations = "classpath:application-integration-test.yaml")
public class EmployeeRestControllerIntegrationTest {

    @Autowired
    private MockMvc mvc;

    // Some test methods here
}
```