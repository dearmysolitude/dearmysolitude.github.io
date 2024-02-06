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
  - Front Controller Pattern
  - DispatcherServlet
---
## Spring MVC (Spring Module)

Spring에서 제공하는 웹 모듈로, Model, View, Controller 세가지 구성요소를 통해 다양한 HTTPS request를 처리하여 REST 뿐 아니라 view를 표시하는 html 도 리턴할 수 있는 framework이다. 

잠깐 Spring MVC에 대한 공식 문서의 설명을 살펴보고 본격적으로 왜 이런 프레임워크가 생겼는지 살펴보자. 그 과정과 Spring MVC에서 [Request lifecycle](MVC-request-lifecycle.md), [RESTfull API](/_posts/03-2.%20network/RESTfull.md)의 개념에 대해 살펴보고 나면 아래의 글을 더 쉽게 이해할 수 있을 것이다.

> The Spring Web model-view-controller (MVC) framework is designed around a DispatcherServlet that dispatches requests to handlers, with configurable handler mappings, view resolution, locale and theme resolution as well as support for uploading files. The default handler is based on the @Controller and @RequestMapping annotations, offering a wide range of flexible handling methods. With the introduction of Spring 3.0, the @Controller mechanism also allows you to create RESTful Web sites and applications, through the @PathVariable annotation and other features.
>
> [Web MVC Framework - Spring docs](https://docs.spring.io/spring-framework/docs/3.2.x/spring-framework-reference/html/mvc.html)

(해석) Spring Web model-view-controller 프레임워크는 DispatcherServlet을 중심으로 디자인 되었다. 이는 요청(request)를 handler에게 전달한다. handler는 사용자 설정 가능한 handler mapping / view resolution / locale /  theme resolution / 파일 업로드를 지원한다. 기본적인 handler는 `@Controller`와 `@RequestMapping` 어노테이션에 기반하며, 넓은 분야의 유연한 handling 메서드를 제공한다. Spring 3.0 에서는 `@PathVariable`어노테이션과 그 외 기능들을 통해, 이 `@Controller` 메카니즘을 사용하여 RESTfull 웹 사이트와 앱을 구축하는 것이 가능하게 되었다.

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

- 클라이언트로부터 요청이 들어오면 서버는 CGI를 구 현한 구현체에게 동적인 데이터를 제공해달라고 요청한다.
- CGI 모든 요청에 프로세스를 사용: 무겁다.
- 같은 요청에도 항상 CGI 객체를 생성하고 제거하는 과정이 필요했다.
즉 두 단점 모두 서버에 기능적인 부담을 주었다. 이에 따라 자바 진영에서는 Servlet이라는 개념이 등장하였다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216862&authkey=%21AKaqdS49OtiqFho&width=1828&height=694" alt="">
  <figcaption>Servlet 의 등장</figcaption>
</figure>

자바의 Servlet은 프로세스 대신 스레드를 사용하여 요청을 처리하며, 싱글톤 패턴을 적용하여 같은 요청에 대해서 효율적으로 처리하도록 하였다.

또한 Servlet은 **HTTP 규약에 맞는 요청을 처리하기 위해 일일이 파싱하지 않고, HTTP 메서드에 대한 분기 처리**를 쉽게 하는 많은 기능을 제공하고 있다. 개발자는 이러한 요청을 처리하는 메서드를 재정의하고, API를 사용하여 요청받을 URL을 매핑하도록 조작해주면 그에 따른 처리는 servlet이 알아서 해주는 것이다.

### Servlet Container

- Servlet의 관리를 담당: servlet lifecycle인 생성, 호출, 제거
- Servlet은 `init()`, `destroy()` 메서드를 통해 관리된다. Servlet Container는 이러한 Servlet의 관리를 담당한다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217006&authkey=%21ADNnwq6BfY1wn0A&width=720&height=271" alt="">
  <figcaption>Servlet Container</figcaption>
</figure>

- 요청이 들어오면 servlet container가 이러한 servlet을 생성하고 파괴하는 것이 아니라 가지고 있는다. 클라이언트 요청이 들어왔을 때, 요청과 매핑되어있는 servlet이 존재한다면 해당 요청을 처리하기 위해 다시 servlet을 호출하여 이를 재사용해 처리한다.

> Spring boot는 Tomcat 이라는 Servlet Container를 사용한다: 기본적으로 8080 포트를 사용.

### Servlet의 문제

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217007&authkey=%21AORHvv6gI82TPTc&width=1501&height=673" alt="">
  <figcaption>Servlet의 문제: 공통 로직의 반복 실행</figcaption>
</figure>
각 요청마다 Servlet이 1:1 대응되어 있어서 공통 로직은 요청이 발생할 때마다 계속해서 실행되어야 했다. 다음 코드는 그 예시이다.
<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217008&authkey=%21ACXz20yUC-18b0g&width=1069&height=957" alt="">
  <figcaption>공통 로직(하늘색 박스)과 메서드들이 servlet에 의존적인 것(붉은 박스)을 확인할 수 있다.</figcaption>
</figure>

정리하자면,

1. 각 Servlet의 service() 메서드에서 중복되는 부분이 많다.
2. 각 컨트롤러에서 공통으로 처리해야 하는 로직이 생기면 중복이 발생한다.
3. Servlet에 종속적인 구조를 가진다.

### Controller의 등장: Front Controller Pattern

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217011&authkey=%21AKqDj5eCzXZp5nY&width=1920&height=607" alt="">
  <figcaption>공통 부분을 뽑아내 Front Controller로 두었다.</figcaption>
</figure>

이로써 1번, 2번 문제점은 해소된다.

**Front Controller**
  1. 클라이언트 - 서버 연결
  2. 각 요청에 맞는 컨트롤러를 매핑하여 정보를 보관
  3. 요청이 들어오면 매핑 정보를 찾아 해당 컨트롤러 호출
  4. 전달할 결과를 생성
  5. 결과를 사용자에게 반환
  
  ✨ 분리한 건 좋은데 Front Controller의 일이 너무 많다!

### Front Controller의 고도화

Front Controller의 역할이 너무 많다는 것을 알 수 있다. 하나의 요소에 여러 기능이 집중된 탓이다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217012&authkey=%21ABwz6cwtgo465NQ&width=1229&height=1031" alt="">
  <figcaption>Front Controller의 세분화</figcaption>
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217013&authkey=%21APlTv-Ag93Bm5uc&width=1237&height=664" alt="">
  <figcaption>1. 핸들러(컨트롤러) 조회: 요청에 대한 핸들러를 찾는 검색</figcaption>
</figure>

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217014&authkey=%21ABYnPY8vrpib2v4&width=1233&height=936" alt="">
  <figcaption>2. Parameger, Model 전달 / 3. ViewName 반환</figcaption>
</figure>

Front Controller 가 객체를 만들어 요청할 정보를 전달받은 model과 함께 넘긴다. **Servlet이 매개 변수로 전달되는 것이 아니라 Java 객체가 전달된다**: 이로써 컨트롤러가 servlet에 종속되는 문제가 해결된다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217015&authkey=%21AB4c9brE9Z1_xS4&width=1228&height=727" alt="">
  <figcaption>4. ViewName과 일치하는 뷰를 검색 / 5. Model에 대한 데이터를 서블릿에 담아 응답</figcaption>
</figure>

viewResolver를 통해 주어진 viewName의 원본 view의 uri를 알아낸 후에 CustomView 객체에 렌더링을 맡기도록 지시한다.

일련의 책임 분리를 통해 객체지향적인 개발이 가능해졌다. 그리고 이러한 과정들을 거쳐 형성된 것이 Spring Web MVC Framework이다.

## Spring Web MVC

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217017&authkey=%21ACDhK3ddmpcIaV0&width=1665&height=865" alt="">
  <figcaption>Spring Web MVC</figcaption>
</figure>

앞서 살펴본 Front Controller 패턴과 다른 점은, Front Controller는 DispatcherServlet으로 바뀌었고, Controller(Handler)를 호출하기 전에 Adapter를 추가하여 다양한 종류의 Handler를 호출할 수 있도록 하였다(확장성 증가).

### Spring Web MVC의 이점

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%217016&authkey=%21ADY6JxjeXN_Shpg&width=1039&height=921" alt="">
  <figcaption>DispatcherServlet</figcaption>
</figure>

- Servlet WebApplicationContext: 웹 요청을 담당하는 빈(객체)이 들어가서 관리 됨
- Root WebApplicationContext: 서비스 계층, Repository등 웹 환경에 독립적인 빈이 들어가 관리 됨

웹 요청시, 필요한 부분을 DispatcherServlet이 알아서 주입하는 구조로 웹 요청이 동작하게 된다.
설정만 잘 해주면, 그 **설정대로 생성된 객체가 스프링 컨테이너 내부에서 관리되고, 필요한 부분에 주입을 받아 Dispatcher Servlet이 사용할 수 있게된다. 어플리케이션 단(컨트롤러 포함)에서도 Spring Web MVC를 활용하여 필요한 부분에 이를 적용할 수 있다.**

개발자는 Spring Web MVC에서 제공하는 DispatcherServlet 웹 요청 처리 관련 구현체를 사용하여, 개발자들이 서비스를 위해 개발해야 하는 핸들러, 즉 비즈니스 로직에만 집중할 수 있도록 하는 환경을 제공해 준다.

## 참고 자료

- [루키의 Servlet & Spring Web MVC - 테코톡](https://www.youtube.com/watch?v=h0rX720VWCg)


