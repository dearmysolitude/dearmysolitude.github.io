---
title: "Spring 관련 용어"
excerpt: "Spring의 기본적인 개념과 용어들"

categories:
  - Spring
tags:
  - 개념
  - Concept
---
## Spring Framework

-   **Java 플랫폼을 위한 오픈소스 프레임워크**
-   **경량 컨테이너**: 자바 객체를 직접 관리하며, 객체 생성/소멸과 같은 라이프사이클을 관리한다.
-   **POJO(Plain Old Java Object)방식의 프레임워크**: 일반적인 J2EE 프레임워크에 비해 구현을 위한 특정 인터페이스를 구현하거나 상속 받을 필요가 없어 기존 라이브러리를 지원하기에 용이하고 객체가 가볍다.
-   **제어 반전(IoC: Inversion of Control)**: 컨트롤의 제어권이 사용자가 아니라 프레임워크에 있어, 필요에 따라 스프링에서 사용자의 코드를 호출한다(라이브러리와는 반대: 라이브러리의 경우 사용자의 코드가 필요에 따라 라이브러리의 코드를 호출한다).
-   **의존성 주입(DI: Dependency Injection)**: 각각의 계층이나 서비스들 간에 의존성이 존재할 경우 프레임워크가 연결시켜 준다.
-   **관점 지향 프로그래밍(AOP: Aspect Oriented Programming)**: 트랜젝션이나 로깅, 보안과 같이 여러 모듈에서 공통적으로 사용하는 기능의 경우 해당 기능을 분리하여 관리할 수 있다.

> 프레임워크란?  
> 복잡한 문제를 해결하거나 서술하는 데 사용되는 기본 개념 구조  
> 혹은, 소프트웨어의 구체적인 부분에 해당하는 설계와 구현을 재사용이 가능하게끔 일련의 협업화된 형태로 클래스들을 제공하는 것, 플랫폼을 위한 오픈소스 애플리케이션 프레임워크

## 참고 자료

- [\[Spring 레퍼런스\] 1장 스프링 프레임워크 소개 #1 :: Outsider's Dev Story](https://blog.outsider.ne.kr/729)
- [Java Spring Boot란? | IBM](https://www.ibm.com/kr-ko/topics/java-spring-boot)
- [Plain Old Java Object - 위키백과, 우리 모두의 백과사전 (wikipedia.org)](https://ko.wikipedia.org/wiki/Plain_Old_Java_Object)
- [스프링 프레임워크 - 위키백과, 우리 모두의 백과사전 (wikipedia.org)](https://ko.wikipedia.org/wiki/%EC%8A%A4%ED%94%84%EB%A7%81_%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC)
- [스프링(Spring) 프레임워크 기본 개념 강좌 (1) - 스프링 이해하기 (ooz.co.kr)](https://ooz.co.kr/170)
- [망규님 블로그](https://mangkyu.tistory.com/5)