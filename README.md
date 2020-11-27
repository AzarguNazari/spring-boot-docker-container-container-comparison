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


