---
title: "Java Lab: 미니 프로젝트"
excerpt: "개념 적용 미니 프로젝트 묶음"
header:
  teaser: /assets/images/java-lab/mm_input.png
sidebar:
  - title: "Java Lab"
    image: /assets/images/java-lab/mm_input.png
    image_alt: "logo"
    text: "개념들을 적용해보는 미니 프로젝트 묶음입니다."
  - text: "[Github Repository](https://github.com/dearmysolitude/java-lab)"

  - title: "1. 회원 관리"
  - text: "**특징:** 객체 직렬화를 통해 입력/출력을 간단하게 처리, Interface 적용 리팩터링 / [강좌](https://www.youtube.com/watch?v=HEsAMjd8zpo)"
  - text: "**기한:** 2024년 01월 03일 ~ 2024년 01월 04일"

  - title: "2. 채팅"
  - text: "**특징:** 채팅 서버에 접속하여 클라이언트들이 동시에 채팅을 주고받는 프로그램 / [강좌](https://www.youtube.com/watch?v=_23srXUbhz0&t=81s)"
  - text: "**기한:** 2024년 01월 12일 ~ 2024년 01월 13일"
gallery:
  - url: /assets/images/java-lab/mm_input.png
    image_path: assets/images/java-lab/mm_input.png
    alt: "코드 1"
  - url: /assets/images/java-lab/mm_output.png
    image_path: assets/images/java-lab/mm_output.png
    alt: "코드 2"
toc: true
toc_sticky: true
tags:
  - 즐거운 자바
  - 객체 직렬화
  - 네트워크 프로그래밍
  - 스레드
  - 인터페이스
  - Java
  - 자바
  - 미니 프로젝트
---
## 개발 도구

- Java 17
- Gradle 8.4
- IntelliJ 2023.3

## 1. 회원 관리 프로그램

- [README.md](https://github.com/dearmysolitude/java-lab/blob/main/document/Membership-Management.md)

### 구현: DAO와 Service 인터페이스의 적용

**UserDAO ✨**

- 프로그램에서 파일시스템에 User 정보를 저장하고 읽기 위해 구현. 저장된 데이터는 프로그램으로 불러와 메모리 상에서 조작하도록 하였다.
- 즉, User 정보에 접근하는 DAO는 파일로부터 **객체 직렬화를 통해 메모리에 데이터를 읽어오거나 쓰는 역할**을 한다.

> **DAO(Data Access Object)**
>
> 데이터베이스의 데이터에 접근하기 위한 객체. 데이터베이스에 직접 접근하여 데이터를 삽입, 삭제, 조회하는 역할을 수행한다. 데이터베이스와의 상호 작용을 추상화하여 비즈니스 로직에서 데이터베이스를 직접 접근하는 것을 방지하고, 응용 프로그램의 유지 보수성과 확장성을 향상시킨다.

**UserService / UserServiceInMemory ✨**

- 메모리에 로드된 데이터를 **비즈니스 로직으로 조작**하는 역할을 한다.
- 처음부터 구현하지는 않았음: User를 관리하는 코드의 반복을 줄여 유지 보수를 위하여 분리하여 구현함.
- 파일에서 인메모리로 가져온 데이터를 다루기 위해 가져온 List<User>를 조작하기 위한 메서드 집단
- 파일 대신 데이터베이스나 네트워크 상의 데이터를 조작할 때에도 DAO로 불러오기만 한다면 유용하게 사용할 수 있다.

## 2. 채팅 프로그램

- [README.md](https://github.com/dearmysolitude/java-lab/blob/main/document/Chating.md)

### 기능 요구사항

1. 나의 [입력]과 상대편의 [입력 → 화면 출력] 동시 발생

   내가 어떤 정보를 입력하고(혹은 하지 않고) 있을 때(동시에), 다른 사람이 보낸 데이터가 내 화면에 출력되어야 한다.

   = 다른 사람이 보낸 메세지를 읽어들여 화면에 출력 + 나의 메세지 입력 → Thread로 구현

2. 사용자가 명령어를 입력하면 채팅 서버에 접속한다.

   이 때 서버는
    1. 접속을 대기
    2. 클라이언트가 접속하면 Socket이 반환
    3. A 클라이언트가 보낸 "안녕하세요"를 읽어들인다.
    4. 서버에 접속하고 있는 모든 클라이언트에게 "안녕하세요" 메세지를 전송한다: 브로드캐스팅

3. 채팅 프로그램
   1. 1 단계: 채팅 대기방
      1) `java 패키지명.ChatClient 닉네임`으로 서버에 접속
      2) 접속한 모든 사용자에게 "홍길동님이 접속하였습니다."
      3) 채팅을 입력한 후 [enter]를 입력하면 모든 사용자(나 포함)에게 내 메세지가 전달
      4) /quit을 입력하면 연결이 끊어진다. 연결이 끊어질 때 모든 사용자에게 "홍길동님이 대화방에서 나갔습니다." 출력
      5) 강제로 연결을 끊었을 때(프로그램 강제 종료)에도 동일하게 출력
   2. 2 단계: 채팅방, 로비 추가


### 구현

## 서버

### 멀티 스레드

- 사용자가 접속할 때마다 클라이언트와 연결되는 Socket을 가지고 있는 Thread가 생성된다(ChatThread).
- 클라이언트와 통신: 클라이언트가 10 개면, ChatThread 객체도 10 개
- 클라이언트 접속이 끝어지면 대응하는 ChatThread도 종료됨
- ChatThread는 연결된 클라이언트들로부터 입력 값을 전달받는 역할을 한다: ClientThread가 Socket을 통해 전달받은 값은 공유 객체를 통해 채팅방 클라이언트들에게 메세지를 전달한다.

### 공유 객체

- 클라이언트는 멀티 스레드로 작동하고, 이 클라이언트들과 연결되어 있는 ClientThread들은 채팅방마다 공유되는 공유 객체를 가진다. 이는 입력 값을 같은 채팅방에 존재하는 모든 클라이언트에게 메세지를 전달하기 위해 사용한다.
- 1 단계에서는 ChatThread를 생성할 때 List<PrintWirte>를 공통으로 두어 출력시 모든 PrintWriter를 순회하며 하나의 클라이언트에서 BufferedReader로 입력한 값을 출력하게 하였으나,
2 단계에서는 Client들이 공통으로 가지는 필드를 List<ChatThread>로 하여 구현한다(즉 클라이언트가 자기 자신들의 리스트를 가진다).

## 클라이언트

### 입출력

- 접속을 시도하는 클라이언트들은 하나의 스레드로 실행한다.
- 사용자로부터의 입력 / 클라이언트가 읽어들인 값을 서버로 전송(출력) / 키보드로부터 입력 의 총 세가지 입출력을 관리한다.
- 서버의 스레드와 Socket으로 연결된다.
- 1 단계에서는 클라이언트 실행시 닉네임을 환경 변수로 전달받도록 구현하였다.


