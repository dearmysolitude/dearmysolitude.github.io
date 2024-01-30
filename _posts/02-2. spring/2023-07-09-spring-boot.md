---
title: "Spring Boot"
excerpt: "Spring과 Spring Boot는 뭐가 다른거지?"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Spring Boot
  - Spring
---

## Spring Boot - 왜 Boot?

Build PRODUCTION-READY apps QUICKLY. Spring Framework의 모듈화된 모듈들 중, 기본적인 설정을 제공.

- **기본적인 프로젝트(Starter Project)** - Make it easy to build variety of applications필요한 모든 의존성 가져옴

- **자동 설정(Auto Configuration)** - Eliminate configuration to setup Spring, Spring MVC and other frameworks!다른 프레임워크를 설정할 필요 없음. 클래스 경로에 있는 항목에 따라 디폴트 설정을 자동으로 제공한다.

- 비기능적 요구사항을 적용시킨다 (NFRs):
    - **Actuator**: Enables Advanced Monitoring of applications
    - **Embedded Server**: No need for separate application servers!
    - Logging and Error Handling
    - Profiles and ConfigurationProperties

- **web apps and REST API**에 대한 빌드를 단순화한다. (Only focuses on Web Apps and API) 웹 애플리케이션을 구조까지 작성하는 것은 매우 어려웠다(Tight coupling을 하였을 때).

`@Controller`, `@RestController`, `@RequestMapping("/courses")`

그리고 Spring MVC를 사용하면서 pom.xml, web.xml, application.properties에 여러 설정이 필요하다는 것을 확인 하였다(Spring과 Spring MVC를 사용한 간단한 애플리케이션의 구현에도).

⇒ 그래서 나온 **Spring Boot**는 프로덕션 환경에 사용 가능한 애플리케이션을 빠르게 빌드하도록 지원한다.

## Continue Here...(ing)

## 참고 자료

- [우아한테크세미나 - 백기선](https://www.youtube.com/watch?v=z0EaPjF3pCQ&t=162s)
