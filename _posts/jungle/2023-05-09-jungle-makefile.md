---
title: "리눅스에서 C/C++ 파일 빌드하기: Makefile"
excerpt: "Spring의 기본적인 개념과 용어들"

categories:
  - Jungle
tags:
  - makefile
---
참고1) : https://modoocode.com/311

참고2) : [https://velog.io/@hidaehyunlee/Makefile-자주-사용하는-문법-정리](https://velog.io/@hidaehyunlee/Makefile-%EC%9E%90%EC%A3%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%EB%AC%B8%EB%B2%95-%EC%A0%95%EB%A6%AC)

## make

- 리눅스 환경에서 컴파일을 진행할 때에는 어떤 파일들을 컴파일하고, 어떤 방식으로 컴파일할지 직접 컴파일러에게 알려주어야 한다.
- make 프로그램은 Makefile 파일을 읽어서 makefile내에 설정된 방법대로 명령어를 처리(컴파일)한다. 이로 큰 규모의 프로그램을 한 번의 명령을 통해 컴파일할 수 있게 된다.

![c&cpp program](https://1drv.ms/i/c/c4f97b3b64ae3e7a/IQTc_7OC4JNkTa_zTeFNm27XASI4lDg2w6o5smNa6XMXzAI?width=1024)

### C 프로그램 실행 과정

- 전처리기를 통해 주석이나 헤더를 불러온다.
- 컴파일러를 통해 어셈블리어로 번역
- .o 파일에는 컴파일된 어셈블리 코드가 들어있음
- 해당 목적코드만으로는 프로그램을 만들 수 없고, 외부 함수들을 가져와서 함수의 동작방법을 불러와야 함(Linker에 의한 linking)

어떠한 조건으로 명령어를 실행할지 담은 파일을 Makefile이라고 하며, make를 터미널상 실행하게 되면, 해당 위치에 있는 makefile을 불러와 명령을 실행한다.

## Make file

> The purpose of the make utility is to determine automatically which pieces of a large program need to be recompiled, and issue the commands to recompile them.

세가지 요소로 이루어져 있음:

1. target: 어떤 것을 make 할지 전달
2. recipes: 명령어를 쓸 때 반드시 탭 한 번으로 들여쓰기를 해줘야 한다.
3. prerequisites: 주어진 target을 make할 때 사용될 파일들의 목록 - 의존 파일(dependency)라고도 한다. 주어진 파일들의 수정 시간보다 타겟이 더 나중에 수정되었다면 타겟의 명령어를 시행하지 않는다. 

예를 들어, `main.o`의 생성 시간이 `main.cc`, `foo.h`, `bar.h`들의 수정시간보다 나중이라면 굳이 `main.o`를 다시 컴파일 할 필요 없음.