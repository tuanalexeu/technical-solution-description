<h1 align="center">
<img src="https://dwglogo.com/wp-content/uploads/2017/12/Spring_Framework_logo_01.png" alt="MkDocs icon" width="170">
<br>Spring Cloud Configuration Server
</h1>

## Description

<p>
The next microservice is a centralized configuration server supported by Spring Cloud. 
This is the place where all configuration properties are stored. 
Storing all the data in one place makes it easier to maintain the whole project, since I can easily view the files.
</p>

<!-- https://shields.io/ -->

## Project structure

This is a microservice, but it does not store the data itself. 
Instead, a private Git repository is used, so that anonymous users cannot access the data.

Here's how you can enable Spring Cloud Config Server:
```java
@SpringBootApplication
@EnableConfigServer
public class ConfigService {
    public static void main(String[] args) {
        SpringApplication.run(ConfigService.class, args);
    }
}
```

application.yml:
```yaml
server:
  port: 8888
spring:
  cloud:
    config:
      server:
        git:
          uri: ${GIT_CONFIG_URI}
          searchPaths: logiweb-service, client-order-service

# enable all actuator endpoints for testing (review this for real deployments)
management.endpoints.web:
  include: '*'
```

## Technology stack
<dl>
<li>Spring Boot</li>
<li>Spring Cloud</li>
</dl>

