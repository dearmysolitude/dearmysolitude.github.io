---
title: "자바 프로그램 실행과정: 간단히 / Java"
excerpt: "자바 프로그램의 실행과정을 간단하게 살펴본다."

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
JVM에 대해서는 참고자료의 아티클을 통해 조금 더 자세하게 정리해보도록 하자.``

# JVM Architecture diagram
![jvm-architecture](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216627&authkey=%21AJfB9y6x1lpOWrA&width=1155&height=650)
 
## 1. .java 파일의 컴파일: .class 파일

```
javac vendingMachine.java

javac vendingMachineMain.java
```

→ vendingMachine.java, vendingMachine.class / vendingMachineMain.java, vendingMachineMain.class 파일 생성

Class 파일은 바이트코드로 이루어져 있으며, 바이트코드는 플랫폼 독립적이므로, 플랫폼에 관계없이 일정하게 번역된다. 바이트코드는 바이너리 코드로 되어있지만, 소프트웨어 단에서 해석이 가능하므로 기계어보다는 추상화되어 있다.

생성되는 class 파일의 위치는 JDK의 기본설정(CLASSPATH 시스템 환경 변수)이나 javac 명령어 옵션을 통해 설정할 수 있으나, IDE를 사용하는 경우 통상적으로 IDE가 프로젝트 하휘에 있는 특정 폴더에 저장한다: IntelliJ 2023.2.2의 경우에는 build 폴더.

 
## 2. 컴파일된 파일의 실행(JVM)
![jvm-process](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216623&authkey=%21AK_6Ec3ONKHxLoI&width=615&height=109)
Working of Java Virtual Machine(JVM)

```
java vendingMachineMain
```

### 2-1. .class 파일을 실행

JVM은 CLASSPATH에 있는 .class 파일을 찾아 실행한다.

IntelliJ의 경우에는 build 패키지 아래에 있는 클래스 파일이겠다.

IntelliJ에서 class파일을 선택하여 Run 해보면 터미널에 java class 실행 명령어가 -classpath 옵션과 함께 실행되는 것을 확인할 수 있다.

JVM은 인터프리팅(Interpreting)/JIT 컴파일(Just-In-Time Compilation) 을 하여 바이트 코드를 읽어 해당 플랫폼의 기계어(바이너리 코드)로 변환하고 PERM이라고 하는 메모리 영역에 모든 클래스 정보를 저장한다.

- PERM 메모리 영역: JVM Architecture에서 Method Aread에 속해있다.
  - Java의 메모리 구조의 하나로, 주로 클래스의 메타데이터, 메서드의 메타데이터, 정적 변수, 상수 정보 등이 저장된다. 흔히 메타데이터 영역이라고 부른다.
  - 그러나 Java 8부터 PERM 영역이 제거되고, Metaspace가 도입되었다.
  - PERM은 지금은 삭제되었으므로 자세한 내용은 [JDK 8에서 Perm 영역은 왜 삭제됐을까 - 기계인간 John Grib](https://johngrib.github.io/wiki/java8-why-permgen-removed/) 에서 확인할 수 있다. → 상세한 내용은 자바의 메모리를 조금 살펴봅시다.

Interpreting: JVM이 한줄씩 코드를 읽어 즉시 실행하는 방식. 구현이 간단하고 플랫폼 독립적이지만, 실행 속도가 느리다.

Just in time compilation: JVM이 바이트코드를 처음부터 읽어 전체를 기계어로 변환한 후에 실행한다. 컴파일 시간이 필요하고 메모리를 많이 사용한다.

현대의 JVM은 두가지를 적절하게 혼합하여 사용한다. 자주 사용하는 파일은 JIT를 통해 빠르게 실행하고, 그렇지 않다면 interpreter를 통해 메모리를 절약한다. 결과적으로, 효율적인 실행능력과 높은 이식성을 동시에 보장할 수 있다.

PERM에 클래스 정보가 올라가게 되면, JVM은 해당 클래스가 어떤 static 메서드를 가지고 있는지, 인스턴스 메서드를 가지고 있는지 알게된다.

 
### 2-2. PERM의 main메서드 실행

JVM이 main 메서드(진입점)를 찾아 실행시킨다

1. Java stack의 stack entry에 main메서드가 올라간다.
    - stack entry에는 프로그램 카운터(PC register)가 존재하여 몇 번째 줄을 실행하고 있는지 JVM이 알고 있다.
    - 이 때 메서드로 전달되는 변수들도 같이 stack entry에 올라간다
      
      Java heap에 실제 변수인 String[] args 인스턴스가 생성되며, 스택 엔드리에 올라가는 메인 메서드의 변수는 이를 참조하도록 생성된다

2. 프로그램 카운터가 증가하며 다음 줄을 읽는다: 실행됨 메서드가 다시 java stack의 stack entry에 추가된다.
    - 해당 함수가 종료되면 스택에서 메서드들이 사라진다: LIFO
    - 메서드가 사라지면서 생성된 지역 변수들도 같이 사라진다.

 
# 참고자료

[JVM ARCHITECTURE. It is very important, as a programmer… by Deepti Swain / InterviewNoodle](https://interviewnoodle.com/jvm-architecture-71fd37e7826e)

[인프런 즐거운 자바 - 메서드가 실행될 때 어떤 일이 일어나는가](https://www.inflearn.com/course/lecture?courseSlug=%EC%A6%90%EA%B1%B0%EC%9A%B4-%EC%9E%90%EB%B0%94&unitId=145895)

[JDK 8에서 Perm 영역은 왜 삭제됐을까 - 기계인간 John Grib](https://johngrib.github.io/wiki/java8-why-permgen-removed/)