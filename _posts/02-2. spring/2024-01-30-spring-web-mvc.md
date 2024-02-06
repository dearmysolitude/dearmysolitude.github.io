---
title: "Spring Web MVC의 등장 배경"
excerpt: "정적 컨텐츠에서 Servlet, Spring Web MVC 까지"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Spring Web MVC
  - Servlet
  - Servlet Container
---
## Spring MVC (Spring Module)

- Spring에서 제공하는 웹 모듈로, Model, View, Controller 세가지 구성요소를 통해 다양한 HTTPS request를 처리하여 REST 뿐 아니라 view를 표시하는 html 도 리턴할 수 있는 framework이다.

> The Spring Web model-view-controller (MVC) framework is designed around a DispatcherServlet that dispatches requests to handlers, with configurable handler mappings, view resolution, locale and theme resolution as well as support for uploading files. The default handler is based on the @Controller and @RequestMapping annotations, offering a wide range of flexible handling methods. With the introduction of Spring 3.0, the @Controller mechanism also allows you to create RESTful Web sites and applications, through the @PathVariable annotation and other features.
>
> -(Web MVC Framework - Spring docs)[https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/mvc.html]

Spring Web model-view-controller 프레임워크는 DisPatcherServlet을 중심으로 디자인 되었다. 이는 요청(request)를 handler에게 전달한다. handler는 사용자 설정 가능한 handler mapping / view resolution / locale /  theme resolution / 파일 업로드를 지원한다. 기본적인 handler는 `@Controller`와 `@RequestMapping` 어노테이션에 기반하며, 넓은 분야의 유연한 handling 메서드를 제공한다. Spring 3.0 에서는 `@PathVariable`어노테이션과 그 외 기능들을 통해, 이 `@Controller` 메카니즘을 사용하여 RESTfull 웹 사이트와 앱을 구축하는 것이 가능하게 되었다.

## Servlet

동적 데이터를 제공하기 위해 CGI를 기반으로 제작된 프로그램. 

- 초창기의 웹은 정적 컨텐츠만을 제공 가능하였다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216861&authkey=%21AB2PPLezu_x07k0&width=684&height=347" alt="">
  <figcaption>초창기 웹 서비스: 정적인 데이터만 처리할 수 있다.</figcaption>
</figure>

- 정적 컨텐츠만을 제공할 수 있는 것을 동적으로 처리하기 위해 CGI(Common Gate Interface)가 도입되었다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216860&authkey=%21AIaD-4v-XqzCqZw&width=820&height=255" alt="">
  <figcaption>CGI</figcaption>
</figure>

- 클라이언트로부터 요청이 들어오면 서버는 CGI를 구 현한 구현체에게 동적인 데이터를 제공해달라고 요처한다.
- CGI 모든 요청에 프로세스를 사용: 무겁다.
- 같은 요청에도 항상 CGI 객체를 생성하고 제거하는 과정이 필요했다.
즉 두 단점 모두 서버에 기능적인 부담을 주었다. 이에 따라 자바 진영에서는 Servlet이라는 개념이 등장하였다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216862&authkey=%21AKaqdS49OtiqFho&width=1828&height=694" alt="">
  <figcaption>Servlet 의 등장</figcaption>
</figure>

자바의 Servlet은 프로세스 대신 스레드를 사용하여 요청을 처리하며, 싱글톤 패턴을 적용하여 같은 요청에 대해서 효율적으로 처리하도록 하였다.

또한 Servlet은 HTTP 규약에 맞는 요청을 처리하기 위해 일일이 파싱하지 않아도 되도록 API를 통해서 제공받을 수 있도록 많은 기능을 제공하고 있다. 개발자는 이러한 요청을 처리해주는 메서드를 재정의하고, API를 사용하여 요청받을 URL을 매핑해주도록 조작해주면 그에 따른 처리는 servlet이 해주는 것이다.


### Servlet Container

- Servlet의 관리를 담당
- Servlet읜 `init()``, `destroy()` 메서드를 통해 관리된다. Servlet Container는 이러한 Servlet의 관리를 담당한다.
- 요청이 들어오면 servlet container가 이러한 servlet을 생성하고 파괴하는 것이 아니라 가지고 있다가 다시 요청이 들어왔을 때 해당 요청을 처리하기 위해 다시 servlet을 호출한다.

>> Spring boot는 Tomcat 이라는 Servlet Container를 사용한다: 기본적으로 8080 포트를 사용.

### Servlet의 문제: Front Controller의 등장

각 요청마다 Servlet이 1:1 대응되어 있어서 공통 로직은 요청이 발생할 때마다 계속해서 실행되어야 했다.

## 이어서

이 글을 이해했다면 [다음 글]({})에서 Spring이 요청을 어떻게 처리하는 것에 대한 이해가 쉬울 것이다.

## 참고 자료

- [루키의 Servlet & Spring Web MVC - 테코톡](https://www.youtube.com/watch?v=h0rX720VWCg)


