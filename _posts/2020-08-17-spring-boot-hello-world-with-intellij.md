---
layout: post
title: Spring Boot hello-world 프로젝트 만들어보기 (with intellij)
---

오늘은 intellij 무료 버전으로 간단한 Spring Boot 프로젝트를 만들어보려고 합니다.


내용을 시작하기에 앞서 양해의 말씀을 드리자면, (현재 회사에서는 파이썬으로 개발을 하고 있어서) 여가 시간에 따로 spring 에 대해서 배워가면서 작성했으므로 부족한 점이 있을 것으로 생각됩니다.

우선 퇴근 후의 시간이나 주말에 조금씩 배워보고자 hello-world 프로젝트로 시작했습니다.

## 시작하기

처음에 intellij 무료 버전에는 별도의 설정이 없어서 당황하다가 Spring Initializr 를 보게되었습니다. 

https://start.spring.io/


## maven 

maven 을 처음 접해 본 사람으로서 파이썬의 PIP보다 더 큰 범위의 도구라고 느껴졌습니다.

pom.xml 파일에 spring-boot-starter 의존성을 기입하여 maven 저장소에 존재하는 spring-boot-starter를 가져옵니다.

만약 Spring Initializr 로 Spring Boot 프로젝트를 생성했다면 기본적으로 작성되어 있습니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

spring-boot-starter-parent 상속받아서 각종 Spring Boot 의존성을 해결합니다.

```xml
    <groupId>org.example</groupId>
    <artifactId>springTest</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <java.version>14</java.version>
    </properties>
```

프로젝트 고유 정보와 자바버전 상수를 작성합니다.

```xml
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.junit.vintage</groupId>
                    <artifactId>junit-vintage-engine</artifactId>
                </exclusion>
            </exclusions>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>
```

spring-boot-starter 의존성을 작성합니다.

해당 부분도 Spring Initializr 로 Spring Boot 프로젝트를 만들었다면 기본적으로 작성되어 있습니다.

## 추가 작업

해당 Hello world 프로젝트에서 간단하게 REST api, 웹 페이지 요청을 구현해보려 Spring Web 과 템플릿 엔진인 Thymeleaf 를 pom.xml 파일 의존성에 추가합니다.

해당 작업도 Spring Initializr 에서 Spring Web, Thymeleaf 의존성을 미리 추가해놓으면 넘어갈 수 있습니다.

```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-thymeleaf</artifactId>
        </dependency>
```

## SpringBoot Application


```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class SpringTestApplication extends SpringBootServletInitializer {
    public static void main(String[] args) {
        SpringApplication.run(SpringTestApplication.class, args);
    }
}
```

SpringBootServletInitializer 를 상속받아 메인 함수를 구현합니다.

SpringBootServletInitializer 상속받는 이유를 알아보기 위해 "Spring Boot 웹 프로젝트 SpringBootServletInitializer 상속"이라는 키워드로 검색해보니 war 배포를 할 때 필요하다고 합니다.

자세한 내용은 이후에 개인적으로 점점 더 알아보려 합니다.

## REST API

GET 요청으로 데이터를 받기 위해서는 RequestMapping 어노테이션을 사용합니다.

```java
import org.springframework.web.bind.annotation.*;

import java.util.HashMap;
import java.util.Map;

@RestController
public class TestController {

    @RequestMapping(value = "/api", method = RequestMethod.GET)
    public Map<String, Object> getTest() {
        Map<String, Object> result = new HashMap<>();
        result.put("application_name", "testing");
        result.put("update_date", "2020.01.01");
        return result;
    }

}
```

요청할 URL 경로를 지정하고, 보낼 값 (여기서는 값을 지정했습니다.)을 HashMap 에 넣어서 json 형식으로 보내주게 할 수 있습니다.


## thymeleaf 템플릿 엔진

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.w3.org/1999/xhtml">
<head>
    <title>Title</title>
</head>

<body>
    <h1 th:text="${name}">Name</h1>
</body>
</html>
```

프로젝트 경로 src\main\resources\templates\index.html 파일을 만들고, name 이라는 변수를 바인딩할 수 있게 준비합니다.

```java
package com.tutorial.spring;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
public class PageController {
    @RequestMapping(value = "/home")
    public String index(Model model){
        model.addAttribute("name", "testing");
        return "index";
    }
}
```

페이지 컨트롤러를 작성하고, 미리 만들어둔 html 파일 이름을 반환 값으로 지정합니다.

속성을 추가하여 html 파일에 작성해둔 name 에 값을 바인딩해줄 수 있습니다. 

## Spring Boot 프로젝트 체험 소감

자바를 배우고 거의 처음으로 maven 을 실습하게 되었고, spring-boot-starter 로 예제 코드를 작성 해보았습니다.

비록 지금은 회사에서 파이썬을 위주로 일을 해서 Spring Boot 프로젝트를 언제 또 실습해볼 지 모르겠지만, 여러 분야를 지속적으로 겪어보는 건 언제나 재미있다고 느낍니다.