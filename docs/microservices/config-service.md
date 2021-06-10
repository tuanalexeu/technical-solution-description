<h1 align="center">
<br><img src="https://dwglogo.com/wp-content/uploads/2017/12/Spring_Framework_logo_01.png" alt="MkDocs icon" width="170">
<br>Spring Cloud Configuration Server
</h1>

## Description

<p>
The last but not least microservice is Centralized Config Service which is a great tool for storing 
properties of all services in one place, so that it's easier to maintain. This is done by using Spring Cloud.
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
          searchPaths: logiweb-service, client-order-service,chat-service

# enable all actuator endpoints for testing (review this for real deployments)
management.endpoints.web:
  include: '*'
```

## Technology stack
<dl>
<li>Spring Boot</li>
<li>Spring Cloud</li>
</dl>

