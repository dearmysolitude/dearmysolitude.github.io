---
title: "Spring Boot의 기능 2: 코드 지원"
excerpt: "Devtools, 자동 설정, 외부 설정 // 작성 중"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Spring Boot
  - Devtools 
---
## Devtools
- 개발 중에 뷰 리소스 또는 템플릿에 적용되는 캐시는 오히려 불편하다.
- 애플리케이션을 자주 재시작하게 된다.
- 웹 브라우저를 자주 리로딩하게 된다.

### View 리소스 / 템플릿 캐시 관리
- 프로그래밍을 하면서 다양한 view 파일을 보게될 것이다. 빌드 후 보여지는 내용을 수정하려고 view 파일을 수정해도 수정이 적용되지 않는다(IDE를 종료해야 한다).
- Thymeleaf는 view 파일을 Spring MVC를 통해서 변환하는 과정을 거친다.
- 내부적으로 view를 이미 Tymeleaf가 캐싱하고 있어서 다시 빌드해도 view에 수정된 코드 내용이 반영되지 않는다.
- application.properties에서 설정을 바꾸어도 되지만: `spring.thymeleaf.cache=false`
- 이러한 세부 설정 없이 Devtools를 의존성에 추가하면 IDE를 종료할 필요 없이 빌드만 다시 해주어도 Thymeleaf가 새롭게 렌더링 한다.

>**Thymeleaf**
>
>Java 웹 및 독립 어플리케이션용 서버 사이드 템플릿 엔진. Spring 프레임워크에서 사용되며, 런타임 시 HTML 컨텐츠를 생성하는데 사용된다. HTML 코드에 특정 Thymeleaf 속성을 포함시켜 작성하는 자연젹 템플릿 언어를 제공하여 동적이고 데이터 기반으로 HTML 페이지를 생성할 수 있다.

### 코드 변경
- 컨트롤러(핸들러)가 추가되었을 때에(코드가 변경된 경우) Devtools는 최소한의 부분만 [ApplicationContext]({% post_url 2023-07-08-spring-framework-with-example %})를 다시 만든다.
- 애플리케이션을 재시작할 필요 없이 빌드를 다시 함으로써 변경된 코드 내용을 어플리케이션에 반영할 수 있는 것이다.
- Application 재기동 트리거는 빌드이다.
- Watcher는 특정 디렉터리의 파일들을 감시한다. 이 파일들이 변경시 application이 재기동 한다.
- 코드 변경이 일어난 경우, 클래스 파일을 새롭게 컴파일하여 기존의 클래스 파일을 새로운 파일로 덮어씌워진다. Watcher는 클래스 파일의 변경을 감지하고 재기동 되는 것이다.

### 웹브라우저 라이브 리로드

라이브 리로드: Devtools는 브라우저 리프레시 할 필요없이 자동으로 플러그인을 통해 뷰파일이 변경되면 새로고침 해 준다.

~~사실 잘 안됨~~

## 배포할 때에는 이 라이브러리를 어떻게 하지?
그렇다면 개발에 도움을 주는 라이브러리는 빼고 패키징 해야할까?

패키징을 한 상태에서 애플리케이션을 실행할 때에는 Devtools 설정들이 기본적으로 무시되므로 그냥 실행해도 상관 없다. Jar에서 실행할 때에느 Devtools의 설정들을 사용하고 싶다면, 그 때 추가 설정을 해야한다.

## 자동 설정