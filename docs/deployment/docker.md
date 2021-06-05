<h1 align="center">
<img src="https://raw.githubusercontent.com/peaceiris/mkdocs-material-boilerplate/master/docs_sample/images/graduate-cap.png" alt="MkDocs icon" width="170">
<br>Docker containers
</h1>

## Description

<p>So far, I have 4 microservices and this site. 
It's not good to run them all together within one environment, that'll cause ports/resources conflicts.
So, I needed something that will help me out. Docker is my solution. Now I'm able to run all the applications separately,
each in inside its own virtual environment.</p>

<!-- https://shields.io/ -->

## Structure

In order to automate the process of building images, I included docker spotify plugin in my maven:

```xml
<build>
    <plugins>
        <plugin>
            <groupId>com.spotify</groupId>
            <artifactId>docker-maven-plugin</artifactId>
            <version>0.4.10</version>
            <configuration>
                <baseImage>java</baseImage>
                <imageName>example</imageName>
            </configuration>
        </plugin>
    </plugins>
</build> 
```

By running `mvn clean install docker:build` all the images will be built automatically.

An example Dockerfile:

```dockerfile
FROM openjdk:8-jre-alpine
ENV APP_FILE @project.build.finalName@.war
ENV APP_HOME /usr/apps
EXPOSE 8080
COPY $APP_FILE $APP_HOME/
WORKDIR $APP_HOME
ENTRYPOINT ["sh", "-c"]
CMD ["exec java -jar $APP_FILE"]
```

All that's left to do is to run the image as a container and forward outer port to the one we will access inside container.<br>
This can be done by `docker run -d --restart=always -p 8080:80 image_name:version`