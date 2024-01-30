---
title: "Spring Interceptor와 resolver"
excerpt: "Spring 애플리케이션에서 request의 lifecycle과 이를 활용한 사용자 정의 annotation의 작성: Spring interceptor와 resolver"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Interceptor
  - Resolver
---

## 문제 상황

Spring에서 api에 접근할 때 로그인 한 경우와 로그인 하지 않은 경우에 대해서 다른 처리를 해야 할 때, 처음에는 api를 분리해서 처리하였는데, 멘토님으로부터 api를 분리하는 기준은 사용자가 아닌, 해당 요청이 어떤 기능인지이므로 이런 식의 접근은 바람직하지 않다는 피드백을 받았다. 따라서, Spring에서 지원하는 사용자 정의 annotation을 통해 로그인한 유저와 아닌 유저를 구분하여 처리하도록 코드를 작성하였다.

그런데 아무래도 프로젝트 기간이 짧다보니 해당 기술에 대한 얕은 이해를 토대로 팀원의 코드에 의존하였다. 따라서 이를 이해하기 위해 Spring에서 요청에 대해 처리하는 기능인 Arguement Resolver와 Interceptor에 대해 알아보도록 한다.





## 참고 자료

- [Spring Arguement Resolver와 Interceptor - Tecoble](https://tecoble.techcourse.co.kr/post/2021-05-24-spring-interceptor/)