---
title: "팀 프로젝트: Roadmaker"
excerpt: "GPT API를 활용한 로드맵 생성 서비스"
header:
  image: /assets/images/roadmaker/roadmaker-title.png
  teaser: assets/images/roadmaker/roadmaker-logo.png
sidebar:
  - title: "Role"
    image: /assets/images/roadmaker/roadmaker-logo.png
    image_alt: "logo"
    text: "Back-End Developer"
  - title: "Excerpt"
    text: "GPT API를 활용한 로드맵 생성 서비스"
gallery:
  - url: /assets/images/roadmaker/roadmaker-front.png
    image_path: assets/images/roadmaker/roadmaker-front.png
    alt: "목록 화면"
  - url: /assets/images/roadmaker/roadmaker-use.png
    image_path: assets/images/roadmaker/roadmaker-use.png
    alt: "사용 화면"
  - url: /assets/images/roadmaker/roadmaker-detail.png
    image_path: assets/images/roadmaker/roadmaker-detail.png
    alt: "세부 화면"
toc: true
toc_sticky: true
tag:
  - Krafton Jungle
---
## 프로젝트 개요 및 특징

GPT API를 사용하여 개발자를 위한 로드맵을 작성해주는 웹 서비스.

### Key Features

- GPT로 원하는 주제의 로드맵 초안을 작성
- 생성된 로드맵 초안 내용/스타일을 편집
- 로드맵 공유/구독, 댓글 기능
- 구독한 로드맵은 개인 저장소에 저장되며, 학습 진행 사항을 기록 가능

### 특징

- Github Actions, Nginx와 AWS의 Code Deploy를 사용한 Blue/Green 자동 배포 환경
- JWT token을 Spring Security와 연동/로그인 기능, Query DSL을 활용한 게시물 검색, 검색 결과 페이지네이션
- GPT API 멀티스레딩 호출로 내용 생성 시간 40% 감소
- 섬네일 이미지 온디맨드 리사이징 구현으로 이미지 리소스 절약

## 기술 스택

<figure style="width: 50%" class="align-center">
  <img src="/assets/images/roadmaker/roadmaker-architecture.png" alt="">
  <figcaption>Architecture</figcaption>
</figure>

- Language: Java 17
- Framework: Spring Boot 3.1
- DB: MySQL 8.0, JPA
- Build: Gradle 8.2
- IDE: IntelliJ 2023.2
- CI/CD: Github Actions/AWS CodeDeploy, S3, EC2
- Reverse Proxy: NginX

## 실제 서비스

{% include gallery caption="서비스 화면" %}


## 링크

[서비스 링크](http://roadmaker.site)

[Github 링크](https://github.com/road-maker)

[포스터: 구글 드라이브](https://drive.google.com/file/d/1fHiA1xgeR228jUKmHyoaYXZ9noIdgeF2/view?usp=drive_link)

<br>

<a href="#page-title" class="back-to-top">{{ site.data.ui-text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
