---
title: "Spring, Spring Boot, Spring MVC의 이해"
excerpt: "Spring, Spring MVC 그리고 Spring Boot"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
---
# Understanding Spring Boot, Spring MVC vs Spring

## Spring Framework

**의존성 주입**✨

Just Dependency Injection is NOT sufficient (You need other frameworks to build apps)

→ Spring Modules and Spring Projects: Extend Spring Eco System

Provide good integration with other frameworks (데이터베이스와 통신하려는 경우: Hibernate/JPA, 유닛테스트를 작성하려는 경우: JUnit & Mockito for Unit Testing)

`@Component`, `@Autowired`, Component Scan etc..

## Spring MVC (Spring Module)

**web apps and REST API**에 대한 빌드를 단순화한다. (Only focuses on Web Apps and API)
Building web applications with Structs was very complex(Tight coupling을 하였을 때)
`@Controller`, `@RestController`, `@RequestMapping("/courses")`

그리고 앞서 Spring MVC를 사용하면서 pom.xml, web.xml, application.properties에 여러 설정이 필요하다는 것을 확인 하였다(Spring과 Spring MVC를 사용한 간단한 애플리케이션의 구현에도).

⇒ 그래서 나온 **Spring Boot**는 프로덕션 환경에 사용 가능한 애플리케이션을 빠르게 빌드하도록 지원한다.

- 좀 더 정확하게 정의를 서술하자면, Spring에서 제공하는 웹 모듈로, Model, View, Controller 세가지 구성요소를 통해 다양한 HTTPS request를 처리하여 REST 뿐 아니라 view를 표시하는 html 도 리턴할 수 있는 framework이다.


## Spring Boot (Spring Project)

Build PRODUCTION-READY apps QUICKLY. Spring Framework의 모듈화된 모듈들 중, 기본적인 설정을 제공.

- **기본적인 프로젝트(Starter Project)** - Make it easy to build variety of applications필요한 모든 의존성 가져옴

- **자동 설정(Auto Configuration)** - Eliminate configuration to setup Spring, Spring MVC and other frameworks!다른 프레임워크를 설정할 필요 없음. 클래스 경로에 있는 항목에 따라 디폴트 설정을 자동으로 제공한다.

- 비기능적 요구사항을 적용시킨다 (NFRs):
    - **Actuator**: Enables Advanced Monitoring of applications
    - **Embedded Server**: No need for separate application servers!
    - Logging and Error Handling
    - Profiles and ConfigurationProperties