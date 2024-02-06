---
title: "Spring Boot의 빌드"
excerpt: "Spring boot가 제공하는 편의: Dependency와 프로젝트 빌드"

toc: true
toc_sticky: true

categories:
  - Spring
tags:
  - Spring Boot
  - Dependency
  - Gradle
  - Maven
  - Jar
---
## Dependency

- **Spring boot**에서는 Spring을 위한 dependency에 대해 버전 관리 기능을 제공한다.
- Spring 프로젝트에 직접 dependency를 설정할 때에는 버전을 명시해 주는 것이 좋은 습관이다.

## 애플리케이션 실행

### 빌드 툴로 빌드하기
`mvn spring-boot: run` / `gradle build` + `gradle bootRun`
- 스프링 부트 Maven/Gradle 플러그인 사용.
- 빌드 툴인 메이븐/그레이들을 사용해 실행.
- IntelliJ 같은 IDE를 사용하는 것이 아닌 소스 코드만으로 실행하는 것.

Maven/Gradle이 설치되지 않은 경우 maven wrapper/Gradle Wrapper 파일 이라는, Maven/Gradle 빌드 툴을 사용할 수 있는 스크립트 파일이 존재한다: `./mvnw spring-boot: run` & `./gradlew bootRun` 명령을 사용해 스크립트 파일을 실행하여 빌드, 실행할 수 있다.

### IDE에서 Main 클래스 실행
- IDE에서 메인 메서드를 실행

### JAR 패키징 & `java -jar`

- 스프링 부트 Maven/Gradle 플러그인을 사용해 특수한 실행 가능한 형태의 JAR 파일 만들 수 있다.
- 서버에 배포하는 용도로 사용
- Maven/Gradle의 Packaging 라이프사이클을 실행하면, 빌드 툴이 프로젝트를 하나의 Jar 파일로 만들어(패키징 해) 준다. 만들어진 Jar 파일은 target 디렉터리에 위치해 있다.
- 자바는 하나의 Jar파일로 뭉치는 스펙이 없다: Spring Boot의 기능.
  
  **Uber JAR, Fat-jar**
  
  - 의존성에 있는 모든 라이브러리를 포함한 JAR 파일을 의미한다.
  - 의존성에 있는 라이브러리, application bootloader(라이브러리 파일을 찾아서 실행할 수 있게 도와준다), 소스 코드를 포함한다.
  - `java -jar [파일명]` 명령어로 단독 실행할 수 있다.
  - 의존성의 라이브러리를 포함하지 않은 jar 파일은 plain jar라고 한다.

  > 참고: [Execuatble Jar와 Plain Jar](https://velog.io/@shawnhansh/SpringBoot-Executable-jar-plain-jar)

