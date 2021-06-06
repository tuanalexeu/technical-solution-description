<h1 align="center">
<img src="https://dwglogo.com/wp-content/uploads/2017/12/Spring_Framework_logo_01.png" alt="MkDocs icon" width="170">
<br>Kotlin and Spring for a chat
</h1>

## Description
Our clients should be able to get in touch with staff. For that purpose, 
I developed a simple chat where client can ask us questions. This is a simple, but very useful application written on Kotlin.
<!-- https://shields.io/ -->

## Project structure
At first, Java was the main language for the application. 
In order to make more modern, I decided to use Kotlin instead.

Since the whole project uses Maven as a build tool, I didn't use gradle.
Here's a part of pom.xml where I define Kotlin plugin:

```xml
<plugin>
    <groupId>org.jetbrains.kotlin</groupId>
    <artifactId>kotlin-maven-plugin</artifactId>
    <configuration>
        <compilerPlugins>
            <plugin>jpa</plugin>
            <plugin>spring</plugin>
            <plugin>all-open</plugin>
        </compilerPlugins>
        <pluginOptions>
            <option>all-open:annotation=javax.persistence.Entity</option>
            <option>all-open:annotation=javax.persistence.Embeddable</option>
            <option>all-open:annotation=javax.persistence.MappedSuperclass</option>
        </pluginOptions>
        <args>
            <arg>-Xjsr305=strict</arg>
        </args>
    </configuration>
    <dependencies>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-noarg</artifactId>
            <version>${kotlin.version}</version>
        </dependency>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-maven-allopen</artifactId>
            <version>${kotlin.version}</version>
        </dependency>
    </dependencies>
</plugin>            
```
It defines some configuration properties, such as default modifier of .kt classes or target build location.

## Chat
WebSocket technology and MQ were used to implement chat.
Here's an example config class:

```kotlin
@Configuration
@EnableWebSocketMessageBroker
class WebSocketConfig : WebSocketMessageBrokerConfigurer {

    override fun registerStompEndpoints(registry: StompEndpointRegistry) {
        registry.addEndpoint("/ws").withSockJS()
    }

    override fun configureMessageBroker(registry: MessageBrokerRegistry) {
        registry.setApplicationDestinationPrefixes("/app")
        registry.enableSimpleBroker("/topic")
    }
}
```

## Technology stack
<dl>
<li>Spring Boot</li>
<li>Kotlin</li>
<li>Maven</li>
</dl>