<h1 align="center">
<br><img src="https://dwglogo.com/wp-content/uploads/2017/12/Spring_Framework_logo_01.png" alt="MkDocs icon" width="170">
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