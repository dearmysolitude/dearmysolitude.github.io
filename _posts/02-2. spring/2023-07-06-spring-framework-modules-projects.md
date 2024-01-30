---
title: "Spring: Project, framework and modules"
excerpt: "프로젝트와 프레임워크, 모듈. 스프링 프레임워크는 왜 배울까?"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
---

- [Spring Boot 3 & Spring Framework 6 마스터 - Udemy](https://www.udemy.com/course/spring-boot-and-spring-framework-korean/learn/lecture/36692976#content)
- [강의 Github Repository](https://github.com/in28minutes/master-spring-and-spring-boot/tree/main/01-spring)

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216710&authkey=%21AFzAjlLQ9RbF3ZU&width=703&height=762" alt="">
  <figcaption>Spring Project는 어떻게 구성되어 있나</figcaption>
</figure> 

## Spring Framework

**의존성 주입**✨: Loose Coupling을 실현한다.

Just Dependency Injection is NOT sufficient (You need other frameworks to build apps)

→ Spring Modules and Spring Projects: Extend Spring Eco System

Provide good integration with other frameworks (데이터베이스와 통신하려는 경우: Hibernate/JPA, 유닛테스트를 작성하려는 경우: JUnit & Mockito for Unit Testing)

`@Component`, `@Autowired`, Component Scan etc..


## **Spring Core**
IOC Container, Dependency Injection, Auto Wiring, ..

These are the fundamental building blocks to: 

- Building web applications
- Creating REST API
- Implementing authentication and authorization
- Talking to a database
- Integrating with other systems
- Writing great unit tests

세가지 측면에서 스프링에 대해 살펴보자.

- **Spring Framework**
- **Spring Modules**
- **Spring Projects**

## Framework and Modules
스프링 프레임워크는 여러 개의 스프링 모듈로 이루어져 있다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216711&authkey=%21AD6mR94Q0mMjJ7Q&width=459&height=580" alt="">
  <figcaption>Spring Modules</figcaption>
</figure> 

- Fundamental Features: Core (IOC Container, Dependency Injection, Auto Wiring, ..)
- Web: Spring MVC etc (Web applications, REST API)
- Web Reactive: Spring WebFlux etc
- Data Access: JDBC, JPA etc
- Integration: JMS etc
- Testing: Mock Objects, Spring MVC Test etc

No Dumb Question: 왜 스프링 프레임워크는 여러개의 모듈로 나뉜걸까?
**Each application can choose modules they want to make use of, 프로젝트 만들때 모든 기능이 필요한건 아니니까.**

## Spring Projects

- **Application architectures evolve continuously, 필요에 의해 계속해서 발전한다.**
    
    Web > REST API > Microservices > Cloud > ...
    
- Spring evolves through Spring Projects:
    1. First Project: Spring Framework
    2. Spring Security: Secure your web application or REST API or microservice
    3. Spring Data: Integrate the same way with different types of databases : NoSQL and Relational
    4. Spring Integration: Address challenges with integration with other applications
    5. Spring Boot: Popular framework to build microservices
    6. Spring Cloud: Build cloud native applications

## Spring Framework를 사용함으로써 얻을 수 있는 이점은?

**Hierarchy: Spring Projects > Spring Framework > Spring Modules**

1. **Loose Coupling**: Spring manages creation and wiring of beans and dependencies
    - Makes it easy to build loosely coupled applications
    - Make writing unit tests easy! (Spring Unit Testing)
2. **Reduced Boilerplate Code**: Focus on Business Logic
    
    Example: No need for exception handling in each method!
    
    All Checked Exceptions are converted to Runtime or Unchecked Exceptions
    
3. **Architectural Flexibility**: Spring Modules and Projects
    
    You can pick and choose which ones to use (You DON'T need to use all of
    them!)
    
4. **Evolution with Time**: Microservices and Cloud
    
    Spring Boot, Spring Cloud etc!

## 왜 스프링 프레임워크를 배워야 하는가?

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216864&authkey=%21ACL0Ixx8omHV3Dw&width=324&height=253" alt="">
  <figcaption>Spring Framework:  </figcaption>
</figure> 

- Spring / Spring Boot 를 통해 다양한 어플리케이션을 만들 수 있다.
  - Web
  - REST API
  - Full Stack
  - Microservices

- Spring framework는 모든 core feature를 제공한다(위에서 살펴봤듯이).
  - Spring framework를 통해 Spring Boot에 대한 이해를 쉽게 할 수 있다.
  - 문제 상황에 대한 디버깅을 빠르게 해결하는 것을 돕는다.

[다음 글]({% post_url 2023-07-06-spring-framework-modules-projects %})에서 Spring Framework의 자세한 핵심 기능에 대해 예제 코드를 통해 이해해보자.
