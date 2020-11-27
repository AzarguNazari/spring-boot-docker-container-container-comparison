# Comparing Spring Boot Application with different dockerization strategies

In here, I am going to compare different ways of dockerizing spring boot application. I am going to use a simple spring application with a `hello` return rest api.

```
  @GetMapping("/hello")
  public String hello(){
    return "Hello";
  }

```

## Simple way of using `jar` file

```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ARG JAR_FILE
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
```
With this approach, the size of container is approximately ~ `122 MB`


## Optimized way of using dependencies copies

Before using this method, make sure you build the dependeinces under `build/dependency` or `target/dependency`
```
FROM openjdk:8-jdk-alpine
RUN addgroup -S spring && adduser -S spring -G spring
USER spring:spring
ARG DEPENDENCY=./build/dependency
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY ${DEPENDENCY}/META-INF /app/META-INF
COPY ${DEPENDENCY}/BOOT-INF/classes /app
ENTRYPOINT ["java","-cp","app:app/lib/*","com.example.demo.DemoApplication"]
```

By using this method, we have container with same size of ~ `122 MB` but the only difference is that in this way, our initialization of application is faster and optimized.




